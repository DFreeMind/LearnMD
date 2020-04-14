`DIGITS` 平台相关环境的安装均可以在项目的 github 主页的 `docs` 文件夹中找到，![image](https://wx4.sinaimg.cn/large/69d4185bgy1g451nykc2hj20rr0m4n1v.jpg) 

以下操作均是在 Ubuntu 系统上，若是在windows或者 centos，安装方式需要自行查找。



# Caffe安装

caffe 的安装可以参照 `BuildCaffe.md` 文件，英伟达提供的 caffe 对 DIGITS 的兼容性只最好的，从 caffe 官网安装遇到的问题会比较多，解决起来比较麻烦。这里就是使用英伟达提供的 Caffe 版本，安装Caffe 之前确保GPU驱动、CUDA、cuDNN都已经安装。

**首先，安装 caffe 需要的相关依赖。**

```bash
sudo apt-get install --no-install-recommends build-essential cmake git gfortran libatlas-base-dev libboost-filesystem-dev libboost-python-dev libboost-system-dev libboost-thread-dev libgflags-dev libgoogle-glog-dev libhdf5-serial-dev libleveldb-dev liblmdb-dev libopencv-dev libsnappy-dev python-all-dev python-dev python-h5py python-matplotlib python-numpy python-opencv python-pil python-pip python-pydot python-scipy python-skimage python-sklearn
```

**之后，github下载英伟达提供的caffe**

创建caffe的安装目录，设置 caffe 安装的环境变量

```bash
mkdir ～/caffe
export CAFFE_ROOT=～/caffe
```

下载 caffe，此处安装的是 0.15 版本，并切换版本

```bash
git clone https://github.com/NVIDIA/caffe.git $CAFFE_ROOT -b 'caffe-0.15'
```

**然后，安装caffe的 Python 依赖**

```bash
sudo pip install -r $CAFFE_ROOT/python/requirements.txt
```

**最后，构建 caffe**

```bash
cd $CAFFE_ROOT
mkdir build
cd build
cmake ..
make -j"$(nproc)" #使用全部的内核进行编译，加快编译速度
make install
```

卸载的话，可以在 build 目录中执行

```bash
make uninstall
```



DIGITS 也支持 tensorflow，他的安装比较简单，只需执行下面命令即可：

```bash
sudo pip install tensorflow-gpu
```



# DIGITS 安装

**首先，安装必要依赖**

```bash
sudo apt-get install --no-install-recommends git graphviz python-dev python-flask python-flaskext.wtf python-gevent python-h5py python-numpy python-pil python-pip python-scipy python-tk
```



**之后，下载源码**

可以设置一个环境变量方便以后的操作，也可以不设置

```bash
DIGITS_ROOT=~/digits
git clone https://github.com/NVIDIA/DIGITS.git $DIGITS_ROOT
```



**然后，安装 Python 依赖**

```
sudo pip install -r $DIGITS_ROOT/requirements.txt
```



**最后，运行 DIGITS**

```bash
./digits-devserver
```

也可以将 digits 安装到系统中，这样就可以直接运行 `digits-devserver` 命令

```bash
sudo pip install -e $DIGITS_ROOT
```

若通过系统运行有问题，出现找不到 caffe 安装目录的情况，可以还按照第一种方式启动。

DIGITS 运行的端口为 `5000`，访问 `http://localhost:5000/` 即可。运行结构如下：

![image](https://wx1.sinaimg.cn/large/69d4185bgy1g452l15vjoj20ts0ffab8.jpg)



# 首页介绍

主要是三大部分：

- `Datasets` 数据集，用于对数据集的管理，已添加的数据集都会再次列出

![image](https://ws4.sinaimg.cn/large/69d4185bgy1g453v55i9jj20rq09xaaq.jpg)

- `Models` 模型管理模块，用于添加和训练模型，以训练好的模型都会再此列出

![image](https://ws2.sinaimg.cn/large/69d4185bgy1g453wu8rhaj20rj0ardgn.jpg)

- `Pretrained Models` 预训练模型管理模块，主要用于预训练模型的添加和删除

![image](https://ws4.sinaimg.cn/large/69d4185bgy1g453ye5cjbj20ra092mxp.jpg)

---

首页上也会显示出当前可用的显卡数量，还可以查看当前 DIGITS的版本，使用的caffe的版本

![image](https://wx2.sinaimg.cn/large/69d4185bgy1g4540g2d69j20bo096t8z.jpg)

# 数据集管理

DIGITS 本身提供了两种开源的数据集 `mnist` 和 `cifar` ，此处以 `mnist` 手写数字数据集为例。首先下载数据集，根据需要选择需要存储的位置

```bash
python -m digits.download_data mnist ~/mnist
```

下载完成之后，数据如下：

![image](https://wx2.sinaimg.cn/large/69d4185bgy1g4546m0rj3j20jh09w75m.jpg)

其中 `train` 是训练数据集，`test` 为测试数据集

![image](https://ws3.sinaimg.cn/large/69d4185bgy1g4548lkev2j20qm0c2wgc.jpg)



数据集下载完成之后，就可以对数据集进行配置。点击 `Images` 选项，根据目的选择一个合适的类型，此处使用的是 `mnist` 数据集，因此选择 `Classification` 选项。

![image](https://wx1.sinaimg.cn/large/69d4185bgy1g454ala16ij209m07hmxc.jpg)

最左侧，配置图片的属性信息：

- `Image Type` 选项选择 `Grayscale`，因为 `mnist` 的图片并不是彩色图片，
- `mnist` 图片的大小为 28×28
- `Resize Transformation` 图片的转换方式，主要用于图片不符合设定的大小时，怎么样将其处理成目标大小，根据需要记性选择，此处选择默认

![image](https://ws4.sinaimg.cn/large/69d4185bgy1g454iqrklej20bf099glu.jpg)

右侧对数据集配置

- `Training Images`选择训练数据的位置，此处为 `/home/lq/dataset/mnist/train`
- `Minimum samples per class`每个类最小采样数量，可以默认
- `% for validation`  训练数据集分出多少用作 `validation` ，根据需要进行选择，此处默认
- 勾选 `Separate test images folder`，设置测试数据集的位置，此处为 `/home/lq/dataset/mnist/test`

![image](https://ws3.sinaimg.cn/large/69d4185bgy1g454utmgp8j20lg0f5q3u.jpg)

---

数据记还有 `Use Text Files` 和 `Use S3` 两种选项，按需选择即可。

最后设置组名和数据集的名字

![image](https://wx4.sinaimg.cn/large/69d4185bgy1g454rf1qvrj20gr0aoq38.jpg)

点击创建，进入数据处理阶段

![image](https://wx1.sinaimg.cn/large/69d4185bgy1g454smyhbvj20rm0ixq57.jpg)

在左侧列表还可以查看相关的数据统计信息：

- 训练集每个种类中图片的数量

![image](https://wx4.sinaimg.cn/large/69d4185bgy1g454wuwd5xj20ld0ekt94.jpg)

- 验证集

![image](https://wx4.sinaimg.cn/large/69d4185bgy1g454xyl489j20l90g2aam.jpg)

- 测试集

![image](https://ws1.sinaimg.cn/large/69d4185bgy1g454yp7zjcj20lg0fydgf.jpg)

处理完成（数据的大小及其机器的性能会极大的影响处理的时间），在数据集列表中就可以查看到。

![image](https://ws4.sinaimg.cn/large/69d4185bgy1g45503pntvj20x20c1dgr.jpg)

# 模型训练

数据集创建好了，就可以去创建模型，因为是要做手写字的分类，因此此处依然选择 `classification`

![image](https://wx2.sinaimg.cn/large/69d4185bgy1g4552hpxsxj207806djri.jpg)

进去之后，在上部左侧，选择我们新建的 `MNIST_28x28` 数据集，`Python Layers` 暂时不用设置

![image](https://ws4.sinaimg.cn/large/69d4185bgy1g45547mculj20ad0jxwfc.jpg)

上部中间设置模型训练的参数

- `Training epochs` 设置训练多少代，值越大训练的时间越长，`MNIST` 数据集较小，此处默认 30 即可
- `Snapshot interval` 多少代生产一次快照
- `Validation interval` 多少代进行一次 `validation`
- `Batch size` 批次的大小，值越大需要的GPU空间越多
- `Solver type` 使用的优化的算法类型，有多个选项，此处默认选择 `SGD` 即可
- `Base Learning Rate` 学习速率，对最终模型的精度和训练的速度都有影响

右侧的 `Data Transformation` 默认即可

![image](https://ws2.sinaimg.cn/large/69d4185bgy1g455b8ce7kj20lg0k7ta1.jpg)

最下部模型设置，有四个可供选择

- `Standard Networks` DIGITS 内置的模型，有 `caffe` 和 `tensorflow` 两种可供选择

![image](https://ws2.sinaimg.cn/large/69d4185bgy1g455dvxqbzj20w709fjs3.jpg)

- `Previous Networks` 之前训练的模型

![image](https://ws4.sinaimg.cn/large/69d4185bgy1g455e7pstkj20w908kjru.jpg)

- `Pretrained Netwroks` 其他的与训练模型

![image](https://wx1.sinaimg.cn/large/69d4185bgy1g455f0ltnlj20wl07274k.jpg)

- `Custom Network` 自定义模型

![image](https://wx2.sinaimg.cn/large/69d4185bgy1g455ft142wj20ww07zwes.jpg)



此处我们选择内置的caffe版本的 `LeNet`模型，此模型要求的图片尺寸符合我们的要求，设置模型的名称和分组，点击创建即可开始训练

![image](https://ws4.sinaimg.cn/large/69d4185bgy1g455id1l53j20wc0gegmr.jpg)

可以观察实时训练过程：

![image](https://ws1.sinaimg.cn/large/69d4185bgy1g45693dew0j20wo0kp41f.jpg)

训练损失、验证精度及其损失等

![image](https://ws2.sinaimg.cn/large/69d4185bgy1g456dianptj20lc0g1aar.jpg)

这时候就可以在 `Models` 列表中看到我们训练的模型

![image](https://ws1.sinaimg.cn/large/69d4185bgy1g459n0i0xsj20xk0cr75d.jpg)



# 模型使用

经过一段时间之后，模型训练好就可以在 `Trained Models` 就可以看到相关信息。

![image](https://ws2.sinaimg.cn/large/69d4185bgy1g458y24pvhj20kk052mxo.jpg)

`Select Model` 可以选择要操作的是哪一个模型，这个列表是根据前文设置的  `Snapshot interval`  的值来生成的，因此我们的 `epochs` 值为 30 ，`Snapshot Interval` 的值为 1 ，因此会生成 30 个中见模型。默认操作的是最后一轮的模型 `Epoch#30`。

- `Download Model` 我们可以将训练好的模型下载到本地，以便在其他地方使用，下载的文件名字是按照`job id-epoch-选择的代数.tar.gz` 的格式，如：`20190618-115745-7c5f_epoch_30.0.tar.gz` ，解压可看到相关的训练文件。

![image](https://wx1.sinaimg.cn/large/69d4185bgy1g459hbaka9j20fj084aav.jpg)

这些文件可以用于添加预训练模型，也可以通过下面的方式更便捷的添加。

- ` Make Pretrained Model` 将当前的模型发布为预训练模型，操作完成就可以看到模型出现在了 `Pretrained Models` 列表中

![image](https://wx3.sinaimg.cn/large/69d4185bgy1g459lgjgn0j20xc0agdgm.jpg)

- `Publish to inference server` 将模型发布到推断服务器【X】

---

**单张图片测试**

可以输入图片的路径，也可以上传单张图片，这里选择上传一张含有手写 7  的图片，勾选下面的 `Show visualizations and statistics` 可以显示模型的可视化和统计信息

![image](https://wx4.sinaimg.cn/large/69d4185bgy1g459wtxsf6j20a307kwep.jpg)

点击 `Classify One` 对这张图片进行预测，模型显示的结果为 `Top-5`

 ![image](https://ws3.sinaimg.cn/large/69d4185bgy1g45a0lfainj20lp09hq3b.jpg)

左侧是实际的图片，右侧是输出的 `Top-5`的结果，可以看到预测是 8 的概率为 99.53%，第二可能的是 0 概率为 0.39%。

在统计信息中，由 data 和 scaled 可以看到图片中对分类起到关键作用的部分基本上覆盖了 8 字全部

![image](https://wx1.sinaimg.cn/large/69d4185bgy1g45a5oe809j20jp0eot9j.jpg)

再往下就是网络的各层统计信息，从卷基层到池化层和全连接层

![image](https://ws4.sinaimg.cn/large/69d4185bgy1g45abre6plj212p0npgsa.jpg)

![image](https://wx4.sinaimg.cn/large/69d4185bgy1g45adclqc2j21590m5422.jpg)



---

**多张图片测试**

`Upload Image List` 可以上传一个文本文件，里面含有图片的 urls，文件的内容如下：

```
/home/lq/dataset/exper/1.png
/home/lq/dataset/exper/7.png
/home/lq/dataset/exper/7-1.png
/home/lq/dataset/exper/8.png
/home/lq/dataset/exper/9.png
```

设置一个文件目录 `Image folder`，此处设置为 `/home/lq/dataset/exper` （也可以不设置）

![image](https://wx3.sinaimg.cn/large/69d4185bgy1g45b29iw71j20aa0dp755.jpg)

点击 `Classify Many` 会对全部的文件进行预测，生成每个文件的 `Top-5` 

![image](https://ws1.sinaimg.cn/large/69d4185bgy1g45b3x6iphj20x907ogmk.jpg)

对于大量的图片还可以生成混淆矩阵

![image](https://ws1.sinaimg.cn/large/69d4185bgy1g45b974udbj20tb0csdgk.jpg)



不使用这种方式，还是使用 `Top N Predictions pre Category` 进行归类，每个此处设置的每个类别展示两张图片

![image](https://ws4.sinaimg.cn/large/69d4185bgy1g45b6hdrblj20f20mi74r.jpg)

# 预训练模型

在 `Pretrained Models` 可以对预训练模型进行管理，我们既可以上传在其他地方下载的模型，也可以在DIGITS 中查找

![image](https://wx3.sinaimg.cn/large/69d4185bgy1g45bxrkir9j207r043q2w.jpg)

首先，看 `Retrieve from Model Store` ，点进去可以看到已有的在线模型，模型列表加载的比较缓慢需要等一等

![image](https://wx2.sinaimg.cn/large/69d4185bgy1g45bzmam1vj20x40b2myh.jpg)

如下载一个 `U-NET` 模型，点击旁边的箭头即可进行下载，下载需要耐心等待，下载的时间有可能比较长，下载完成时控制体会有提示

```log
2019-06-18 16:43:11 [20190618-164309-df15] [INFO ] Job complete.
2019-06-18 16:43:11 [20190618-164309-df15] [INFO ] Job complete.
```

可以看到下载的 `jod id` 为 `20190618-164309-df15` 在 DIGITS 的安装目录中就可以找到相关的下载文件

![image](https://wx3.sinaimg.cn/large/69d4185bgy1g45el1p8xqj20j706tjs5.jpg)

回到首页，在预训练模型里就可以看到此模型了

![image](https://wx1.sinaimg.cn/large/69d4185bgy1g45em0n79yj20x20b9gmi.jpg)



除了下载在线的，还可以上传自己训练或者下载的，有两种方式可以上传一种是填写相关的配置文件，第二种上传压缩包。比如，模型商店里面并没有 `VGG19` 的模型，我们可以从其他地方下载，然后上传上去。这里需要注意的是，从网上下载的 `VGG19` 模型通常只含有主文件，但是 DIGITS 还需要 `label.txt` 和 `original.prototxt` 文件，因此需要将这两个文件补全，否则预训练文件无法上传成功

![image](https://wx1.sinaimg.cn/large/69d4185bgy1g45f9sp7hrj20dv0dwaax.jpg)

`labels.txt` 里面不一定要含有内容 ，可以只是一个空文件，谁给你传完成就可以在列表中看到

![image](https://ws4.sinaimg.cn/large/69d4185bgy1g45fegzsdej20x90cowfp.jpg)



# 问题

1、日志没有配置

```
WARNING: log_file config option not found - no log file is being saved
```

默认的日志存储在 `$DIGITS_ROOT/digits/digits.log` ，产生上面问题的原因可能是由于日志文件只有root用户有权限，可以将此文件的权限开放或者删除原有重新创建一个。



2、上传 tensorflow 模型暂时有问题，可以查看下面的 issue

[Loading pretrained tensorflow model](https://github.com/NVIDIA/DIGITS/issues/1791)

