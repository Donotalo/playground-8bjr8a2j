# Pre-requisites

The build system of Linux kernel for RISC-V requires one more environment variable. Let's set it first. From the root working directory, run the following commands:
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

Let's download Linux kernel source code and build it from there. Linux `v5.19.6` will be used for this tutorial. Run the following from the root working directory:
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
make defconfig
make -j$(nproc)
```
