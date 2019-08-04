
# 基于sip协议的音视频通话服务器和安卓端APP
==========================================================

### 服务器搭建

系统为ubuntu16.04，如果是云服务器请确保所有端口都打开！

##### 1.下载源码以及铃声文件
```
wget http://downloads.xiph.org/releases/opus/opus-1.1.4.tar.gz
wget http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz
wget http://files.freeswitch.org/freeswitch-releases/freeswitch-1.6.17.tar.xz
mkdir freeswitch_sounds
freeswitch_sounds
wget http://files.freeswitch.org/releases/sounds/freeswitch-sounds-en-us-callie-8000-1.0.50.tar.gz
wget http://files.freeswitch.org/releases/sounds/freeswitch-sounds-en-us-callie-16000-1.0.50.tar.gz
wget http://files.freeswitch.org/releases/sounds/freeswitch-sounds-en-us-callie-32000-1.0.50.tar.gz
wget http://files.freeswitch.org/releases/sounds/freeswitch-sounds-en-us-callie-48000-1.0.50.tar.gz
wget http://files.freeswitch.org/releases/sounds/freeswitch-sounds-music-8000-1.0.50.tar.gz
wget http://files.freeswitch.org/releases/sounds/freeswitch-sounds-music-16000-1.0.50.tar.gz
wget http://files.freeswitch.org/releases/sounds/freeswitch-sounds-music-32000-1.0.50.tar.gz
wget http://files.freeswitch.org/releases/sounds/freeswitch-sounds-music-48000-1.0.50.tar.gz
```


#### 2.编译脚本
新建脚本文件install.sh，内容如下
```
#!/bin/bash
workpath=`pwd`
sudo apt-get update
sudo apt-get install -y vim
sudo apt-get install -y g++
sudo apt-get install -y zlib1g-dev
sudo apt-get install -y libjpeg-dev
sudo apt-get install -y libsqlite3-dev
sudo apt-get install -y libcurl4-gnutls-dev
sudo apt-get install -y libpcre3-dev
sudo apt-get install -y libspeexdsp-dev
sudo apt-get install -y libedit-dev
sudo apt-get install -y libssl-dev
sudo apt-get install -y libopus-dev
sudo apt-get install -y liblua5.2-dev
sudo apt-get install -y libldns-dev
sudo apt-get install -y libsndfile1-dev

sudo ln -s /usr/lib/x86_64-linux-gnu/liblua5.2.so.0.0.0 /usr/lib/x86_64-linux-gnu/liblua.so
sudo mv /usr/include/lua5.2/lua* /usr/include/
sudo mv /usr/include/opus/opus* /usr/include/
sudo mv /usr/lib/x86_64-linux-gnu/libsndfile* /usr/lib/
sudo ldconfig -v
cd ${workpath}
tar xf yasm-1.3.0.tar.gz
cd ${workpath}/yasm-1.3.0/
./configure && make && sudo make install

cd ${workpath}
tar xf freeswitch-1.6.17.tar.xz
cp freeswitch_sounds/freeswitch-sounds-* freeswitch-1.6.17/
sudo rm -rf  ${HOME}/freeswitch
cd ${workpath}/freeswitch-1.6.17/
./configure --prefix=${HOME}/freeswitch && make -j 4 && make install && make cd-sounds-install && make cd-moh-install

rm -rf ${workpath}/freeswitch-1.6.17
rm -rf ${workpath}/yasm-1.3.0


sudo iptables -F
${HOME}/freeswitch/bin/freeswitch
```

打开终端修改脚本文件可执行权限：
```
chmod a+x install.sh
```
此时的文件目录应如下：

![](./image/tree_of_files.png)

#### 3.编译安装
注意必须使用root用户运行该脚本才可以！

打开终端输入：
```
./install.sh
```

等待安装完毕！
注：如果使用的是云服务器则需要修改部分配置才能用sip软件连接上。
