# Installing Pre-requisites

Following command can be executed to install all software necessary for this tutorial:

``` bash
sudo apt update && sudo apt install build-essential libncurses-dev rsync git ninja-build libglib2.0-dev libpixman-1-dev bison flex libssl-dev wget
```

Prepare a working directory:
``` bash
mkdir tech.io && cd tech.io
```
This will be considered as the root working directory for this tutorial.

# Installing Toolchain

[Buildroot](https://buildroot.org/) is a software package that can generate necessary tools for cross compiling code base for embedded Linux. This tutorial will use version `v2023.08`.

``` bash
# Download the file
wget https://buildroot.org/downloads/buildroot-2023.08.tar.gz

# Extract from compressed file
tar -xf buildroot-2023.08.tar.gz
```
> `tar` parameters:
> - x = Extract files
> - f = Use archive file

``` bash
cd buildroot-2023.08

# To generate toolchain configuration file
make menuconfig
```

Select the following options:
1. Target options
    1. Target Architecture > RISCV
    1. Target Architecture Size > 64-bit
1. Toolchain
    1. C library > musl
    1. Kernel Headers > Linux 6.4.x kernel headers
    1. Binutils Version > binutils 2.41
    1. GCC compiler Version > gcc 13.x

Exit and save the configuration. Build the toolchain:
``` bash
make sdk -j$(nproc)
```
> - `-j$(nproc)` = `$(nproc)` will expand to number of available processing units, `-j` flag will paralellize build utilizing the output indicated by `$(nproc)`


The output is the file `output/images/riscv64-buildroot-linux-musl_sdk-buildroot.tar.gz`. Extract it:

``` bash
# Create a toolchain directory for toolchain extraction
cd ..
mkdir toolchain && cd toolchain

tar -xf ../buildroot-2023.08/output/images/riscv64-buildroot-linux-musl_sdk-buildroot.tar.gz
```

All the important binaries are now in `riscv64-buildroot-linux-musl_sdk-buildroot/bin` directory. The RISC-V compier is `riscv64-linux-gcc`. Fix the hardcoded paths by running the script:

``` bash
riscv64-buildroot-linux-musl_sdk-buildroot/relocate-sdk.sh
```

Place the binaries in system path:

``` bash
# Go to the working directory
cd ..

# Create a script to update system path
gedit tech.io-env.sh &
```

Write the following in the `tech.io-env.sh` file:
```
export PATH=~/tech.io/toolchain/riscv64-buildroot-linux-musl_sdk-buildroot/bin:$PATH
```

Update environment variable by running the script using [source](https://superuser.com/questions/46139/what-does-source-do) & test:

``` bash
source tech.io-env.sh
riscv64-linux-gcc --version
```

It presented the version as follows:
```
riscv64-linux-gcc.br_real (Buildroot 2023.02) 12.2.0
Copyright (C) 2022 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

This confirms that the necessary toolchain has been installed and added to the system path.
