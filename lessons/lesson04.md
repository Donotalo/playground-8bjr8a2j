# Pre-requisites

The `CROSS_COMPILE` environment variable needs to be defined to the prefix of the RISC-V gcc toolchain name that's added to the path. U-Boot build system will use this variable to correctly indentify the toolchain needed.

The script below will append the line at the end of `tech.io-env.sh` and make it available. Execute the following from the root working directory:

``` bash
echo "export CROSS_COMPILE=riscv64-linux-" >> tech.io-env.sh
source tech.io-env.sh
```

# Build U-Boot

## Download Source Code

This tutorial will use [U-Boot](https://www.denx.de/wiki/U-Boot) `v2022.07`. From the root working directory, run the following in the terminal:

``` bash
# Download U-Boot source
git clone https://source.denx.de/u-boot/u-boot.git

# Checkout required version
cd u-boot
git checkout v2022.07
```

## Board Configuration Files

U-Boot needs to be built for the hardware on which it will run. U-Boot needs to know what's available in the hardware side (device tree) and how to use them (drivers). Each hardware configuration is different. U-Boot source tree has configuration file specific to each of the hardware that U-Boot supports. These files are also referred as board configuration file. Support for additional hardware can be made by creating separate board configuration file.

## Find Architecture Specific Configuration Files

This part isn't mandatory if the board configuration file is known.

U-Boot stores board specific configuration files in the `configs` directory. Let's list available configuration files for RISC-V:
``` bash
ls configs | grep riscv --color=always
```
> - `--color=always` = Make the foreground color different for search terms

This generates the following output:
```
openpiton_riscv64_defconfig
openpiton_riscv64_spl_defconfig
qemu-riscv32_defconfig
qemu-riscv32_smode_defconfig
qemu-riscv32_spl_defconfig
qemu-riscv64_defconfig
qemu-riscv64_smode_defconfig
qemu-riscv64_spl_defconfig
```

A `QEMU` simulated board will be chosen, with `RISC-V` 64 bit architecture capable of running `Linux`. `qemu-riscv64_smode_defconfig` will serve the purpose of this tutorial. The S-Mode (supervisor mode) configuration is chosen so that it can load Linux kernel.

## Build

The command to build is:
``` bash
# To generate .config file out of board configuration file
make qemu-riscv64_smode_defconfig

# To build U-Boot
make -j$(nproc)
```

## Output
The U-Boot binary is located at `./u-boot.bin`.
