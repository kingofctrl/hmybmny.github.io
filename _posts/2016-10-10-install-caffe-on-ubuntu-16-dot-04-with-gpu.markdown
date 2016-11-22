---
layout: post
title: Install Caffe on Ubuntu 16.04 with GPU
issueid: 4
categories: 
- Caffe
tags:
- Ubuntu 
- Caffe 
---

这段时间在学习 Caffe 的使用, Caffe 的安装过程并不复杂，但是对于新手来说可能会出现各种各样的问题，有时候折腾几个小时甚至一天都没有成功，所以写下安装过程，方便查阅。

**Install with CPU**

- 安装依赖库

```
sudo apt update
sudo apt upgrade
sudo apt install -y build-essential cmake git pkg-config
sudo apt install -y libprotobuf-dev libleveldb-dev libsnappy-dev \
                    libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt install -y --no-install-recommends libboost-all-dev
# 如果使用 OpenBlas 代替默认的 ATLAS的话，
# 需要将 libatlas-base-dev 改为 libopenblas-dev
sudo apt install -y libatlas-base-dev 
sudo apt install -y libgflags-dev libgoogle-glog-dev liblmdb-dev
sudo apt install -y python-pip python-dev python-numpy python-scipy
```

- 修改配置文件

```
cd ~/
git clone https://github.com/BVLC/caffe.git
cd caffe/
cp Makefile.config.example Makefile.config
vim Makefile.config
```

取消注释

> CPU_ONLY := 1 

> WITH_PYTHON_LAYER := 1 

如果使用 OpenBlas 代替默认的 ATLAS的话，则修改

> BLAS := open

并运行以下命令（使用 OpenBlas 的情况下） 

```
echo 'export OPENBLAS_NUM_THREADS=4' >> ~/.bashrc
```

修改

> INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial 

> LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial 

- 安装 make pycaffe 所需的库

这一步之前最好先修改 pip 源，由于下载的文件比较大，默认源速度很慢，修改源可以参考 [Linux 修改镜像源加快下载速度(pip-RubyGems-NPM-Docker)](http://hmybmny.com/2016/09/change-sources/)

```
cd python
sudo -H pip2 install -r requirements.txt
```

- 安装

```
cd ..
make all -j $(($(nproc) + 1))
make test
make runtest
make pycaffe
make distribute
# user 是你的用户名
echo 'export PYTHONPATH=/home/user/caffe/python:$PYTHONPATH' >> ~/.bashrc 

# 重启终端
python
>>>import caffe 
>>>               # 没报错就成功了
```

如果选择 CPU 安装的话，最好使用 OpenBlas 代替默认的 ATLAS，因为 OpenBlas 对 CPU 多线程支持很好，能加快一点速度算一点吧。

**Install with GPU (NVIDIA)**

- 进入 BIOS，将 Secure Boot 改为 Disabled
- 下载 cuda8 和 Patch1 [https://developer.nvidia.com/cuda-release-candidate-download ](https://developer.nvidia.com/cuda-release-candidate-download) ,下载完后双击 cuda-misc-headers-8-0_8.0.27.1-1_amd64.deb 安装 Patch1
- 下载 cuDNN v5.1 Library for Linux [https://developer.nvidia.com/rdp/cudnn-download ](https://developer.nvidia.com/rdp/cudnn-download) ,对于想使用 cuDNN 加速的同学，首先得确保你的 GPU 计算能力 (capability) 大于 3.0
- 安装依赖库，和 Install with CPU 第一步一样

```
sudo apt update
sudo apt upgrade
sudo apt install -y build-essential cmake git pkg-config
sudo apt install -y libprotobuf-dev libleveldb-dev libsnappy-dev \
                    libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt install -y --no-install-recommends libboost-all-dev
# 如果使用 OpenBlas 代替默认的 ATLAS的话，
# 需要将 libatlas-base-dev 改为 libopenblas-dev
sudo apt install -y libatlas-base-dev 
sudo apt install -y libgflags-dev libgoogle-glog-dev liblmdb-dev
sudo apt install -y python-pip python-dev python-numpy python-scipy
```

- 在 Software & Updates 中将 Additional Drivers 下的 NVIDIA Corporation 改为 Using NVIDIA binary driver...(proprietary, tested)
- 安装 CUDA

```
sudo dpkg -i cuda-repo-ubuntu1604-8-0-rc_8.0.27-1_amd64.deb
sudo apt update
sudo apt install cuda
sudo apt install cuda-drivers

cd /usr/local/cuda-8.0/samples/
sudo make all -j $(($(nproc) + 1))

# 查看 GPU 计算能力 (capability)
./1_Utilities/deviceQuery/deviceQuery
```

##### 下图是我查看 GPU 计算性能后的输出结果

![GPU 计算能力](/images/gpu_capability.png)

- 如果 GPU 计算能力 (capability) 大于 3.0，那么就可以使用 cuDNN 加速，否则跳过安装 cuDNN这一步
- 安装 cuDNN

```
# 到 cuDNN v5.1 Library for Linux 下载后存放的目录，打开终端
tar -xzvf cudnn-8.0-linux-x64-v5.1.tgz
sudo cp cuda/lib64/lib* /usr/local/cuda-8.0/lib64/
sudo cp cuda/include/cudnn.h /usr/local/cuda-8.0/include/
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
```

- 安装 make pycaffe 所需依赖, 这一步之前最好先修改 pip 源，由于下载的文件比较大，默认源速度很慢，修改源可以参考 [Linux 修改镜像源加快下载速度(pip-RubyGems-NPM-Docker)](https://hmybmny.com/2016/10/change-sources/)

```
cd python
sudo -H pip2 install -r requirements.txt
```

- 修改 Makefile.config

去掉注释

> USE_CUDNN := 1 

> WITH_PYTHON_LAYER := 1

修改

> CUDA_DIR := /usr/local/cuda-8.0

> INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial 

> LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial 

- 安装

```
cd ..
make all -j $(($(nproc) + 1))
make test
make runtest
make pycaffe
make distribute
# user 是你的用户名
echo 'export PYTHONPATH=/home/user/caffe/python:$PYTHONPATH' >> ~/.bashrc 

# 重启终端
python
>>>import caffe 
>>>               # 没报错就成功了
```

**GPU (AMD)**

- https://github.com/BVLC/caffe/tree/opencl
- https://github.com/amd/OpenCL-caffe

参考资料

1. [caffe](https://github.com/BVLC/caffe/wiki)
2. [caffe-gpu-installation](https://github.com/IraAI/caffe-gpu-installation/wiki)
