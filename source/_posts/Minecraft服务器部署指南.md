---
title: Minecraft服务器部署指南
date: 2022-08-15 17:08:31
tags: 
    - Minecraft
    - 部署
    - Game
---
>Ryokutya心血来潮，且邀请许久不联系的小伙伴开始了MC之旅~
<!--more-->
### 准备工作
首先需要准备一台全新的服务器 * 嘶哈嘶哈 *，当然不可能在本地部署对吧。这里我用虚拟机模拟在远程服务器终端上的操作。
>运行环境 `Ubuntu 20.04LTS`，版本较新就行，不重要捏。

众所周知，`Minecraft`是用`Java`开发的。那么第一步，先安装`JDK`。它是Java的开发工具包，包括了Java运行环境`JRE`，Java工具（javac/java/jdb等），还有Java的基础API。  
那么直接安装`JRE`也是没有关系的，这里演示如何安装`JDK`的过程。

### 安装JDK
首先打开Oracle官网的[JDK下载页面](https://www.oracle.com/java/technologies/downloads/)，获取下载连接。由于要将Minecraft部署在linux服务器上，选择Linux对应的`x64 Compressed Archive`就行啦。  
![┭┮﹏┭┮图片失效了..](/images/jdk_install.png)  
那么我们复制右边对应的链接，下载JDK到当前目录。
```bash
wget https://download.oracle.com/java/18/latest/jdk-18_linux-x64_bin.tar.gz
```
下载可能会有些小慢，耐心等待下载完成。之后在当前路径下找到`jdk-18_linux-x64_bin.tar.gz`，将其解压到`/usr/local`下。
```bash
sudo tar -zxvf jdk-18_linux-x64_bin.tar.gz -C /usr/local
```
之后进入`/usr/local`目录下，查看当前JDK文件夹名称`jdk-18.0.2`，并记住这个文件夹，之后一步要用到。  
最后一步就是配置环境变量啦。
```bash
export JAVA_HOME=/usr/local/jdk-18.0.2 #这个jdk-18.0.2就是上边解压出来的文件夹
export PATH=$JAVA_HOME:$JAVA_HOME/bin:
```
配置完成后，在控制台中输入`java -version`出现三行，`JDK`安装成功了！

### 下载Minecraft_server.jar
点击[这里](https://www.minecraft.net/zh-hans/download/server)进入下载页面，或者你有其他的版本的jar包，那么直接放到服务器上。
```bash
cd ~ #切换到家目录
wget  https://piston-data.mojang.com/v1/objects/f69c284232d7c7580bd89a5a4931c3581eae1378/server.jar #下载官方jar包
```
### 启动Minecraft
```bash
java -Xmx1024M -Xms1024M -jar minecraft_server.1.19.2.jar --nogui --port 19999  #一条命令就启动啦！
```
具体的启动参数请参考[官方文档](https://minecraft.fandom.com/wiki/Tutorials/Setting_up_a_server#Java_options)。

### 别忘了开放服务器端口！
进入云服务器的控制台，开放端口（19999），部署就完成了！

---
>另外还有docker部署的方式，之后写吧，🕊。  