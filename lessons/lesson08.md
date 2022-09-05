# Procedure

To properly load Linux kernel in QEMU, additional storage is required that will hold the following:
1. U-Boot environment to load Linux kernel and specify Linux kernel command line.
2. Linux kernel binary.
3. Filesystem where Linux will boot.

## Creating Storage

Run the following command in terminal from root working directory to create storage space:
``` bash
# Create a 128 MB disk image
dd if=/dev/zero of=disk.img bs=1M count=128
```
> `dd` is a software that can be used to copy and convert files. [`/dev/zero`](https://unix.stackexchange.com/questions/63238/purpose-of-dev-zero) is a special file that acts like a stream or generator. Reading `/dev/zero` always gives `0x00`. Writing to it has no effect.
> - `if` = Input file (read from this file)
> - `of` = Output file (write to this file)
> - `bs` = Reads/writes up to this amount of bytes at a time
> - `count` = Copy only this many input block from `if`

Two partitions will be created on the disk image `disk.img`. The first partition will be bootable.

## Creating Partitions

[`parted`](https://linux.die.net/man/8/parted) will be used to create partitions on the image `disk.img`. Create a partition table in the image:
``` bash
sudo parted disk.img mklabel gpt
```
> - `mklabel` = Command to create partition table on `disk.img`
> - `gpt` = [GPT](https://en.wikipedia.org/wiki/GUID_Partition_Table) is chosen as the partition type

Now it has a partition table. Let's mount the image as [loop device](https://en.wikipedia.org/wiki/Loop_device) so that it can be used as [block device](https://en.wikipedia.org/wiki/Device_file#Block_devices). A block device is a type of device from which blocks of data can be read from/written to at a time. Mounting `disk.img` as block device will allow creating partitions in it.
``` bash
# Setup disk.img as first available loop device
sudo losetup --find --show disk.img
```
> - `find` = Finds the first unused loop device
> - `show` = Show the name of the loop device `disk.img` is attached to

Note the full path of the loop device. On the tutorial machine it was `/dev/loop0`. Operaing on `/dev/loop0` will operate on the `disk.img`. Let's continue partitioning:
``` bash
# Create 64MB primary partition
sudo parted --align minimal /dev/loop0 mkpart primary ext4 0 50%

# Create another partition
sudo parted --align minimal /dev/loop0 mkpart primary ext4 51% 100%

# Optional: inspect the partitions
sudo parted /dev/loop0 print
```
> - `align` = Set partition alignment for optimum performance
> - `mkpart` = `parted` command to create partition
> - `primary` = To make primary partition
> - `ext4` = Type of the filesystem
> - `0` = Beginning of the partition
> - `100%` = End of the partition

## Formatting the partitions
