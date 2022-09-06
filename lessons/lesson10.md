U-Boot needs to be configured so that it can load the Linux kernel from the `disk.img`. The kernel is the file named `Image` in the first partition of the disk.

Run the following from the root directory:
``` bash
cd u-boot
make menuconfig
```
U-Boot configuration options will be loaded to the terminal.
