# Installing Pre-requisites

On a fresh Debian machine, necessary software to build software can be installed by the following command:
``` bash
sudo apt install build-essential
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

``` sh
cd buildroot-2022.05.1

# Install pre-requisites to build buildroot
sudo apt install libncurses-dev

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

Exit and save the configuration.
