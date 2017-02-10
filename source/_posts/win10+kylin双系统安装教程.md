---
title: 折腾uefi下win10与Ubuntu 16.04 LTS双系统，完美兼容grub2引导
---


## 折腾uefi下win10与Ubuntu 16.04 LTS双系统，完美兼容grub2引导

---

<!-- more -->
实际上会有茫茫多的朋友怕麻烦直接选择虚拟机或者wubi方式，个人认为以上两个方式不能算作双系统，更确切的说是主副系统，副系统性能会有很大的瓶颈，虽然使用无障碍，但反过来说因为简单也不需要开帖子讲了。
所以我当然是u盘镜像安装的办法。

## 安装win10
没话说，最靠谱的是大白菜工具  
[大白菜u盘制作官网](http://bd.dabaicaipe.cn/)
找个8g的U盘做win10安装盘，镜像可以选微软官方的，也可以雨林木风都没有问题，做成uefi格式的。

一般来说用户已经有win10了，需要保证的是win10一定得在unbantu之前安装。
win10系统版本方面不存在特别大的问题。我使用的是专业版，但其他的系统也做过测试。

开始下面几部之前
请做好备份！
请做好备份！
请做好备份！
重要的事情说三遍。

## 安装Ubuntu Kylin 16.04 LTS

### 需要软件及下载
[官方镜像](http://cn.ubuntu.com/download/)
选好32位或者64位，看好自己的内存，大于8G的推荐64

用麒麟的话美化的和中文化也都不错，选原版也可以自己配置，个人觉得麒麟的东西没有太鸡肋，所以直接上Kylin即可。

[软碟通（UltraISO）](https://cn.ultraiso.net/xiazai.html)
制作u盘镜像比较好用

另外还需要8个G的U盘，上面的盘子用完以后格式化做这个盘子也没事儿。

### 磁盘空间初步分配

进入磁盘管理
![](http://upload-images.jianshu.io/upload_images/671333-dc6d1df4a0cdeeb9.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里我盘分完了没有剩余空间，所以将使用空间比较小的盘分出来一点，然后右键选择压缩卷
![](http://upload-images.jianshu.io/upload_images/671333-62656bf485dfa53b.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果一个盘留不出这么多空间也可以压缩两个盘各分出一点，总空间上基本一致也行。

压缩得到的空间要放你的unbuntu，所以分个50G差不多吧，我留了50G
![](http://upload-images.jianshu.io/upload_images/671333-774209de6270a208.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### win10内商业化策略配置修正
这部的东西跟微软的商业化策略有关，具体的原理和问题我最后一部跟大家交流

#### 禁用快速启动
左下角右键，菜单内找到电源选项
![](http://upload-images.jianshu.io/upload_images/671333-837bb5e8df95cfa7.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-a2f098c304adf7a3.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-d8c1eddc38c86f02.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注： “快速启动”是Windows10 为了加速自己的启动过程而跳过一些bios的检查。

#### 禁用安全启动(Secure Boot)
![](http://upload-images.jianshu.io/upload_images/671333-9ad9e1bbe89a46b9.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-f6d30476f9890b87.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-a4c2b221482662ce.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-79c14596b8e2ff01.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-8744a1856b523ffb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-1e32fb618b44052d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

也可以暴力方式完成这个步骤
首先进入bios
![](http://upload-images.jianshu.io/upload_images/671333-dc6247170f77acb5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-c55a0714e274bc38.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Secure Boot也是一个微软策略
有兴趣可以看这篇帖子
[反Secure Boot垄断：兼谈如何在Windows 8电脑上安装Linux](http://www.ruanyifeng.com/blog/2013/01/secure_boot.html)

### 制作Ubuntu的启动U盘

进入UltraISO，打开文件：
![](http://upload-images.jianshu.io/upload_images/671333-e92ae2799befcd4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-b48b97bc234b3d9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-eb7ee3645028df4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

写入完毕：
![](http://upload-images.jianshu.io/upload_images/671333-0632d96cf8a98934.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### U盘安装Ubuntu
找到镜像U盘，调整Priority Order，Save and Exit
![](http://upload-images.jianshu.io/upload_images/671333-52245da80b6a24fe.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注：下次restart记得重置，否则无限循环。
![](http://upload-images.jianshu.io/upload_images/671333-b2c03e24a877eb5b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择“安装Ubuntu Kylin”：
![](http://upload-images.jianshu.io/upload_images/671333-930c13649fd95892.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

完成默认设置：
![](http://upload-images.jianshu.io/upload_images/671333-316c38f12e231f9c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注：如果网络和空间匀速，建议选择“安装中下载更新”和“安装这个第三方软件”。

非常重要的一步，选择“其他选项”：
![](http://upload-images.jianshu.io/upload_images/671333-7f565024748264af.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为空闲磁盘分区：
在这一步会看到我们之前分配的未使用磁盘空间，我们即将为这块空闲磁盘分区，为了更方便理解接下来的操作，这里简单介绍一下安装过程所涉及到的几个主要的Linux分区：
/：存储系统文件，建议10GB ~ 15GB；
swap：交换分区，即Linux系统的虚拟内存，建议是物理内存的2倍；
/home：home目录，存放音乐、图片及下载等文件的空间，建议最后分配所有剩下的空间；
/boot：包含系统内核和系统启动所需的文件，实现双系统的关键所在，建议200M。
![](http://upload-images.jianshu.io/upload_images/671333-efa9c15dce9a2366.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选定空闲磁盘，点击+，首先分配16G空间给/分区，选择“主分区”、“空间起始位置”、Ext4和“挂载点/”：
![](http://upload-images.jianshu.io/upload_images/671333-b66a114eca0c8aee.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注：实际上，一块硬盘最多容纳4个主分区，或3个主分区外加1个扩展分区，在选择分区类型时，可能会出现“安装系统时空闲分区不可用”状况，为了解决问题，下面一律选择“逻辑分区”。
重复创建步骤，分配16G空间给swap分区，选择“逻辑分区”(主分区已满)、“空间起始位置”、用于“交换空间”：
![](http://upload-images.jianshu.io/upload_images/671333-8e62830d2d99a979.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接着分配200M空间给/boot分区，选择“逻辑分区”(主分区已满)、“空间起始位置”、“Ext4”和“挂载点/boot”：
![](http://upload-images.jianshu.io/upload_images/671333-3245cb65c8097b19.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后将所有剩余空间分配给/home分区，选择“逻辑分区”(主分区已满)、“空间起始位置”、“Ext4”和“挂载点/home”：
![](http://upload-images.jianshu.io/upload_images/671333-c70f4de86b3d6db5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择/boot对应的盘符作为“安装启动引导器的设备”，务必保证一致：
![](http://upload-images.jianshu.io/upload_images/671333-f4cecdad1256398f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将改动写入磁盘
![](http://upload-images.jianshu.io/upload_images/671333-e60e325a26327ab6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

进入最后的设置：
![](http://upload-images.jianshu.io/upload_images/671333-611c071e24380048.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-cc8b9e0e50fefef9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-159aa5ed54bb2977.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-abe8611d11530843.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/671333-0829a0d732bb9991.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---


OK,到上一步为止是每一个帖子里面都大同小异的内容，终于贴完，所以下面的就是每篇帖子和每个朋友使用的不同的地方。

有朋友选择关闭uefi，有朋友用easyBCD也有人用easyUEFI，还有茫茫多的选择。为什么这件事后面内容这么多呢，大家可以在这个时候重启选择进入windows，然后重启。

没错你会发现直接进入了windows，刚刚的蓝色界面的grub2引导完全不见了，无论如何做系统优化，竟然要强改引导部分的活不得不说微软的商业逻辑确实跟我这样的开源爱好者相差甚远

没事儿，只要第一遍进了grub2那么这个时候我就开始教一步步怎样修复grub2引导












