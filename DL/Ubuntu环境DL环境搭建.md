# Ubuntu DL环境

## 环境

系统：16.04 64-bit

CPU： i7-8750H CPU @ 2.20GHz × 12

GPU：GT 1050Ti



# Terminal 代理配置

目前国内的 Anaconda 镜像都不再可用，使用 `conda` 命令下载软件会非常的慢，这时候可以为终端配置代理，通过此方式加速软件的下载。如何获取一个代理服务器，这个可以在网上找到很多方法，不在赘述。

Ubuntu 配置终端代理的方式可以参考下面两篇文章：

- [ubuntu系统终端与浏览器走代理（SS）配置](https://blog.csdn.net/happen_if/article/details/90151403)
- [Ubuntu系统下浏览器和终端的SS代理配置](https://blog.csdn.net/jesse_mx/article/details/52863204)

## 代理配置

**首先：安装 ss 的客户端 shadowsocks-qt5**

```bash
sudo add-apt-repository ppa:hzwhuang/ss-qt5 
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```

在 shadowsocks-qt5 里面配置 ss 服务器，具体的shadowsocks-qt5 的使用方式可以网上自己找。配置好之后，需要安装终端代理软件 **polipo**。

**其次：安装 `polipo`**

```bash
sudo apt-get install polipo
```

**再次：配置 `polipo`**

```bash
cd /etc/polipo/
sudo chmod 777 config # 为config文件申请最高权限
vi /etc/polipo/config # 打开进行编辑
```

添加如下内容：

```bash
socksParentProxy = "localhost:1080" # 代理服务器
socksProxyType = socks5 # 协议
logLevel=4
```

**最后：`polipo` 的使用**

```bash
sudo service polipo stop
sudo service polipo start
```

**验证及其使用**

```bash
 curl ip.gs #查询你的IP地址和地理信息
```

此时会查询到你当前网络的服务商，

```
Current IP / 当前 IP: ×××.××.×××.×××
ISP / 运营商:  DRPENG
City / 城市: Beijing Beijing
Country / 国家: China
```

然后执行下面的命令：

```bash
http_proxy=http://localhost:8123 curl ip.gs
```

此时再看到的就是代理服务器的地址：

```
Current IP / 当前 IP: ×××.××.×××.××
ISP / 运营商:  it7.net
City / 城市: Los Angeles California
Country / 国家: United States
```

如果不是，需要自己检查哪里出现了问题。

`8123` 是默认端口，也可以根据需要进行修改

**设置别名**

上面每次想要使用代理都需要输入一窜命令有点麻烦，可以为其创建一个别名，操作如下：

```bash
# 打开配置文件
vi .bashrc
# 设置别名
alias hp="http_proxy=http://localhost:8123" 
# 让配置生效
source ~/.bashrc
```

这样命令就可以简化成如下形式：

```bash
hp curl ip.gs
```

**为当前会话设置全局代理**

为每一条命令都加上 `hp` 显然是比较麻烦的，我们可以让**当前终端**所有的命令都都经过代理，设置如下：

```bash
export http_proxy=http://localhost:8123 # 当前终端使用代理
unset http_proxy # 当前终端取消代理
```

更长久的配置可以将其配置到 `.bashrc` 里面。

这样的配置对于直接访问网址的命令来说是有用的，但是对于  `conda` 病不起作用，具体的 `conda` 配合可以参考下文。

## conda 配置

上面配置好了之后，`conda` 不一定能用，这时候需要通过 `.condarc` 为 conda 配置代理服务器，配置的方式可以参考：[如何设置anaconda代理](https://blog.csdn.net/hellocsz/article/details/86520360)、[给anaconda设置代理来访问包来源](https://blog.csdn.net/mx_windforce/article/details/53006966)

打开文件添加自己的代理服务器：

```
proxy_servers:
    http: http://127.0.0.1:1080
    https: http://127.0.0.1:1080
```

如果使用的是 ss 服务器，这时的代理协议应该是 `socks5`（适自己的配合而定），比如此处我使用的 `shadowsocks-qt5` 的默认代理协议就是 `socks5`，那么此时的配置如下：

```
proxy_servers:
    http: socks5://127.0.0.1:1080
    https: socks5://127.0.0.1:1080
```

这样使用 `conda` 命令就会走代理了。



# Anaconda

## 安装



## 镜像源

安装完成，可以在 `～/.condarc` 中看到当前可用的数据源（当前国内镜像源已经不可用，镜像源的添加方式就不在此给出）：

```json
channels:
  - https://mirrors.cloud.tencent.com/anaconda/cloud/msys2/
  - https://mirrors.cloud.tencent.com/anaconda/cloud/conda-forge/
  - https://mirrors.cloud.tencent.com/anaconda/cloud/pytorch/
  - https://mirrors.cloud.tencent.com/anaconda/pkgs/free/
  - defaults
show_channel_urls: true
```

不想使用的镜像源直接删除即可，想要使用哪个数据源直接加入即可。





## 卸载

[Ubuntu16.04上删除Anaconda2](https://blog.csdn.net/hhy_csdn/article/details/82116486)



# NVIDIA驱动安装

参考：

[Ubuntu 16.04安装NVIDIA驱动](https://blog.csdn.net/CosmosHua/article/details/76644029)

---

安装NVIDIA驱动的方式有多种，PPA安装的方式并不是特别的可靠，因此选择使用 runfile的安装方式。

**首选：卸载已经安装的 N 卡**

若使用的是 `apt-get` 方式安装，则使用如下方式

```bash
sudo apt-get remove --purge nvidia*
```

若使用runfile 安装的则使用此方式

```bash
sudo chmod +x NVIDIA-*.run
sudo ./NVIDIA-*.run --uninstall
```

**然后：禁用 `nouveau`**

```bash
sudo gedit /etc/modprobe.d/blacklist.conf
```

在文件里面添加：

```
blacklist nouveau
options nouveau modeset=0
```

这里是禁用的第三方应用，然后执行

```bash
sudo update-initramfs -u
```

重启之后，执行

```bash
lsmod | grep nouveau
```

若没有输出说明禁用成功。

**之后：禁用图形界面**

```bash
sudo service lightdm stop
```

使用 `Ctrl + Alt + F1` 进入命令行，登进去。

**最后：安装驱动**

为驱动赋予执行权限，然后执行安装命令

```bash
#给驱动run文件赋予执行权限：
sudo chmod +x NVIDIA-Linux-x86_64-384.59.run
#后面的参数非常重要，不可省略：
sudo ./NVIDIA-Linux-x86_64-384.59.run –no-x-check -no-nouveau-check -no-opengl-files
```

- `–no-opengl-files`：表示只安装驱动文件，不安装OpenGL文件。这个参数不可省略，否则会导致登陆界面死循环，英语一般称为”login loop”或者”stuck in login”。
- `–no-x-check`：表示安装驱动时不检查X服务，非必需。
- `–no-nouveau-check`：表示安装驱动时不检查nouveau，非必需。
- `-Z, --disable-nouveau`：禁用nouveau。此参数非必需，因为之前已经手动禁用了nouveau。

- `-A`：查看更多高级选项。



**测试**

```bash
nvidia-smi #若列出GPU的信息列表，表示驱动安装成功
nvidia-settings #若弹出设置对话框，亦表示驱动安装成功
```





# CUDA9.x安装

参考：

[CUDA9.1、cuDNN7在Ubuntu16.04上的安装](https://blog.csdn.net/jonms/article/details/79318566#chatqa)

## 检查环境

**检查本机GPU情况**

```bash
lspci | grep -i nvidia
```

**检查系统是否支持CUDA**

```bash
uname -m && cat /etc/*release
```

**验证 gcc**

```
gcc --version
```

若没有则进行安装

```bash
 sudo apt install gcc
```

验证系统是否安装了 kernel header和 package development

```bash
uname -r
sudo apt install linux-headers-$(uname -r)
```

安装 runfile

```bash
sudo apt install gcc-multilib
```

## 安装

此处通过 runfile 安装，首先去 NVIDIA官网下载自己需要的 [CUDA](https://developer.nvidia.com/cuda-90-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=runfilelocal) 版本。

关闭图形化界面

```bash
sudo service lightdm stop
```

之后按 `Alt + Ctrl + F1` 进入命令行，登录之后运行

```bash
sudo sh cuda_9.0.176_384.81_linux.run
```

按照提示安装即可，**如果单独安装过 NVIDIA的驱动，安装 CUDA 的时候就不要选择安装 NVIDIA驱动了，要不会出现问题。同时也不要安装OpenGL**。

如果安装的过程出现少包的情况，通过以下方式进行安装：

```bash
sudo apt install freeglut3-dev build-essential libx11-dev libxmu-dev libxi-dev libgl1-mesa-glx libglu1-mesa libglu1-mesa-dev
```

安装完成之后，重启图形界面

```bash
sudo service lightdm restart
```

验证是否安装成功：

```bash
ls /dev/nvidia*
```

若出现四条则表示安装成功，但我本机安装完成之后只出现两条，解决办法如下：

首先，创建一个启动脚本

在 `/etc/` 下创建一个 `rc.local` 脚本，脚本的内容如下：

```bash
/sbin/modprobe nvidia

if [ "$?" -eq 0 ]; then
  # Count the number of NVIDIA controllers found.
  NVDEVS=`lspci | grep -i NVIDIA`
  N3D=`echo "$NVDEVS" | grep "3D controller" | wc -l`
  NVGA=`echo "$NVDEVS" | grep "VGA compatible controller" | wc -l`

  N=`expr $N3D + $NVGA - 1`
  for i in `seq 0 $N`; do
    mknod -m 666 /dev/nvidia$i c 195 $i
  done

  mknod -m 666 /dev/nvidiactl c 195 255

else
  exit 1
fi

/sbin/modprobe nvidia-uvm

if [ "$?" -eq 0 ]; then
  # Find out the major device number used by the nvidia-uvm driver
  D=`grep nvidia-uvm /proc/devices | awk '{print $1}'`

  mknod -m 666 /dev/nvidia-uvm c $D 0
else
  exit 1
fi
```

重启之后，重新查看安装成功与否会出现如下错误：

```text
ls: cannot access '/dev/nvidia*': No such file or directory
```

但好像并不影响使用，不行就太多重启几次就好了。

安装完成之后，设置环境变量，在 `.bashrc` 中添加

```bash
export PATH=/usr/local/cuda-9.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64:$LD_LIBRARY_PATH
```



验证驱动的版本

```bash
cat /proc/driver/nvidia/version
```

```
NVRM version: NVIDIA UNIX x86_64 Kernel Module  390.87  Tue Aug 21 12:33:05 PDT 2018
GCC version:  gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.11) 
```

验证 CUDA 版本

```bash
nvcc -V
```

```
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2017 NVIDIA Corporation
Built on Fri_Sep__1_21:08:03_CDT_2017
Cuda compilation tools, release 9.0, V9.0.176
```

**运行 CUDA 的例子**：

进入相关目录，进行例子编译

```bash
cd /home/user_name/NVIDIA_CUDA-9.1_Samples # 例子的位置，视安装位置而定
make -j
```

等十几分钟，编译完成（**编译的时候如果不添加 `-j` 有可能编译不通过**），如果出现编译错误系统会报错。编译完成的二进制文件在 `NVIDIA_CUDA-9.1_Samples/bin` 中，接着运行下面命令：

```bash
cd bin/x86_64/linux/release
./deviceQuery  
```

最后检查系统与 `CUDA-Capable device` 的链接情况：

```bash
./bandwidthTest
```

出现如下信息，说明安装成功。

**第二种编译方式**

上面的编译方式是把所有的例子全部编译，我们也可以只选择编译其中的一项，如设备查看的选项

```bash
cd /home/latelee/NVIDIA_CUDA-9.0_Samples/1_Utilities/deviceQuery
# 编译
make -j
# 设备查询
./deviceQuery
```

之后就会输出设备的相关信息

```text
./deviceQuery Starting...

 CUDA Device Query (Runtime API) version (CUDART static linking)

Detected 1 CUDA Capable device(s)

Device 0: "GeForce GTX 1050 Ti"
  CUDA Driver Version / Runtime Version          9.1 / 9.0
  CUDA Capability Major/Minor version number:    6.1
  Total amount of global memory:                 4040 MBytes (4236312576 bytes)
  ( 6) Multiprocessors, (128) CUDA Cores/MP:     768 CUDA Cores
  GPU Max Clock rate:                            1620 MHz (1.62 GHz)
  Memory Clock rate:                             3504 Mhz
  Memory Bus Width:                              128-bit
  L2 Cache Size:                                 1048576 bytes
......
```



## 卸载

CUDA 自带卸载工具，手下进入到 cuda 的 bin 目录

```bash
cd /usr/local/cuda/bin
```

然后运行 uninstall 开头的 `.pl` 文件

```bash
sudo ./uninstall_cuda_9.0.pl
```



# 安装 CuDNN

参考：

[安装cuDNN-7.5.0](https://blog.csdn.net/huang826336127/article/details/86670925)

[关于CUDA和cuDNN的安装](https://www.cnblogs.com/blog4ljy/p/9281592.html)

[ubuntu 17.10 + CUDA 9.0 + cuDNN 7 + tensorflow源码编译](https://zhuanlan.zhihu.com/p/30781460)

---

安装之前需要先卸载之前的版本，删除之前的版本，删除的方式如下：

```bash
# 删除原来存在的cudnn文件
sudo rm -rf /usr/local/cuda/include/cudnn.h
sudo rm -rf /usr/local/cuda/lib64/libcudnn*
```

因为前面是重新安装 CUDA，因此就不需要删除原来的文件。



根据安装的 CUDA 版本，选择合适的 [CuDNN](https://developer.nvidia.com/cudnn) 。因为此处安装的是 `CUDA 9.0` 因此选择 [Download cuDNN v7.6.0 (May 20, 2019), for CUDA 9.0](https://developer.nvidia.com/rdp/cudnn-download#a-collapse760-90) 。

下载完成之后，将其解压到 CUDA 的安装目录即可。

```bash
tar -zxvf cudnn-9.0-linux-x64-v7.6.0.64.tgz
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

之后将 CUDA 加入到系统的环境变量里面：

```bash
gedit ~/.bashrc
export PATH=${PATH}:/usr/local/cuda-9.0/bin
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}/usr/local/cuda-9.0/lib64:/usr/local/cuda-9.0/lib:/usr/local/cuda/extras/CUPTI/lib64
```

让环境变量生效即可。



# OpenCV安装

参考：[Ubuntu中opencv3的编译安装](https://blog.csdn.net/otengyue/article/details/80410569)

[Ubuntu16.04卸载其他版本的opencv并安装opencv3.2.0+opencv contrib3.2.0](https://blog.csdn.net/qq_29229045/article/details/78527391)

安装必要依赖包

```bash
sudo apt-get update
sudo apt-get upgrade

sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev libjpeg62-dev libtiff4-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
```

github下载 OpenCV 源码，切换到自己想要的版本，这里以 3.2.0 为例

```bash
git clone https://github.com/opencv/opencv.git
cd opencv
git checkout 3.2.0
git pull

git clone https://github.com/opencv/opencv_contrib.git
cd opencv_contrib   
git checkout 3.2.0
git pull
```

设置 `cmake` 参数

```
cd opencv
mkdir build
cd build

# 不带opencv_contrib的编译
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..

# 或者，带opencv_contrib的编译
sudo cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=OFF \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_EXTRA_MODULES_PATH=/home/lq/github/opencv_contrib/modules \
    -D BUILD_EXAMPLES=OFF \
    -D WITH_LAPACK=OFF \
    -D WITH_IPP=OFF ..
```

- CMAKE_INSTALL_PREFIX：安装的python目录前缀，其实就是指定了python模块的安装路径：CMAKE_INSTALL_PREFIX/lib/python2.7/dist-packages。获取该路径的方式可以用python -c "import sys; print sys.prefix"
- PYTHON_EXCUTABLE：指定python路径,在电脑上有多个版本的python时，可以通过该方式指定哪个python版本使用

- OPENCV_EXTRA_MODULES_PATH: 指定opencv所需模块的路径，就是之前我们所说的contrib



编译安装

```bash
make -j8 
sudo make install
sudo ldconfig	
```

测试

```bash
# 安装版本
pkg-config --modversion opencv
# 安装位置
pkg-config --cflags opencv 
pkg-config --libs opencv
```





# caffe2 安装

两种安装方式，一是源码编译安装，一种是通过 anaconda 安装。



## anaconda方式安装

如果是 CUDA8 和 CuDNN7 可以通过如下方式安装

```
conda install pytorch-nightly cuda80 -c pytorch
```

CUDA9 和 CuDNN 7 通过如下方式安装

```
conda install pytorch-nightly -c pytorch
```

目前本机的CUDA是8，因此这里选择 cuda80的版本。

注：这种方式安装有可能让 DIGITS 运行不起来。因此推荐从源码安装

## 源码安装

参考：

[源码安装 caffe2](https://caffe2.ai/docs/getting-started.html?platform=ubuntu&configuration=compile#install-with-gpu-support)

[Pytorch源码编译简明指南](https://oldpan.me/archives/pytorch-build-simple-instruction)

---

安装相关依赖：

```bash
sudo apt-get update
sudo apt-get install -y --no-install-recommends \
      build-essential \
      git \
      libgoogle-glog-dev \
      libgtest-dev \
      libiomp-dev \
      libleveldb-dev \
      liblmdb-dev \
      libopencv-dev \
      libopenmpi-dev \
      libsnappy-dev \
      libprotobuf-dev \
      openmpi-bin \
      openmpi-doc \
      protobuf-compiler \
      python-dev \
      python-pip                          
pip install --user \
      future \
      numpy \
      protobuf \
      typing \
      hypothesis
```

对于 Ubuntu 16.04 使用以下命令

```bash
sudo apt-get install -y --no-install-recommends \
      libgflags-dev \
      cmake
```

## 通过 pytorch 安装

Github 下载 pytorch，之后进入 pytorch 目录：

```bash
git clone https://github.com/pytorch/pytorch.git && cd pytorch
```

更新项目的子模块

```bash
git submodule update --init --recursive
```

安装 pytorch

```bash
python setup.py install
```

默认安装会编译CUDA，如果不想编译 CUDA 可以使用如下的命令：

```Python
NO_CUDA=1 python setup.py install
```

这种安装方式会引起较多问题，pytorch 和 caffe2 会有兼容问题。

## 通过 caffe2 安装

参考：

[ubuntu 16.04下安装caffe2](https://blog.csdn.net/xiangxianghehe/article/details/70171342)

[ubuntu16.04中安装caffe2 和 detectron](https://blog.csdn.net/cym1990/article/details/79409476)

github 下载 caffe2 源码

```bash
git clone --recursive https://github.com/caffe2/caffe2.git
```

编译并安装：

```bash
cd caffe2
make 
cd build 
sudo make install
```



可能遇到的问题

没有安装OpenCV，导致make 的时候，出现不支持 CUDA9.0，可以安装 OpenCV，或者 make的时候添加 `-DUSE_OPENCV=OFF` 选项。



为了能与 DIGITS 更好的配合，可以参考此方式安装，<https://github.com/NVIDIA/DIGITS/blob/digits-6.0/docs/BuildCaffe.md> ，以上面的安装略不同。



## 测试是否安装成功

检查pytorch是否安装成功：



```bash
cd ~ && python -c 'from caffe2.python import core' 2>/dev/null && echo "Success" || echo "Failure"
```

或者

```bash
python -c 'from caffe2.python import core' 2>/dev/null && echo "Success" || echo "Failure"
```

如果是failure，试着cd到caffe2/build的文件夹里，然后执行

```bash
python -c 'from caffe2.python import core' 2>/dev/null
```

如果successful，说明是环境变量的设置问题，如果还是失败，则会有具体的提示。



配置环境变量（以下是通过 caffe2 编译安装，而非pytorch ），编辑`~/.bashrc`

```bash
sudo gedit ~/.bashrc
```

添加以下内容：

```bash
export PYTHONPATH=/usr/local:$PYTHONPATH
export PYTHONPATH=$PYTHONPATH:/home/....../caffe2/build  (后面路径为caffe2的编译路径，在caffe2/build中，命令行输入pwd可以得到这个路径)
export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
```

---



## 卸载 Caffe

如果是源码安装 pytorch，卸载可以按照如下方式：

```bash
pip uninstall torch
python setup.py clean
```

查询是否安装某个软件可以使用如下命令：

```bash
pip freeze
# 或者
pip list
```



# 安装 NVIDIA DIGITS

通过源码安装 [DIGITS](https://github.com/NVIDIA/DIGITS)，目前版本为 `6.1.1` ，安装方式可以参考 [Building DIGITS](https://github.com/NVIDIA/DIGITS/blob/master/docs/BuildDigits.md)，下载源码：

```bash
git clone https://github.com/NVIDIA/DIGITS.git
```

进入 DIGITS 目录，安装必要的依赖：

```bash
sudo pip install -r $DIGITS_ROOT/requirements.txt
```

安装完成之后，通过如下脚本启动 DIGITS：

```bash
./digits-devserver
```

之后就可以使用 DIGITS，访问地址如下：`ttp://localhost:5000/`





# DIGITS with TensorFlow

[Installing TensorFlow](https://github.com/NVIDIA/DIGITS/blob/master/docs/BuildTensorflow.md)



# DIGITS中 使用 Caffe2

参考：

[不用写代码就能实现深度学习？手把手教你用英伟达 DIGITS 解决图像分类问题](https://www.cnblogs.com/hanson1/p/7105307.html)

---

目前使用 DIGITS 提供的 caffe 可以安装成功，地址为：<https://github.com/NVIDIA/DIGITS/blob/master/docs/BuildCaffe.md>



# 常见问题

## NVIDIA重启无效

Ubuntu 使用的是双系统，在每次重新系统时候 `nvidia-smi` 就会无效，需要重新安装显卡驱动，可以尝试如下的方法解决：

第一种说发是关闭Ubuntu的软件自动更新：

[【已解决】Ubuntu18.04下NVIDIA GPU驱动程序运行一段时间后出错“NVIDIA-SMI has failed”](https://blog.csdn.net/cskywit/article/details/85211884)



第二中方法是说重新安装 `dkms` ，通过 `dkms` 安装 NVIDIA的驱动（**此方法测试可用**）：

[无法连接NVIDIA驱动：NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver](https://blog.csdn.net/hangzuxi8764/article/details/86572093)

```
sudo apt-get install dkms
sudo dkms install -m nvidia -v 384.81
```

在 `/usr/src` 可以查看驱动的版本号。