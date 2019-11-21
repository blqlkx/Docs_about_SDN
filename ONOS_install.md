# ONOS_INSTALL
[toc]

ONOS安装记录
操作系统：ubuntu18.04
详情：Distributor ID:	Ubuntu
	   	  Description:	Ubuntu 18.04.2 LTS
          Release:	18.04
          Codename:	bionic
查看系统信息命令：lsb_release -a
Ps：本文安装onos也基于此系统版本下。Centos安装或者其他系统安装 可能情况不一样本文描述的是针对ubuntu18.04下安装onos的记录。
切换root用户密令 ：su root  输入密码即可
新系统需要先设置root用户密码：sudo password root 先输入当前用户密码 再输入root用密码即可。
然后用su root 命令切换到root 用户


更换下载源
目的：提升软件下载速度，解决下载限制等问题
操作如下：
编辑/etc/apt/sources.list文件, 在文件最前面添加以下条目(操作前请做好相应备份)：

##中科大源
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

ps:镜像源查询网站：https://mirrors.ustc.edu.cn/repogen/ 
请根据根据自己系统版本信息选对应的镜像源 添加到/etc/apt/sources.list 中即可。



## install
1. 安装jdk1.8：
官网下载：https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
不要装openjdk  后续onos编译会有问题。
安装步骤：1.下载jdk-8u201-linux-x64.tar.gz
	      2.移动到/usr/local/src 目录下面
	      3.解压缩文件：	tar -zxvf jdk-8u201-linux-x64.tar.gz
          4.改名： mv jdk1.8.0_201 jdk1.8
          5.需要的话安装vim 完整版：具体命令如下
	sudo apt-get remove vim-common   //删除vim-common 精简版
	sudo apt-get install vim    //安装 完整版

 6. vi /etc/profile
export JAVA_HOME=/usr/local/src/jdk1.8 
export JRE_HOME=${JAVA_HOME}/jre export 
CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib 
export PATH=.:${JAVA_HOME}/bin:$PATH
7.使环境变量生效  source /etc/profile
8.验证是否安装成功： java -version 出现 java version "1.8.0_201"  版本信息代表安装成功

安装git
命令 ：sudo apt-get install git
输入： git -version 现实git版本信息代表安装成功

ubuntu安装bazel （编译onos工具）
支持的操作系统版本
 
安装必要的支撑软件：
sudo apt-get install pkg-config zip g++ zlib1g-dev unzip python
下载bazel：地址https://github.com/bazelbuild/bazel/releases
 
下载图中的版本

安装：
chmod +x bazel-<version>-installer-linux-x86_64.sh
./bazel-<version>-installer-linux-x86_64.sh --user
安装到root目录下面。--user 默认安装到当前root/bin用户下面 当前是root用户的前提下
配置环境变量exprot PATH="$PATH:$HOME/bin"
	source /etc/profile
验证时否安装成功：运行bazel命令

安装mininet：
方式1命令 sudo apt-get install mininet方式 2源码安装略
	     测试是否安装成功 mn --version 查询 mininet 版本信息

安装ftp服务
安装命令：sudo apt-get install vsftpd
可以使用下列命令来打开，关闭，重启ftp服务
sudo /etc/init.d/vsftpd start
sudo /etc/init.d/vsftpd stop
sudo /etc/init.d/vsftpd restart

安装onos
git clone https://gerrit.onosproject.org/onos
git tag
git checkout 1.9.1
1.9.1 以上版本用bazel编译有问题。建议用1.9.1
cd onos
添加环境变量 /etc/profile export ONOS_ROOT=~/onos路径
tool/build/onos-buck build onos --show-ouput 或者 source tools/dev/bash_profile 使用op命令编译
编译结束会打印压缩包位置

编译好的jar包路径：
buck-out/gen/tools/package/onos-package/onos.tar.gz
复制到别的目录下解压
cd 解压目录/apache-karaf-3.0.8/bin
./start clean debug #启动onos服务
./client #打开控制台
开启相关服务
onos> app activate org.onosproject.openflow
onos> app activate org.onosproject.fwd
不开这两个服务mininet无法连接onos

./stop #停止onos服务

测试mininet和onos连接脚本：
sudo mn --controller remote,ip=127.0.0.1 --topo torus,3,3

onos：gui地址：http:localhost:8181/onos/ui
默认用户名密码：onos rocks