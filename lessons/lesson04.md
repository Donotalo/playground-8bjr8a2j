# U-Boot Pre-requisites

The `CROSS_COMPILE` variable needs to be added to environment variable. `CROSS_COMPILE` should be a prefix to the RISC-V toolchain name that's added to the path. U-Boot build system will use this variable to correctly indentify the toolchain needed.

Execute the following from the root working directory:

``` bash
# Open the script file containing environment variables
gedit tech.io-env.sh &
```

Add the following line to the end of the file:
```
export CROSS_COMPILE=riscv64-linux-
```
Save and close the file. Execute it:
``` bash
source tech.io-env.sh
```

# Build U-Boot

## Download Source Code

This tutorial will use U-Boot `v2022.07`. Run the following in the terminal:

``` bash
# Download U-Boot source
git clone https://source.denx.de/u-boot/u-boot.git

# Checkout required version
cd u-boot
git checkout v2022.07
```

## Find Architecture Specific Configuration Files

U-Boot stores board specific configuration files in the `configs` directory. Let's list available configuration files for RISC-V:
``` bash
ls configs | grep riscv --color=always
```
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

