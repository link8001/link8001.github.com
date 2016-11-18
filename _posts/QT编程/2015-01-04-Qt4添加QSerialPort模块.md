---
layout: post
comments: true
categories: QT编程
---

转自[CSDN](http://blog.csdn.net/yuyu414/article/details/42400721)

Qt中添加了QSerialPort类，非常好用，可是由于各种原因，很多人还是要用Qt4，比如我，后来发现官方文档说QSerialPort支持Qt4，就研究了一下，比较笨，搞了好久才弄好。

### 第一步：下载QSerialPort模块

官方网站我经常打不开，所以上传到网盘了。

http://download.qt-project.org/  这是官方的，各种版本都有，大家可以自己找找。

http://pan.baidu.com/s/1gdnanOv  这是我自己下载的，共享给大家。

### 第二步：打开QSerialPort工程
首先确认您电脑上有可以正常使用的Qt4版本，我这里用的是Qt4.8.6.

解压下载的qtserialport-opensource-src-5.3.2.rar，会发现一个qtserialport.pro，打开它。

选择项目，在构建步骤中选择“添加构建步骤->Make”，如下图：

![2](http://img.blog.csdn.net/20150104213230343)

在Make参数这一栏里输入“install”

![2](http://img.blog.csdn.net/20150104213341467)

搞定，现在点击项目中的“构建”，就会编译，然后把QSerialPort库安装到你的Qt4里面。

![2](http://img.blog.csdn.net/20150104213626000)


这是Debug版本，然后Release版本也是一样做的

![2](http://img.blog.csdn.net/20150104213850940)

### 第三步：编译错误undefined reference to ...
如果是下载的官方的，可能会有下面的错误：（建议用最新的5.4.2版本，已经修复改错误，后面有链接）

![2](http://img.blog.csdn.net/20150104214346748)

跟踪到错误的地方：

![2](http://img.blog.csdn.net/20150104214418619)

修改为：

![2](http://img.blog.csdn.net/20150104214701234)

再次编译，还有错误：

![2](http://img.blog.csdn.net/20150104214711811)

跟踪错误地方：

![2](http://img.blog.csdn.net/20150104214749188)

修改为：注意四个地方都要修改

![2](http://img.blog.csdn.net/20150104214859934)

### 第四步：串口测试程序
新建一个工程，在pro文件中加入 CONFIG += serialport

![2](http://img.blog.csdn.net/20150104215151109)


然后在.h文件中包含 #include <QSerialPort/QSerialPort> #include <QSerialPort/QSerialPortInfo>

![2](http://img.blog.csdn.net/20150104215138150)

搞定了，QSerialPortInfo是一个很不错的东西，可以查找电脑上的可用串口，用法如下：

![2](http://img.blog.csdn.net/20150104215703161)

有时候串口比较多的时候，会发现排序混乱，向下面这个样子：

![2](http://img.blog.csdn.net/20150104215759648)

作为一个重度强迫症程序员，这是绝对不允许的，3怎么能排在8后面呢，绝对不能！！！

这样解决：

![2](http://img.blog.csdn.net/20150104220300199)

这回看起来就舒服多了:

![2](http://img.blog.csdn.net/20150104220401875)

差不多就这些了。

### 虚拟串口工具
推荐一个虚拟串口工具，很好用，里面有教程

http://download.csdn.net/detail/yuyu414/5094469

上面这个需要两个积分，找了半天不知道怎么删掉积分要求，重新上传了一份在百度网盘，好东西，当然要大家来分享

http://pan.baidu.com/s/1o69M78I

——————————————————我是分割线——————————————————

Qt串口模块更新：qtserialport-opensource-src-5.4.0
修复了上面的错误，官方更新日志：

 - [QTBUG-41190] Fixed build on Qt4 related to QT_DEPRECATED_SINCE(5, 2) macro.


 - QSerialPortInfo:
   * Added support of a serial number of the USB device from the Sysfs backend.
   * Fixed crash on OSX related to wrong use of QCFString class.
   * Added enumeration of on-board types of serial ports (like SOC and OCP)
     at using of the Sysfs backend on Linux.
   * [QTBUG-40113] Fixed crash related to dynamic udev loading.


 - QSerialPort:
   * Fixed filtering of custom baud rate on Linux.
   * [QTBUG-41295] Now the reading are not stalled on Windows when used the
     read buffer with the limited size.
   * Now the reading are not stalled on Windows after calling of the clear()
     method.
   * Now the bytesToWrite() on Windows returns the zero after WriteFile()
     was called.

——————————————————我还是分割线——————————————————

重要更新：Qt串口模块5.5开始不再支持Qt4，所以建议用最后一个版本，qtserialport-opensource-src-5.4.2

下载地址：

http://pan.baidu.com/s/1o6UVlk2 百度网盘
http://download.qt.io/official_releases/qt/5.4/5.4.2/submodules/ 官方
