A storage is needed that can contain the Linux kernel which will be loaded to memory for booting. This section will prepare the storage and copy necessary files in it. As we'll be running everything virtually, the storage will also be virtual.

# Creating Storage

Run the following command in terminal from root working directory to create storage space:
``` bash
# Create a 128 MB disk image
dd if=/dev/zero of=disk.img bs=1M count=128
```
> [`dd`](https://man7.org/linux/man-pages/man1/dd.1.html) is a software that can be used to copy and convert files. [`/dev/zero`](https://unix.stackexchange.com/questions/63238/purpose-of-dev-zero) is a special file that acts like a stream or generator. Reading `/dev/zero` always gives `0x00`. Writing to it has no effect.
> - `if` = Input file (read from this file)
> - `of` = Output file (write to this file)
> - `bs` = Reads/writes up to this amount of bytes at a time
> - `count` = Copy only this many input block from `if`
> 
> The file `disk.img` will be a `bs`x`count` bytes file containing only 0x00.

# Creating Partitions

Two partitions will be created on the disk image `disk.img`. The first partition will be bootable.

[`parted`](https://man7.org/linux/man-pages/man8/parted.8.html) will be used to create partitions in the image `disk.img`. Create a partition table in the image:
``` bash
sudo parted disk.img mklabel gpt
```
> - `mklabel` = Command to create partition table on `disk.img`
> - `gpt` = [GPT](https://en.wikipedia.org/wiki/GUID_Partition_Table) is chosen as the partition type

Now it has a partition table. Let's mount the image as [loop device](https://en.wikipedia.org/wiki/Loop_device) so that it can be used as [block device](https://en.wikipedia.org/wiki/Device_file#Block_devices). A block device is a type of device from which blocks of data can be read from/written to at a time. Mounting `disk.img` as block device will allow creating partitions in it.
``` bash
# Attach disk.img with the first available loop device
sudo losetup --find --show disk.img
```
> - `find` = Finds the first unused loop device
> - `show` = Show the name of the loop device `disk.img` is attached to

Note the full path of the loop device. On the tutorial machine it was `/dev/loop0`. Operating on `/dev/loop0` will operate on the `disk.img`. Let's continue partitioning:
``` bash
# Create a couple of primary partitions
sudo parted --align minimal /dev/loop0 mkpart primary ext4 0 50%
sudo parted --align minimal /dev/loop0 mkpart primary ext4 50% 100%

# Optional: inspect the partitions
sudo parted /dev/loop0 print
```
> - `align` = Set partition alignment for optimum performance
> - `/dev/loop0` = Device on which the operation will take place
> - `mkpart` = `parted` command to create partition
> - `primary` = To make primary partition
> - `ext4` = Type of the filesystem
> - `0 50%` / `50% 100%` = Beginning / end of the partition in terms of total available storage

# Formatting the Partitions

The partitions can be observed by the following command:
``` bash
ls -l /dev/loop0*
```
On the tutorial machine, the partitions are `/dev/loop0p1` and `/dev/loop0p2`.

Format the partitions and create [`ext4`](https://en.wikipedia.org/wiki/Ext4) filesystem:
``` bash
sudo mkfs.ext4 /dev/loop0p1
sudo mkfs.ext4 /dev/loop0p2

# Mark first partition as bootable
sudo parted /dev/loop0 set 1 boot on
```

# Copy the Linux Kernel

The Linux kernel was built from source for `RISC-V`. Now we've some storage (`disk.img`) ready where the kernel can reside.
``` bash
# Mount the 1st partition
sudo mkdir /mnt/uboot
sudo mount /dev/loop0p1 /mnt/uboot

# Copy the Linux kernel
sudo cp linux/arch/riscv/boot/Image /mnt/uboot

# Unmount the partition to save the changes
sudo umount /mnt/uboot
```

We're done with the loop device `/dev/loop0`. Detach it so that it can be used later:

``` bash
sudo losetup -d /dev/loop0
```
> - `d` = Option to detach a loop device
> - `/dev/loop0` = Name of the loop device to detach
