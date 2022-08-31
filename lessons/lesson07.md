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

Let's download Linux kernel source code and build it from there. Run the following from the root working directory:
``` bash
git clone git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
```
