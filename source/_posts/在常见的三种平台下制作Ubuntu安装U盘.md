
---
title: 在常见的三种平台下制作Ubuntu安装U盘
date: 2017/10/22 10:46:25
---

# 在常见的三种平台下制作Ubuntu安装U盘

![](http://image.techweb.com.cn/upload/roll/2015/11/20/201511201399_8979.jpg)

<!-- more -->
很多朋友都知道Ubuntu是一个非常不错的Linux发行版，要在官网下载到Ubuntu也非常简单。但下载好ISO之后我们要怎么来安装呢？当然，早年前我们都是通过记录DVD光盘的方式来进行安装，现在随着光驱逐步被市场所淘汰，Ubuntu同Windows一样与时俱进，同样也可以通过制作Ubuntu安装U盘的方式来进行安装。
下面我们就来介绍下如何在Windows、Mac甚至Linux平台下，如何制作Ubuntu安装U盘的几种方式。

Ubuntu版本的选择
首先大家需要知道Ubuntu有LTS版本和“技术前沿版”，这两种版本都可以作为日常的桌面终端进行使用，但通常我们会认为LTS版本更加稳定， 而且可以获得至发行之日起为期5年的技术支持。而LTS版本之间发行的所谓“技术前沿版”仅有9个月的支持周期，到期之后用户就必需升级到新的版本下。

再有就是32位和64位版本选择的问题。我个人比较建议大家都选择目前较主流的64位版本进行安装，当然，如果你的电脑太老旧或不能支持的话，还是安装32位吧！之前有一个比较流行的说法是内存小于3GB时就不要选择64位版本进行安装，其实这种说法可以忽略不计，64位可以更加充分的利用CPU支持，哪怕你的内存小于3GB。

无论你选择什么版本，都可以到官方网站进行ISO安装镜像的下载：Ubuntu官方镜像下载
制作Ubuntu安装U盘

一旦Ubuntu的ISO下载安装，我们就需要将其写入到U盘当中。其实无法你在哪种操作系统中制作Ubuntu安装U盘的方式都大相径庭，下面我们就分别进行介绍。
1.Windows中制作Ubuntu安装U盘
Universal USB Installer是一个Windows下制作Linux安装U盘非常流行和常用的一个工具，该工具是绿色版本不需要安装，支持当前主流的Linux发行版，当然也支持Ubuntu。
![](http://image.techweb.com.cn/upload/roll/2015/11/20/201511207398_7246.jpg)
打开Universal USB Installer，之后我们只需按上图所示选择好下载到Ubuntu镜像，再指定好我们当前U盘的盘符即可。为了保证操作过程中不出问题，建议大家勾选对U盘进行格式化。
Universal USB Installer下载
2.Mac中制作Ubuntu安装U盘
在Mac下制作Ubuntu安装U盘对很多普通用户来说就比较棘手了，因为我们必需用到Mac的终端命令。当然好处就是不用下载那些杂七杂八又不常用的工具来占用空间了。
打开终端，使用如下命令
![](http://image.techweb.com.cn/upload/roll/2015/11/20/201511208542_8942.jpg)
先浏览到下载文件夹：
cd ~/Downloads
然后执行如下命令：
hdiutil convert -format UDRW -o ubuntu.iso ubuntu-xxxxxx.iso
最后一部分是你下载好的Ubuntu镜像的文件名，请执行前按你的情况替换好。该命令可以将ISO镜像转换成Mac更容易地实现。
再执行，删除Mac版为镜像文件添加的.dmg扩展名：
mv ubuntu.iso.dmg ubuntu.iso
下一步列出当前驱动器：
diskutil list
然后插入U盘重新执行以上命令：
diskutil list
找出之前没有的驱动器挂载点后执行：
diskutil unmountDisk /dev/diskN
其中N是上条命令中找出的U盘挂载点号。
执行如下命令开始写入Ubuntu镜像文件到U盘：
sudo dd if=./ubuntu.iso of=/dev/rdiskN bs=1m
写入完成后，我们执行如下命令弹出U盘就制作完成了：
diskutil eject /dev/diskN
3.Linux中制作Ubuntu安装U盘
Linux下制作Ubuntu安装U盘的方式与Mac类似，都是通过终端命令来完成：
![](http://image.techweb.com.cn/upload/roll/2015/11/20/201511206352_3159.jpg)
先浏览到下载文件夹：
cd ~/Downloads
然后使用如下命令开始写入：
sudo dd if=./ubuntu-iso-name.iso of=/dev/sdX
其中X为U盘的挂载点，当然ubuntu-iso-name表示的是下载好Ubuntu镜像的名称，需要你自己改好。
制作完成后使用如下命令推出U盘即可：
sudo eject /dev/sdX
小结
以上我们介绍了3种制作Ubuntu安装U盘的方式，相信大家按步骤来都可以制作完成。如果遇到问题可以留言，我们一起来看看怎么解决。