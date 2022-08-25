# Installing Pre-requisites

Following command can be executed to install all software necessary for this tutorial:

``` bash
sudo apt install build-essential libncurses-dev rsync
```

[Buildroot](https://buildroot.org/) is a software package that can generate necessary tools for cross compiling code base for embedded Linux. This tutorial will use version `v2022.05.1`.

``` bash
# Download the file
wget https://buildroot.org/downloads/buildroot-2022.05.1.tar.gz

# Extract from compressed file
tar xf buildroot-2022.05.1.tar.gz
```
> `tar` parameters:
> - x = Extract files
> - f = Use archive file

``` bash
cd buildroot-2022.05.1

# To generate toolchain
make menuconfig
```

Select the following options:
1. Target options
    1. Target Architecture > RISCV
    1. Target Architecture Size > 64-bit
1. Toolchain
    1. C library > musl
    1. Kernel Headers > Linux 5.17.x kernel headers
    1. Binutils Version > binutils 2.38
    1. GCC compiler Version > gcc 11.x

Exit and save the configuration. Build the toolchain:

``` bash
make sdk
```
