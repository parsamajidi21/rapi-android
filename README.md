# This is a repo dedicated to porting Android on Raspberry pi 4

## U-Boot

### Prepare the Environment for Cross Compilation
Install the GCC toolchain for bare-metal targets on your host machine. It's recommended to use the version 9.2 or higher of the ARM GNU Toolchain to cross-compile software.

1- **Create your own workspace in intended path**
```
$ mkdir ~/workspace
$ cd ~/workspace
```
2- **Download and unpack the the 32-bit and/or 64-bit Arm cross-toolchain files:**
```
$ wget -O arm-gnu-toolchain-12.3.rel1-x86_64-aarch64-none-linux-gnu.tar.xz "https://developer.arm.com/-/media/Files/downloads/gnu/12.3.rel1/binrel/arm-gnu-toolchain-12.3.rel1-x86_64-aarch64-none-linux-gnu.tar.xz?rev=cf8baa0ef2e54e9286f0409cdda4f66c&hash=4E1BA6BFC2C09EA04DBD36C393C9DD3A"
$ tar xvf arm-gnu-toolchain-12.3.rel1-x86_64-aarch64-none-linux-gnu.tar.xz
```
3- **Set the toolchain path:**
```
$ ln -s arm-gnu-toolchain-12.3.rel1-x86_64-aarch64-none-linux-gnu gcc-linaro-aarch64
```
4- **Prepare the environment variables:**
```
$ export ARCH=arm64 > ~/workspace/setEnv.sh
$ export DTC_FLAGS="-@" >> ~/workspace/setEnv.sh
$ export PATH=~/workspace/gcc-linaro-aarch64/bin/:$PATH >> ~/workspace/setEnv.sh
$ export CROSS_COMPILE=aarch64-none-linux-gnu- >> ~/workspace/setEnv.sh
$ source ~/workspace/setEnv.sh
```

### Install Tools and Dependencies
1- **Install Bison, Flex, Swig and OpenSSL and other dependencies:**
```
$ sudo apt-get install bc build-essential git libncurses5-dev lzop perl libssl-dev bison flex swig libyaml-dev pkg-config python3-dev
```
2- **Install the Device Tree Compiler (DTC) Tool:**
```
$ git clone git://git.kernel.org/pub/scm/utils/dtc/dtc.git -b v1.6.1 ~/dtc
$ cd ~/dtc
$ make
$ export PATH=$HOME/dtc/:$PATH
```
3- **Install U-Boot Tools:**
```
$ sudo apt-get install u-boot-tools
```
### Fetch U-Boot Sources
```
git clone https://github.com/u-boot/u-boot.git
```
### Configure U-Boot
1- Find the right configuration file at [the U-Boot Version Information for Raspberrypi](https://github.com/u-boot/u-boot/blob/master/doc/board/broadcom/raspberrypi.rst) table:

2- Go to the u-boot directory and use make mrproper for a clean configuration. Then, execute make <board_defconfig> to configure U-boot from a defconfig:
```
$ cd ~/workdir/u-boot
$ make mrproper
$ make <board_defconfig> # recommended config: make rpi_arm64_defconfig
```
### Compile U-Boot
```
$ make -j$(nproc) 2>&1 | tee build.log
```



