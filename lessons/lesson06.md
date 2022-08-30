The OpenSBI firmware that loads U-Boot in QEMU - all binaries are ready now.

Let's create a script file for executing QEMU. From the root working directory, run the following:
``` bash
cd qemu/
gedit run-u-boot.sh &
```

Write the following in the `run-u-boot.sh`:
```
#! /usr/bin/bash

./build/qemu-system-riscv64 -smp 2 \
-m 1G \
-nographic \
-machine virt \
-bios ../opensbi/build/platform/generic/firmware/fw_payload.elf
```
Save & close the file. Meaning of QEMU parameters are as follows:

> - `smp` = Number of processing units
> - `m` = Total memory
> - `nographic` = Graphics less environment
> - `machine` = QEMU virt machine to be used
> - `bios` = Path to the bios file

Make the script file executable:
``` bash
chmod +x run-u-boot.sh
```

The system can now be run by the following command:
``` bash
./run-u-boot.sh
```

QEMU can be exited by pressing `Ctrl+A` + `X`.
