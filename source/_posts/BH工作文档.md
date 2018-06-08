---
title: BH工作文档
date: 2017/9/22 10:46:25
---

…
<!-- more -->

##### STBCGI_YW 以及 STBTransfer 项目编译

头文件包含丢失，查阅并且环境设置。
包括项目本身路径/￥PROJECT_PATH￥/STB_CGI_YW
以及工具库BCMLib/Head/中的常规调试库以及网络库
BCM7251s下的编译用库

eclipse 3.8 juno不太稳定，down了三次了
不是用户官方的arm-linux-gnueabihf工具，使用顾那边拖来的库
改用新版的eclipes （4.7 oxygen），老版的cdt存在问题，不识别新的工程。
弄清了关于新版cdt工程的一些问题：
 .metadata 中保存了工程的本地临时配置和中间文件
.cproject/.project文件中保存了主要工程的配置

在执行可执行文件时遇到找不到路径的情况，尝试了权限，用户等问题以后尝试了android的依赖库缺失的问题
找到了16.04 Android编译的依赖库安装帖子，安装了大量android编译依赖的库
执行相关的android执行文件编译器通过

配置了C/C++ BUild的配置中的Path，改成自己的ToolChain/BCM72515/build
/bin/目录，确认能够找到交叉编译的工具链。

工程正常编译



#####  工程业务流程理解

文件夹的工程管理中.gitignore中忽略了Release（生成结果），RemoteSystemsTempFiles（中间文件），
.metadate (eclipse本地工程保存的文件)

先忽略这三个文件，其他部分为源码或执行脚本

常规的流程为使用eclipse打开工程，其中主要是STBCGI_YW工程，STBTransfer工程
这两个工程是主要的生成工程，主要的业务执行流程和网端逻辑都在里面。

>注意点：使用了一段时间以后还是觉得用eclipse+cdt这样冗杂的ide编译这样精简的小项目很废，所以我尝试使用sublime text加插件和编译系统的方式去编译这一过程，最后在系统里也集成脚本的使用。

编译完毕的两个工程的文件都放在自己对应目录的文件夹名字（xxxx）Release内，工程中也仅存在一个编译配置Release。

ToolChain里面是编译使用的环境以及工具库

Resource文件夹内是资源文件及界面的数据驱动文件

经过执行RecreateResource.sh脚本可以通过STBTool已经编译完毕的界面定义工具来得到最后的界面定义结果Skin_chs.IMG

完毕之后使用Release.sh脚本来将包括生成的Release骨架在内的，库文件，执行文件统一拷贝到Release文件夹内，根据编译的平台是否连接u盘或者工厂来决定是否执行脚本link2copy.sh，如果两者都不是则说明在本地，那么app.sh 和 STBCfg.xml就没有意义

link2copy脚本执行后发现是一个规律的保留名字最长的同前缀库的方案，使用新的更长字段的库确保功能更新

release文件夹下假设是udisk配置下生成的则会存在两个文件夹，两个文件，分别是，app.sh  STBCfg.xml
前者定义了在板子上app启动运行的主要过程，后者定义了板子运行时需要通过远程的服务器验证获取相关的文件进行相关的验证信息，所以需要有一个默认的

完毕以后使用Release的不同配置能够得到不同的Release的安装目录版本，使用 udisk产生一个放置在u盘app文件夹下的U盘内运行版本
目录结构为[U盘目录]/app/[release 下的所有文件]
启动盘制作完毕，需要设置板子到初始化状态，先要搭建板子的开发环境
板子电路元件面朝自己，usb等接口朝上，右侧的hdmi为输出口左侧的为输入口，左侧的串口需要转usb连接到自己的开发机上，网线需要连接在开发机同一个局域网域名内并确认ping通
hdmi输出口连接显示器，另外在开发板上插上鼠标操作

开发机需要通过minicom来与开发板进行串口的交互调试：
sudo apt-get install minicom 下载安装到本机
sudo minicom -s 来进行初始化的第一次使用配置
中间设置串口波特率115200， 8N1 （8位传输无校验1位停止位）
重启板子，确认minicom上显示正常以后，进入stb/config/app/目录下，将serverip.dat文件删除 改动 STBCfg.xml文件中的最后一个属性ISVERIFIED 为“0”
来使运行脚本时根据u盘重新生成服务器ip地址

插上制作好的U盘重启会自动进入配置ip的画面，配置到搭建的nfs服务器然后认证确定就会再次重启，默认使用此ip通过nfs加载app下文件，假设没有nfs那么自己搭建一个

nfs搭建步骤：
16.04LTS
sudo apt-get install nfs-kernel-server
vim /etc/exports
在最下方添加固定写死在脚本中的路径/root/STBVerify/Program *(rw,sync,no_root_squash,no_subtree_check)加载远程的app主体文件以及/root/STBVerify/Private *(rw,sync,no_root_squash,no_subtree_check)来传送本地log到远端服务器

然后在自己的开发电脑中将编译且处理好的当前的两个文件夹拖至指定位置,
最后运行sudo /etc/init.d/rpcbind restart进行rpcbind重启
sudo /etc/init.d/nfs-kernel-server restart进行nfs服务重启

之后sudo /etc/init.d/nfs-kernel-server status查看运行状态
showmount -e查看提供挂载服务的文件夹列表
本地尝试用 sudo mount -t nfs localhost:/root/STBVerify/Program/testDir来挂载文件夹(testDir 需要自己创建)，尝试成功以后可以用 umount /testDir来解除挂载

之后就可以通过mount -t nfs -o nolock -o rw [开发机的ip地址]:/root/STBVerify/Program 来 在开发板上尝试挂载nfs系统，

>注意点：我的电脑在执行脚本期间遇到了mount失效的情况，是由于udhcpc服务没有分配成功局域网ip地址导致的，在脚本中我sleep 3 来延迟了启动，udhcpc服务正常运行。

##### 程序在板子上和服务器上运行起来的大致流程

启动板子后板子上 /stb/app下的app.sh脚本会作为程序入口点运行，先后检查usb,hardisk,app,等然后执行检查到的相应目录中的app.sh 中的脚本，

以usbdisk的脚本为例，先后做了udhcpc的ip分配

判断/app目录下是否存在数据，假设存在那么就会直接执行本地的ktv.sh脚本

假设没有serverip.data文件则说明第一次运行。

直接从本地执行找到运行app.sh的当前目录下找到两个文件夹使用，并进入连接设置画面

否则从远端的服务器上进行mount来获取文件，到本地的tmpapp目录中；将日志文件替换放置到/pravite/【本机 mac 地址】中传到服务器上，再挂断加载private。
执行ktv.sh进入常规界面。

将三个主程序运行的目录进行权限提升然后启动lighttpd接收数据
关闭打开的udhcpc使用nexus加载android系统需要的一些基础配置，然后调用STBTransfer和STBCGI_YW文件来加载界面，网络交互。

##### 尝试更新在板子上的程序的流程注意点

>尝试在板子上使用新编译出来的Program，按照第一次的部署流程来做发现并没有能够及时更新，最后发现linux的缓存机制起到了作用，在板子断电以后没有保存新写入的一份文件，而用了原来的一份，解决办法是插上新的u盘并使用telnet host 来连接调试用板子。输入sync来确定缓存写入到硬盘。


>telnet TARGET_DEVICE_IP  在板子启动以后板子启动了telnet服务器线程， 
最高权限，密码为root，


##### STBTransfer 代码翻看

调试和审查代码及其配置我在本地开了一条分支forTest，用于自己修改玩耍。

从KTVMain.cpp中找到程序入口点，int main()函数

LOGMSG 层级的log输出工具，猜测android gdb实现

读取板子上的固定位置的STBCfg.xml文件确定默认的ip配置和显示

CKTVConfig类管理读取到的信息来完成第一幅配置画面的数据驱动

NativeStart函数来初始化BaseApp基础消息处理类，确定显示字体

创建并初始化全局的手写处理，页面管理器，播放控制等等全局UI类实例
最后起消息响应架构的循环的主线程。

在main函数中用NativeOnStep函数执行E3dopenGL的帧渲染线程操作并记录执行时间



待续未完..........






























































