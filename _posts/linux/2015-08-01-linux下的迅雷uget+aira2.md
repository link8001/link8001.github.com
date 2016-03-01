---
layout: post
comments: true
categories: linux
---

Windows下的下载工具--迅雷，之所以下载速度快，乃是它能搜索资源、为己所用，而不是仅仅从原始地址这单一资源处下载。

Ubuntu下也有类似的工具，那就是aira2。

aira2是一个命令行下载工具，可以配合其他图形界面的下载软件使用。我用的是uget+aria2。uget本身是一个小巧实用的多线程下载工具，加上aria2作为插件，下载速度有明显提高。

一、安装

uget和aria2都可以在“软件中心”中安装，但是版本太老啦，无法发挥作用，所以最好还是在终端中添加ppa进行安装：

1.uget的安装：

*sudo add-apt-repository ppa:plushuang-tw/uget-stable*

*sudo apt-get update*

*sudo apt-get install uget*

2.aria2的安装：

*sudo add-apt-repository ppa:t-tujikawa/ppa*

*sudo apt-get update*

*sudo apt-get install aria2*

安装完aria2后，可以在终端中运行aria2c -v，查看版本和支持的特性。需要1.10以上的版本才能支持资源搜索。

二、使用与设置技巧

1.主界面及版本

uGet1.8.2(stable)

2.启用aria2插件

*--enable-rpc=true*

3.设置下载任务的属性(同时下载几个任务、多少个服务器、保存位置等)

Home/属性

经本人测试(其他网友也是如此设置的)，服务器数设置为16比较合适。

OK啦，现在可以享受你的下载速度啦。我这破网速，用uget一直维持在80k/S左右，用了aria2插件后，经常冲上160K/S，哈哈。

apt-fast是一款替代apt-get提升下载速度的软件，安装软件时，通过增加线程使下载软件速度加快。

apt-fast已经更新到1.6.4版本，引入配置“对话框”，在其安装过程中，一步步对话框选择设置，每个用户的配置选项，从而改善和清晰化apt-fast的初始配置过程。

如何安装apt-fast ?添加官方PPA源(支持ubuntuLucid,Natty,Oneiric,Precise,Quantal)，打开终端，输入以下命令：

Ubuntu11.04~13.10(out of date)

*sudo apt-get install axelaria2*

*sudo apt-add-repository ppa:apt-fast/stable*

*sudo apt-get update*

*sudo apt-get install apt-fast*

Ubuntu14.04 and later versions

*sudo add-apt-repository ppa:saiarcot895/myppa*

*sudo apt-get update*

*sudo apt-get -y install apt-fast*

安装之后使用方法和apt-get使用一样.更新源列表

*sudo apt-fast update*

更新系统

*sudo apt-fast upgrade*
