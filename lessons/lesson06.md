The OpenSBI firmware that loads U-Boot in QEMU - all executables are ready now.

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
> Note that this is a [multiline script](https://superuser.com/a/1634621). Each line should end with a backslash (`\`) for continuation.

Save & close the file. Meaning of the [QEMU parameters](https://www.qemu.org/docs/master/system/invocation.html) are as follows:

> - `smp` = Number of processing units
> - `m` = Total memory
> - `nographic` = Graphics less environment
> - `machine` = Specify the machine QEMU will emulate, here [QEMU virt machine for RISC-V](https://www.qemu.org/docs/master/system/riscv/virt.html) will be used
> - `bios` = Path to the bios file

Make the script file executable:
``` bash
chmod +x run-u-boot.sh
```

The system can now be run by the following command:
``` bash
./run-u-boot.sh
```

By observing the console output, it can be seen that OpenSBI and U-Boot are loaded and being executed. Here's part of the output on the test machine that shows that U-Boot is running:
```
U-Boot 2022.07 (Sep 23 2022 - 09:51:06 +0100)

CPU:   rv64imafdch_zicsr_zifencei_zba_zbb_zbc_zbs
Model: riscv-virtio,qemu
DRAM:  1 GiB
Core:  26 devices, 13 uclasses, devicetree: board
Flash: 32 MiB
Loading Environment from nowhere... OK
In:    uart@10000000
Out:   uart@10000000
Err:   uart@10000000
Net:   No ethernet found.
Hit any key to stop autoboot:  0 

Device 0: unknown device
scanning bus for devices...

Device 0: unknown device
No ethernet found.
No ethernet found.
=>
```

`=>` is the default U-Boot prompt. U-Boot is ready to take commands. Type `help` and press `Enter` to see available commands. Try some of the commands and see U-Boot's responses. U-Boot will see the hardware that's provided by QEMU. Some commands to try:

- `cpu detail` = Print all available CPUs in details
- `fdt print /` = Print complete flattened device tree, this will list all hardware with all properties available in the system
- `fstypes` = File system types that U-Boot's current build supports

QEMU can be exited by pressing `Ctrl+A` followed by `X`.
