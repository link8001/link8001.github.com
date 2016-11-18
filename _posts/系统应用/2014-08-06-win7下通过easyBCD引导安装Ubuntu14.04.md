---
layout: post
comments: true
categories: 系统应用
---

转自[百度经验](http://jingyan.baidu.com/article/e4d08ffdace06e0fd2f60d39.html)

Ubuntu14.04作为目前最新版本的ubuntu系统，相信很多人都想在自己的电脑上安装一下，然而系统的安装方法各式各样，u盘法、grub引导法等等，这里我将介绍在win7系统下用easyBCD软件建立引导，然后安装ubuntu的方法，这就不需要制作u盘启动盘那么麻烦，只需配置一些引导文件即可。

![2](http://h.hiphotos.baidu.com/exp/w=500/sign=df21d031259759ee4a5060cb82fa434e/14ce36d3d539b600ed28bb8bea50352ac65cb72c.jpg?_=4803863)

### 工具/原料

    win7系统
    ubuntu14.04系统镜像文件
    easyBCD 2.2
    DAEMON tools（非必须）

### 安装系统的前期准备

1.1 在网上下载ubuntu-14.04-desktop-amd64.iso，百度搜索一下，很多网站都有下载链接，同时准备好easyBCD软件（网上下载最新版）。建议将iso文件移动到c盘根目录，当然也可以放到别的目录。

![2](http://d.hiphotos.baidu.com/exp/w=500/sign=f740a91339292df597c3ac158c305ce2/7e3e6709c93d70cf5869c631fbdcd100baa12b7e.jpg?_=4803863)

2.2 打开easyBCD软件，可以看到目前只有一个win7启动项。

选择“添加新条目”，然后选择“NeoGrub”，点击“安装”。

![2](http://e.hiphotos.baidu.com/exp/w=500/sign=8f74d82accfc1e17fdbf8c317a91f67c/f3d3572c11dfa9ec46ce392b61d0f703918fc12e.jpg?_=4803863)

![2](http://f.hiphotos.baidu.com/exp/w=500/sign=501457db922397ddd67998046982b216/ae51f3deb48f8c54a1c2ab1339292df5e0fe7ffc.jpg?_=4803863)

2.3 然后点击配置，将menu.lst文件的内容替换成一下文本：

    title Install Ubuntu

    root (hd0,0)

    kernel (hd0,0)/vmlinuz.efi boot=casper iso-scan/filename=/ubuntu-14.04-desktop-amd64.iso locale=zh_CN.UTF-8

    initrd (hd0,0)/initrd.lz

    title reboot

    reboot

    title halt

    halt

说明：hd0表示c盘所处的硬盘号，一般电脑只有一个，所以都是hd0；如果有多个硬盘，则根据情况改为hd0、hd1等。

hd0后面的数字表示C盘在硬盘中的分区顺序，每个人的系统不大一样，不知道的可以在磁盘管理里面看一下，本人c盘是第三个分区，因此写为（hd0,2），如果是第一个，写为（hd0,0）即可。

![2](http://g.hiphotos.baidu.com/exp/w=500/sign=65f9eb5dfb1986184147ef847aec2e69/503d269759ee3d6d8319e9f740166d224f4ade2b.jpg?_=4803863)

![2](http://h.hiphotos.baidu.com/exp/w=500/sign=916546d9ae6eddc426e7b4fb09dab6a2/eac4b74543a98226506d431b8982b9014a90eb5a.jpg?_=4803863)

2.4 经过配置后，c盘中会多出一个“NST”文件夹和一个NeoGrub文件。

接下来把系统镜像文件用DAEMON tools打开，或者以压缩包形式将其中两个文件解压出来，分别是initrd.lz和vmlinuz.efi，这两个文件在casper文件夹里。

![2](http://c.hiphotos.baidu.com/exp/w=500/sign=7a71b14fae4bd11304cdb7326aaea488/b21c8701a18b87d6959bfe23040828381f30fd35.jpg?_=4803863)

![2](http://c.hiphotos.baidu.com/exp/w=500/sign=f11f9be1a864034f0fcdc2069fc27980/1e30e924b899a901f6ec0e0a1e950a7b0208f535.jpg?_=4803863)

2.5 将解压出来的两个文件复制到c盘根目录，可以看看c盘中添加了多少相关的文件。这样，安装系统的前期准备就完成了，可以重启电脑准备安装ubuntu14.04了。

![2](http://b.hiphotos.baidu.com/exp/w=500/sign=ed76ed5897eef01f4d1418c5d0ff99e0/a686c9177f3e670990c411c338c79f3df9dc55c1.jpg?_=4803863)

### 安装Ubuntu系统

3.1 重启电脑，可以看到多了一个启动项“NeoGrub引导加载器”，选择该项enter，然后选择“install ubuntu”，另外两项分别是“重启”和“关机”，是为了防止安装出错而设的，这个在之前的menu.lst配置文件中已经设定好了。

![2](http://b.hiphotos.baidu.com/exp/w=500/sign=0e9e1405ca3d70cf4cfaaa0dc8ddd1ba/7a899e510fb30f243307aa3fcb95d143ad4b0368.jpg?_=4803863)
![2](http://d.hiphotos.baidu.com/exp/w=500/sign=d29540f6818ba61edfeec82f713597cc/ac6eddc451da81cb5a7bc5f15166d01609243168.jpg?_=4803863)

3.2 接下来如果正常的话，会刷刷的满屏幕文字，很高端的样子，等着它结束就行。如果在这一步报错，一般来说就是之前的menu.lst配置文件不对，无法访问C盘，这时只要“reboot”然后进入win7重新配置就行。

![2](http://e.hiphotos.baidu.com/exp/w=500/sign=4febeb31fbdcd100cd9cf821428b47be/43a7d933c895d143d967a51f70f082025aaf07fc.jpg?_=4803863)

![2](http://g.hiphotos.baidu.com/exp/w=500/sign=c99dbd1b7c3e6709be0045ff0bc79fb8/34fae6cd7b899e5161a87b2841a7d933c8950de3.jpg?_=4803863)

3.3 刷完后就进入一个小系统，别以为这就装好了，此时最重要的一步，通过快捷键ctrl+alt+T打开终端，输入：

    sudo umount -l /isodevice

注意空格和小写的L，执行后就可以双击安装图标进行安装了

![2](http://g.hiphotos.baidu.com/exp/w=500/sign=c0772de23e6d55fbc5c676265d234f40/d439b6003af33a870fb04126c55c10385343b529.jpg?_=4803863)

![2](http://h.hiphotos.baidu.com/exp/w=500/sign=4910d3e3cd1b9d168ac79a61c3dfb4eb/fc1f4134970a304e1ac9e1c6d2c8a786c9175c26.jpg?_=4803863)

![2](http://e.hiphotos.baidu.com/exp/w=500/sign=81873b4623a446237ecaa562a8237246/c75c10385343fbf25eb4e71fb37eca8065388f76.jpg?_=4803863)

3.4 接下来选择简体中文；不用选中安装第三方软件和更新，否则安装会很慢，为保险起见可以断开网络连接；安装类型选择“其他选项”。

![2](http://f.hiphotos.baidu.com/exp/w=500/sign=ee7600054836acaf59e096fc4cd88d03/5d6034a85edf8db1ee8a3f580a23dd54564e7461.jpg?_=4803863)

![2](http://e.hiphotos.baidu.com/exp/w=500/sign=0c988e158518367aad897fdd1e728b68/279759ee3d6d55fb0e32351a6e224f4a20a4dd12.jpg?_=4803863)

![2](http://e.hiphotos.baidu.com/exp/w=500/sign=77d5f10da28b87d65042ab1f37092860/21a4462309f790525ceb04020ff3d7ca7bcbd512.jpg?_=4803863)

3.5 接下来需要设置分区，首先设置交换空间大小，与电脑内存差不多或为电脑内存的两倍。

![2](http://h.hiphotos.baidu.com/exp/w=500/sign=401f51739d2f07085f052a00d925b865/91529822720e0cf310030dc70946f21fbe09aa12.jpg?_=4803863)

![2](http://b.hiphotos.baidu.com/exp/w=500/sign=9a8a3f580a23dd542173a768e108b3df/4610b912c8fcc3ce92648a0f9145d688d43f2061.jpg?_=4803863)

![2](http://b.hiphotos.baidu.com/exp/w=500/sign=bfcf6b99097b02080cc93fe152d8f25f/f7246b600c3387443a316c40520fd9f9d72aa012.jpg?_=4803863)

3.6 然后设置其他挂载点的大小，分区方案很多，这里简单的设置/、/boot、/home共3个分区，均为ext4文件系统。

/ 10G；/boot 100M;/home 剩余所有空间。

注意linux系统的1G对应1000M。

![2](http://h.hiphotos.baidu.com/exp/w=500/sign=b51084df542c11dfded1bf2353266255/500fd9f9d72a6059f2e4a2262b34349b033bba12.jpg?_=4803863)

3.7 接下来就是简单的设置地区、键盘布局，接着就是用户名和密码。

![2](http://d.hiphotos.baidu.com/exp/w=500/sign=26e7a2262b34349b74066e85f9eb1521/7dd98d1001e9390122f5ce0278ec54e736d19612.jpg?_=4803863)

![2](http://b.hiphotos.baidu.com/exp/w=500/sign=c02063c235d3d539c13d0fc30a86e927/7aec54e736d12f2eb0f2ed5c4cc2d56285356812.jpg?_=4803863)

![2](http://d.hiphotos.baidu.com/exp/w=500/sign=03f2254ff8f2b211e42e854efa816511/e61190ef76c6a7efddd1eb1afefaaf51f3de6612.jpg?_=4803863)

3.8 以上所有东西都设置好了，就自动开始安装系统，等待一段时间就可以，如果安装过程在下载东西，可以点击“跳过”，因为系统安装完成后同样可以更新下载。

![2](http://f.hiphotos.baidu.com/exp/w=500/sign=ee38c41d9013b07ebdbd50083cd69113/77c6a7efce1b9d16e830d95df0deb48f8c546412.jpg?_=4803863)

![2](http://a.hiphotos.baidu.com/exp/w=500/sign=7948c4bbd3a20cf44690fedf46084b0c/3b292df5e0fe99259cfb2a6c37a85edf8db17112.jpg?_=4803863)

3.9 安装完成后点击“现在重启”，可以看到多了好多启动项，界面也变了。选择第一项启动ubuntu系统，这样就可以愉快的玩转ubuntu啦。
![2](http://f.hiphotos.baidu.com/exp/w=500/sign=e4e64c26c55c1038247ecec28210931c/d4628535e5dde7119d590fcaa4efce1b9d166106.jpg?_=4803863)
![2](http://f.hiphotos.baidu.com/exp/w=500/sign=2a5a1a55262dd42a5f0901ab333b5b2f/2fdda3cc7cd98d10fc99430d223fb80e7bec90b9.jpg?_=4803863)

3.10 安装完后，不要忘了回到win7系统打开easyBCD软件把“NeoGrub”引导项删除，否则每次进入win7都得选一次。如果觉得以后不想重装Ubuntu了，就可以把C盘的相关文件都删掉，节省空间嘛。

好啦，祝大家玩得愉快！
![2](http://c.hiphotos.baidu.com/exp/w=500/sign=6373fdda247f9e2f70351d082f31e962/08f790529822720ece0badc678cb0a46f31fabc1.jpg?_=4803863)

注意事项

    menu.lst的内容不要弄错，正确判别c盘的分区号
    进入小系统后别忘了执行指令sudo umount -l /isodevice
    新手在为系统分区前可以先网上搜索一下分区方案
    小系统是可以直接联网操作的，如果遇到什么不懂的可以通过自带的火狐浏览器进行搜索
