A disk image is ready with Linux kernel. We need a way to make QEMU aware of the presence of this disk. If QEMU can correctly identify the disk, it will pass the information to U-Boot and U-Boot will be able to find the partitions in it.

> How? When QEMU is compiled, it creates a device tree binary (dtb) and passes it to U-Boot. A device tree is a description of all hardware of the host. In memory the device tree is organized as flattened device tree (FDT).
> 
> ```
> QEMU -- passes dtb --> OpenSBI -- passes dtb -->
>   U-Boot -- parses dtb > finds disk > partitions > filesystem > kernel file
> ```

Run the following commands from the root working directory to add additional parameters to QEMU so that QEMU recognizes the disk image:
``` bash
editor run-u-boot.sh
```
Append the following lines to the end of the file:
```
-blockdev driver=file,filename=./disk.img,node-name=disk \
-device virtio-blk-device,drive=disk
```

Save and close the file. Run the script:
``` bash
./run-u-boot.sh
```
Observe that U-Boot has detected the disk as seen in something like the following console output:
```
...
Hit any key to stop autoboot:  0

Device 0: QEMU VirtIO Block Device
            Type: Hard Disk
            Capacity: 128.0 MB = 0.1 GB (262144 x 512)
... is now current device
Scanning virtio 0:1...
** File not found ubootefi.var **
Failed to load EFI variables
** Unable to write file ubootefi.var **
Failed to persist EFI variables
BootOrder not defined
EFI boot manager: Cannot load any image
scanning bus for devices...
...
```

# What's Happening?

`disk.img` is a block device. In [QEMU a block device](https://qemu-project.gitlab.io/qemu/system/invocation.html#hxtool-1) should be represneted by two things: a [device back end](https://qemu-project.gitlab.io/qemu/system/device-emulation.html#device-back-end) and a [device front end](https://qemu-project.gitlab.io/qemu/system/device-emulation.html#device-front-end). The `blockdev` parameter specifies a device back end and the `device` parameter specifies a device front end that the guest sees. When the guest operates on the front end, e.g., reading a file, it actually reads some bytes from the back end.

## Setup

### Device Back End
`-blockdev driver=file,filename=./disk.img,node-name=disk`

- `driver=file` = Indicates that the back end is a file
- `filename` = Path to the disk image that will act as the back end
- `node-name` = An identifier so that this back end driver can be referred in other nodes

### Device Front End
`-device virtio-blk-device,drive=disk`

- `virtio-blk-device` = This is a block device for `virt` machine
> To get a list of devices QEMU supports, run `./qemu/build/qemu-system-riscv64 -device help`
> 
> To get further help on `virtio-blk-device`, run `./qemu/build/qemu-system-riscv64 -device virtio-blk-device,help`
- `drive` = Name of the back end that will be manipulated by the front end
