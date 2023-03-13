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
U-Boot 2023.01 (Mar 13 2023 - 11:01:11 +0000)

CPU:   rv64imafdch_zicsr_zifencei_zihintpause_zba_zbb_zbc_zbs_sstc
Model: riscv-virtio,qemu
DRAM:  1 GiB
Core:  26 devices, 12 uclasses, devicetree: board
Flash: 32 MiB
Loading Environment from nowhere... OK
In:    serial@10000000
Out:   serial@10000000
Err:   serial@10000000
Net:   No ethernet found.
Working FDT set to bf7357d0
Hit any key to stop autoboot:  0 

Device 0: unknown device
scanning bus for devices...

Device 0: unknown device
No ethernet found.
No ethernet found.
=> 
```

QEMU can be exited by pressing `Ctrl+A` followed by `X`.

# A Little More on U-Boot

`=>` is the default U-Boot prompt. U-Boot is ready to take commands. Type `help` and press `Enter` to see available commands. Try some of the commands and see U-Boot's responses. U-Boot will see the hardware that's provided by QEMU. Some commands to try:

- `cpu detail` = Print detailed CPU information.
- `bdinfo` = Print board specific information.
- `fdt print /` = Print complete flattened device tree, this will list all hardware with all properties available in the system. The `/` indicates start from the root of the tree.
- `fstypes` = File system types that U-Boot's current build supports.

On the tutorial machine, the `bdinfo` command reveals the following details about the board that QEMU has supplied to U-Boot:
```
=> bdinfo
boot_params = 0x0000000000000000
DRAM bank   = 0x0000000000000000
-> start    = 0x0000000080000000
-> size     = 0x0000000040000000
flashstart  = 0x0000000020000000
flashsize   = 0x0000000002000000
flashoffset = 0x0000000000000000
baudrate    = 115200 bps
relocaddr   = 0x00000000bff59000
reloc off   = 0x000000003fd59000
Build       = 64-bit
current eth = unknown
ethaddr     = (not set)
IP addr     = <NULL>
fdt_blob    = 0x00000000bf7378d0
new_fdt     = 0x00000000bf7378d0
fdt_size    = 0x0000000000001520
lmb_dump_all:
 memory.cnt  = 0x1
 memory[0]	[0x80000000-0xbfffffff], 0x40000000 bytes flags: 0
 reserved.cnt  = 0x2
 reserved[0]	[0x80000000-0x8007ffff], 0x00080000 bytes flags: 0
 reserved[1]	[0xbf736480-0xbfffffff], 0x008c9b80 bytes flags: 0
devicetree  = board
```

It can be found that the RAM start address and size are `0x80000000` and `0x40000000` (1 GB) respectively. This information is important as various data are needed to be placed within this address range.
