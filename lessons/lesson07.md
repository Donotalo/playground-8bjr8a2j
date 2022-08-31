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
