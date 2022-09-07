U-Boot needs to be configured so that it can load the Linux kernel from the `disk.img`. The kernel is the file named `Image` in the first partition of the disk.

Run the following from the root directory to start U-Boot configuration utility:
``` bash
cd u-boot
make menuconfig
```
U-Boot configuration options will be loaded to the terminal. Navigate to `Boot options` -> `bootcmd value` and write the following as the `bootcmd value`:
```
ext4load virtio 0:1 84000000 Image; booti 0x84000000 - ${fdtcontroladdr}
```

Save the configuration and exit the configuration utility. Now build U-Boot, build OpenSBI embedding U-Boot binary and run U-Boot in QEMU:
``` bash
make -j$(nproc)
cd ../opensbi/
make PLATFORM=generic FW_PAYLOAD_PATH=../u-boot/u-boot.bin -j$(nproc)
cd ../qemu/
./run-u-boot.sh
```

The autoboot feature of U-Boot will proceed and start the kernel! Well, the kernel will crash and not usable, but at least U-Boot is able to load and run it.

# What's Happening?

As autoboot is set, it executed the commands set in the *environment variable* `bootcmd`. Commands can be separated by semicolon. We've set 2 commands: `ext4load` and `booti`.
```
ext4load virtio 0:1 84000000 Image
```
This command loads data from ext4 filesystem.
> - `virtio` = Name of the interface to read from
> - `0:1` = Read from 1st available device's 1st partition (the device number starts from `0`, the partition number starts from `1`)
> - `84000000` = A hexadecimal number, this is the address in RAM where the file will be loaded
> - `Image` = Name of the file to be loaded into RAM

```
booti 0x84000000 - ${fdtcontroladdr}
```
This command executes Linux Image format from memory.
> - `0x84000000` = Location where Linux Image is stored
> - `-` = Unused parameter, used to be address of initrd (initial RAM disk)
> - `${fdtcontroladdr}` = A variable holding the address of the FDT in memory
