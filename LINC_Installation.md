# LINC安装指南

## Erlang环境安装
参考GitHub：https://github.com/FlowForwarding/LINC-Switch

### Ubuntu（执行1，3，7）
1. sudo apt-get install gcc wget make autoconf openssl libssl-dev libncurses5 libncurses5-dev
注意apt包的版本，高版本的需要先卸载再安装低版本
libssl0.9.8的安装：https://pkgs.org/download/libssl0.9.8， 下载并用`dpkg -i libssl_xxx.deb`安装

2. CentOS安装 有几个包不一样，用
yum install gcc wget make autoconf openssl openssl-devel ncurses ncurses-devel
参考URL：https://blog.zfanw.com/install-erlang-on-centos/

3. 通过.deb安装
dpkg -i esl-erlang_16.b.2.basho10-1~ubuntu~bionic_amd64.deb

2. 从Erlang官网下载source：
https://www.erlang.org/downloads
**注意：**
Ubuntu18.04及以上 必须选择**R16B**
Ubuntu16.04及以下 必须选择**R16B03**
**LINC要求的Erlang版本不能高于R16B03。**

4. Unpack
tar -zxf otp_src_%OTP-VSN%.tar.gz
cd otp_src_%OTP-VSN%
export ERL_TOP='pwd'（如果这条命令有问题就把单引号换成反引号，markdown里面反引号转义了）

5. compile && install
./otp_build autoconf
./configure && make && sudo make install

6. 验证
$ erl
> Erlang R16B03 (erts-5.10.4) [source] [64-bit] [async-threads:10] [kernel-poll:false]
Eshell V5.10.4  (abort with ^G)
1> 

再按ctrl+c, 再按v查看版本信息
> Eshell V5.10.4  (abort with ^G)
1> 
BREAK: (a)bort (c)ontinue (p)roc info (i)nfo (l)oaded
(v)ersion (k)ill (D)b-tables (d)istribution
v
Erlang (BEAM) emulator version 5.10.4
Compiled on Tue Aug 19 16:45:11 2014

## LINC安装
1. git clone \<repo>,
URL : https://github.com/FlowForwarding/LINC-Switch
2. cd LINC_SWITCH
3. 安装其他用于LINC的apt包
apt-get install git-core bridge-utils libpcap0.8 libpcap-dev libcap2-bin uml-utilities
4. 重命名sys.config.orig为sys.config
cd rel/files
mv sys.config.orig sys.config
5. 编译
cd LINC_SWITCH
make
6. 验证
> root@workgroup3:~/LINC-Switch# rel/linc/bin/linc console
Exec: /root/LINC-Switch/rel/linc/erts-5.10.4/bin/erlexec -boot /root/LINC-Switch/rel/linc/releases/1.0/linc -mode embedded -config /root/LINC-Switch/rel/linc/releases/1.0/sys.config -args_file /root/LINC-Switch/rel/linc/releases/1.0/vm.args -- console
Root: /root/LINC-Switch/rel/linc
Erlang R16B03 (erts-5.10.4) [source] [64-bit] [async-threads:10] [kernel-poll:false]
load_driver 'netlink_drv' from: '/root/LINC-Switch/rel/linc/lib/netlink-1/priv'
10:15:10.508 [info] Application lager started on node linc@workgroup3
10:15:10.508 [info] Application ssh started on node linc@workgroup3
10:15:10.511 [info] Application enetconf started on node linc@workgroup3
10:15:10.515 [info] Application linc started on node linc@workgroup3
Eshell V5.10.4  (abort with ^G)
(linc@workgroup3)1> 

7. 常用命令
运行：~/LINC-Switch/rel/linc/bin# ./linc start
停止：~/LINC-Switch/rel/linc/bin# ./linc stop

8. 参考URL
[LINC安装指南_SDNLAB](https://www.sdnlab.com/13326.html)
[LINC使用指南](https://www.sdnlab.com/13375.html)
参考GitHub（见上文）
[OTP_Github](https://github.com/erlang/otp/blob/maint/HOWTO/INSTALL.md)

## LINC_config_generator安装
1. git clone https://github.com/FlowForwarding/LINC-config-generator
2. cd LINC-config-generator
3. make
4. 将lincoe的sysconfig文件复制到此文件夹下：cp /data/linc-oe/rel/files/sys.config sys.config.template
5. chmod +x /data/onos/tools/test/bin/onos-netcfg(更改权限)

## 验证IP+光
cd /data/mininet/custom
sudo -E python topos/opticalTest.py应该能够启动

## 常见问题
1. LINC的最后make报错
![](_v_images/20191016110247647_26854.png)
解决：删除erlang环境 重新尝试其他版本。一般是由于Ubuntu版本和apt包版本有冲突导致，虽然Erlang环境安装没问题

2. sysconfig问题
```bash
*** Creating sys.config...
/data/LINC-config-generator/config_generator TopoConfig.json /data/LINC-config-generator/sys.config.template 127.0.0.1 6653
***ERROR: Error creating sys.config file: escript: exception error: no match of right hand side value {error,enoent}
  in function  config_generator:parse/4 (src/config_generator.erl, line 46)
  in call from escript:run/2 (escript.erl, line 747)
  in call from escript:start/1 (escript.erl, line 277)
  in call from init:start_it/1 
  in call from init:start_em/1 

*** Starting CLI:
mininet> 
```

报错原因:找不到/data/LINC-config-generator/sys.config.template这个路径下的文件，修改为/data/LINC-config-generator/priv/sys.config.template，或者    `mv sys.config.template ..`让其找到对应文件即可



http://tieba.baidu.com/p/6316348925?traceid=

![](_v_images/20191104151758077_24013.png)