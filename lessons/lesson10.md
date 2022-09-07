U-Boot needs to be configured so that it can load the Linux kernel from the `disk.img`. The kernel is the file named `Image` in the first partition of the disk.

Run the following from the root directory:
``` bash
cd u-boot
make menuconfig
```
U-Boot configuration options will be loaded to the terminal. Navigate to Boot options -> bootcmd value and write the following as the bootcmd value:
```
ext4load virtio 0:1 84000000 Image; booti 0x84000000 - ${fdtcontroladdr}
```
Exit the configuration utility and save the configuration.
