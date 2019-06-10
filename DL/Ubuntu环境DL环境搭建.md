# Ubuntu DL环境搭建t

## 环境

系统：16.04 64-bit

CPU： i7-8750H CPU @ 2.20GHz × 12

GPU：GT 1050Ti



## Anaconda卸载

[Ubuntu16.04上删除Anaconda2](https://blog.csdn.net/hhy_csdn/article/details/82116486)





## 安装 NVIDIA DIGITS

通过源码安装 [DIGITS](https://github.com/NVIDIA/DIGITS)，目前版本为 `6.1.1` ，下载源码：

```bash
git clone https://github.com/NVIDIA/DIGITS.git
```







# 常见问题

## NVIDIA重启无效

Ubuntu 使用的是双系统，在每次重新系统时候 `nvidia-smi` 就会无效，需要重新安装显卡驱动，可以尝试如下的方法解决：

第一种说发是关闭Ubuntu的软件自动更新：

[【已解决】Ubuntu18.04下NVIDIA GPU驱动程序运行一段时间后出错“NVIDIA-SMI has failed”](https://blog.csdn.net/cskywit/article/details/85211884)



第二中方法是说重新安装 `dkms` ，通过 `dkms` 安装 NVIDIA的驱动（**此方法测试可用**）：

[无法连接NVIDIA驱动：NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver](https://blog.csdn.net/hangzuxi8764/article/details/86572093)





