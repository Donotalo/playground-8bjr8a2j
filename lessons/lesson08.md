# Procedure

To properly load Linux kernel in QEMU, additional storage is required that will hold the following:
1. U-Boot environment to load Linux kernel.
2. Linux kernel command line.
3. Filesystem where Linux will boot.

## Creating Storage

Run the following command in terminal from root working directory to create storage space:
``` bash
# Create a 128 MB disk image
dd if=/dev/zero of=disk.img bs=1M count=128
```

