# Pre-requisites

The build system of [Linux kernel](https://kernel.org/) for RISC-V requires one more environment variable. Let's set it first. From the root working directory, run the following commands:
``` bash
gedit tech.io-env.sh &
```
Append the following line at the end of the file:
```
export ARCH=riscv
```
Save and close the file. Now run it:
``` bash
source tech.io-env.sh
```

# Build Linux Kernel

Let's download and build Linux kernel from source code. Linux `v5.19.6` will be used for this tutorial. Run the following from the root working directory:
``` bash
# Download the compressed file that contains Linux source code
wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.19.6.tar.xz

# Extract and enter into directory
tar xf linux-5.19.6.tar.xz
cd linux-5.19.6/
```

Linux kernel will be built for `RISC-V` architecture. The default configurations for several targets can be obtained by the command below:
``` bash
make help | grep defconfig --color=always
```

The default configutation, `defconfig`, is good enough so let's build it:
``` bash
# Generate .config file
make defconfig

make -j$(nproc)
```

## Did it Compile for RISC-V?

Open the `.config` file.
``` bash
gedit .config &
```
Observe that the RISC-V configuration option is enabled:
```
CONFIG_RISCV=y
```
This happens because of the environment variable `ARCH` is set to `riscv`. It is used when the `.config` file was generated. The details can be found by opening the `Makefile`:
``` bash
gedit ./Makefile &
```
and searching for `ARCH` and `CROSS_COMPILE`.

## Output Files

The output file is `arch/riscv/boot/Image` and a compressed copy of it is at `arch/riscv/boot/Image.gz`.
