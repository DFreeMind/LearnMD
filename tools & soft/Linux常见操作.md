# 非 root 用户安装 python3

- [给非 root 用户安装自己的 Python3 和 pip3](https://zcdll.github.io/2018/01/29/own-python-pip/)

- [Linux环境（服务器）下非root用户安装Python3.6](https://www.jianshu.com/p/c45af0ddd68a)



**首先，下载所需要的python**

找到自己需要的 python版本，地址为：[https://www.python.org/ftp/python/](https://link.jianshu.com/?t=https://www.python.org/ftp/python/) 。此处安装 python3.5.7

下载python安装文件：

```bash
wget https://www.python.org/ftp/python/3.5.7/Python-3.5.7.tgz
```

解压下载的文件，并进入目录解压目录

```bash
tar -xzvf Python-3.5.7.tgz
cd Python-3.5.7
```

编译安装

```bash
# 设置安装的路径
./configure --prefix=/home/lq/software/python3
# 编译、安装
make
make install
```



之后，设置环境变量















