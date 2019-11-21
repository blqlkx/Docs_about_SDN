# mininet安装指南

## 修改环境

### 更新python环境
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

python
>>> import sgp4        #    无报错，即环境配置成功
```

### 拉代码(算了还是直接从129机器拷贝吧)

```bash
cd /data
scp -r root@47.00.129.129:/data/mininet .

cd mininet/utils
./install.sh        #一般不会有错
mn        # 验证，出现mininet> 就行
```
