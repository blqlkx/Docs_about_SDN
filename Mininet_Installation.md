# mininet安装指南


## 更新python环境
1. 将python的软连替换为python3， pip的软连替换为pip3
2. 步骤：

```bash
which python
which python3        #一般都在同一个目录下
cd /usr/bin
ll *python*
rm -rf python
ln -s python python3.X    # 3.5\3.6哪个存在替换哪个
python    # 验证：cmd敲python，显示版本为python3.x就行

which pip        #查找到pip所在目录，cd
mv pip pip2
apt install python3-pip        # 有时需要先执行apt update&&apt upgrade
which pip3        #回显有路径即可, cd目录
mv pip3 pip
pip install sgp4        #执行两遍，第二遍会提示already installed sgp4 in DIR, 记录下安装目录
pip install bottle

python
>>> import sgp4        #    无报错，即环境配置成功
```

## 拉代码(算了还是直接从129机器拷贝吧)

```bash
cd /data
scp -r root@47.00.129.129:/data/mininet .

cd mininet/utils
./install.sh        #一般不会有错
mn        # 验证，出现mininet> 就行
```
## 拉代码from git
**注：目前代码未开源，所以能scp尽量scp，需要从git拉代码就联系我加公钥**
### git配置
1. 下载
CentOS用户使用`yum install git`， Ubuntu用户选择`apt install git`
2. git配置
```bash
git config --global user.name "Your Name"        # name填写blqlkx
git config --global user.email "email@example.com"        # email填写871640094@qq.com
git config --list        # 查看配置是否生效
```
3. 生成ssh key

```bash
ssh-keygen -t rsa -C your_email@youremail.com        # 一路会车
cd ~/.ssh
cat id_rsa.pub        #获取公钥
```

4. 公钥在github上配置完成后即可拉代码

## mininet安装
1. git clone git@github.com:blqlkx/mininet_satellite.git
2. mv mininet_satellite mininet
3. cd mininet/utils
4. ./install.sh
5. 如果改了代码需要再次install，只需执行./install.sh -n

## 启动
lkx.py是IP+光拓扑，和opticalTest差不多，6sw2ho是纯IP拓扑， LINCOE是卫星拓扑（还没修改完）
```bash
cd mininet/custom        # 有一些test拓扑
sudo -E python XXX.py        # 都是这么启动，包括IP+光
cd mininet/custom/Satellites/src
sudo -E python LINCOE.py        # 卫星拓扑
```