+++
author = "causehhc"
title = "ubuntu1604升级python版本到37"
date = "2021-04-11"
description = ""
tags = ["Ubuntu","Python"]
categories = ["嵌入式"]
+++

1. 下载一些依赖项
```
sudo apt-get install \
python-dev \
python-setuptools \
python-pip \
python-smbus \
build-essential \
libncursesw5-dev \
libgdbm-dev \
libc6-dev \
zlib1g-dev \
libsqlite3-dev \
tk-dev \
libssl-dev \
openssl \
libffi-dev
```
2. 新建临时文件夹并进入，**完成测试后可将本文件夹删除**

```
mkdir py37_tmp && cd py37_tmp
```
3. 下载py37本体

```
wget https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tgz
```
4. 解压

```
tar zxvf Python-3.7.3.tgz
cd Python-3.7.3
```
5. 编译

```
./configure --with-ssl
make
sudo make install
```
**注意**：–with-ssl必须加上，否则使用pip安装第三方包时，会引发ssl错误。导致无法使用。
6. 查找py37位置

```
whereis python3.7
```

![111.jpg](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b75af98fd3394da8b208df09ac788ebd~tplv-k3u1fbpfcp-watermark.image)
7. 删除原py35版本的软链接

```
sudo rm -rf /usr/bin/python3
sudo rm -rf /usr/bin/pip3
```
8. 创建软链接

```
# 添加python3的软链接
sudo ln -s /usr/local/bin/python3.7 /usr/bin/python3.7
# 添加 pip3 的软链接
sudo ln -s /usr/local/bin/pip3.7 /usr/bin/pip3.7
```
9. 测试

```
python3 -V
pip3 -V
```
![2222.jpg](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2b909b55bf3742e4a988c0528760848f~tplv-k3u1fbpfcp-watermark.image)

## 一些问题
在使用pip3安装包时报错：

```
subprocess.CalledProcessError: Command '('lsb_release', '-a')' returned non-zero exit status 1.
```


![11111.jpg](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2b53b566bb6e433c886067816aab3429~tplv-k3u1fbpfcp-watermark.image)
解决方案`sudo rm /usr/bin/lsb_release`


