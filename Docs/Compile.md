# 编译

## 普通编译

首先请确保系统已安装 ```libpcap``` 库以及 ```CMake``` 。

```bash
git clone https://github.com/LGiki/fzuSupplicant.git
cd fzuSupplicant
mkdir build
cd build
cmake ../
make
```

之后可以在 ```build/bin``` 目录下找到 fzuSupplicant 的可执行文件。

## 交叉编译

交叉编译需要先编译 libpcap ，之后再编译 fzuSupplicant。下面以交叉编译到 ar71xx 路由器为例：(以下代码中的一些参数需要根据你的实际情况做相应的修改，仅供参考)

### 获取目标设备的交叉编译工具链

从 [https://downloads.openwrt.org/](https://downloads.openwrt.org/) 上面下载目标设备的交叉编译工具链。例如 ar71xx 芯片的工具链下载地址为：[https://downloads.openwrt.org/releases/18.06.0/targets/ar71xx/generic/openwrt-sdk-18.06.0-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64.tar.xz](https://downloads.openwrt.org/releases/18.06.0/targets/ar71xx/generic/openwrt-sdk-18.06.0-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64.tar.xz)

(若下载缓慢，可以到[清华大学镜像源](https://mirrors.tuna.tsinghua.edu.cn/lede/)以及[中国科学技术大学镜像源](https://mirrors.ustc.edu.cn/lede/)下载相应工具链)

```bash
wget https://downloads.openwrt.org/releases/18.06.0/targets/ar71xx/generic/openwrt-sdk-18.06.0-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64.tar.xz
tar xvJf openwrt-sdk-18.06.0-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64.tar.xz
```

### 配置环境变量

环境变量中的具体路径以及参数要根据你的实际情况做相应的修改，以下代码仅供参考：

```bash
export PATH=$PATH:/home/xxx/openwrt-sdk-18.06.0-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64/staging_dir/toolchain-mips_24kc_gcc-7.3.0_musl/bin
export CC=mips-openwrt-linux-gcc
export CPP=mips-openwrt-linux-cpp
export GCC=mips-openwrt-linux-gcc
export CXX=mips-openwrt-linux-g++
export RANLIB=mips-openwrt-linux-ranlib
export LC_ALL=C
export LDFLAGS="-static"
export CFLAGS="-Os -s"
export STAGING_DIR=/home/xxx/openwrt-sdk-18.06.0-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64/staging_dir/toolchain-mips_24kc_gcc-7.3.0_musl
```

### 交叉编译 libpcap

```bash
wget http://www.tcpdump.org/release/libpcap-1.9.0.tar.gz
tar zxvf libpcap-1.9.0.tar.gz
cd libpcap-1.9.0
./configure --host=mips-linux --with-pcap=linux
make
```

如果交叉编译 libpcap 的过程中遇到错误，不用担心，这里我们只需要用到 ```libpcap.a``` ，编译后能得到该文件即可。之后将该文件以及 libpcap 的相关头文件复制到工具链的目录中：

```bash
cp libpcap.a /home/xxx/openwrt-sdk-18.06.0-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64/staging_dir/toolchain-mips_24kc_gcc-7.3.0_musl/lib
cp pcap.h /home/xxx/openwrt-sdk-18.06.0-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64/staging_dir/toolchain-mips_24kc_gcc-7.3.0_musl/include
cp -r pcap /home/xxx/openwrt-sdk-18.06.0-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64/staging_dir/toolchain-mips_24kc_gcc-7.3.0_musl/include
```

### 交叉编译 fzuSupplicant

```bash
git clone https://github.com/LGiki/fzuSupplicant.git
cd fzuSupplicant
mkdir build
cd build
cmake ../ -DCMAKE_FIND_ROOT_PATH=/home/xxx/openwrt-sdk-18.06.0-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64/staging_dir/toolchain-mips_24kc_gcc-7.3.0_musl -DCMAKE_FIND_ROOT_PATH_MODE_LIBRARY=ONLY -DCMAKE_C_COMPILER=/home/xxx/openwrt-sdk-18.06.0-ar71xx-generic_gcc-7.3.0_musl.Linux-x86_64/staging_dir/toolchain-mips_24kc_gcc-7.3.0_musl/bin/mips-openwrt-linux-gcc
make
```

之后可以在 ```build/bin``` 目录下找到 fzuSupplicant 的可执行文件。