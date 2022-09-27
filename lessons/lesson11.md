The reason Linux kernel couldn't boot in last chapter is because the [root file system](https://tldp.org/LDP/sag/html/root-fs.html) was missing from the system. U-Boot didn't provide one.

Let's provide a root file system so that Linux can boot.

# Building Root File System

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
make -j$(nproc)
```
