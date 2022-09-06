A disk image is ready with Linux kernel. We need a way to make QEMU aware of the presence of this disk. If QEMU can correctly identify the disk, it will pass the information to U-Boot and U-Boot will be able to find the partitions in it.

Run the following commands from the root working directory to add additional parameters to QEMU so that QEMU recognizes the disk image:
``` bash
cd qemu/
gedit ./run-u-boot.sh &
```
Append the following lines to the end of the file:
```
-blockdev driver=file,filename=../disk.img,node-name=disk \
-device virtio-blk-device,drive=disk
```

Save and close the file. Run the script:
``` bash
./run-u-boot.sh
```
Observe that U-Boot has detected the disk as seen in the console output:
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
BootOrder not defined
EFI boot manager: Cannot load any image
scanning bus for devices...
...
```
