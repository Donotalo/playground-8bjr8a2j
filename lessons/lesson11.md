The reason Linux kernel couldn't boot in last chapter is because the [root file system](https://tldp.org/LDP/sag/html/root-fs.html) was missing from the system. U-Boot didn't provide one.

Let's provide a root file system so that Linux can boot.

# Build the Root File System

## Buildroot

Run the following script from the root working directory to create a root file system using [Buildroot](https://buildroot.org/):
``` bash
cd buildroot-2022.05.2
make menuconfig
```

From the configuration screen, set the following:
1. Filesystem images
    1. Select `ext2/3/4 root filesystem`
        1. Set `ext4` from `ext2/3/4 variant`
    1. Set `rootfs` as the `filesystem label`
    2. Set `16M` as `exact size`
    3. Select `tar the root filesystem` (this will compress the root file system)
        1. Set `lz4` as the `Compression method`

Exit and save the configutation when prompted. Build it:
``` bash
make
```

The output file is `./output/images/rootfs.tar.lz4`.

# Copy the Root File System to Disk

The root file system needs to be available to U-Boot. Let's copy it in the `disk.img` image. Run the following from the root working directory:
``` bash
sudo losetup --find --show --partscan disk.img
```
> - `partscan` = Scan the image `disk.img` for partition and attach each partition to a separate loop device

On the tutorial machine, the image is attached to the `/dev/loop0` loop device. The partition numbers can be confirmed by:
``` bash
ls -l /dev/loop0*
```

On the tutorial machine, the partitions are `/dev/loop0p1` and `/dev/loop0p2`. Now we've a way to copy files to `disk.img`. Run the following in the terminal to copy the root file system in partition 2:
``` bash
sudo mount /dev/loop0p2 /mnt/uboot
sudo cp buildroot-2022.05.2/output/images/rootfs.tar.lz4 /mnt/uboot
sudo umount /mnt/uboot

# Release /dev/loop0 to be used later
sudo losetup -d /dev/loop0
```

# Modify U-Boot

Linux kernel can be called with arguments. In the arguments, the location of root file system can be specified. Let's modify U-Boot to setup this arguments.

Run the follwing from the root working directory:
``` bash

```

1. From `Boot options`, set `Enable boot arguments`.
    1. Set the follwing in the `Boot arguments`: 
