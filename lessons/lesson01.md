# Confession

I'm a beginner in the field of U-Boot and QEMU. I felt like it'd be good to have all the resources that I used to run U-Boot and Linux in QEMU in one place. I've written this tutorial with that in mind. At the same time I tried to make it as beginner friendly as possible. However, there could be errors in the process/explanation so care should be taken when using this tutorial.

# Prologue

The purpose of this tutorial is to show how to run U-Boot and Linux in QEMU and to list necessary resources all in one place. Because the code will be running on emulated machine, there is no need for real hardware. The chosen architecture is `RISC-V`.

This tutorial is inspired by and heavily followed the contents of this amazing talk: [Embedded Linux from scratch in RISC-V](https://mirror.cyberbits.eu/fosdem/2021/D.embedded/linux_from_scratch_on_risc_v.webm).

## Useful Links

- [U-Boot](https://www.denx.de/wiki/U-Boot) is a universal bootloader.
- [Linux](https://www.linux.org/) is a free Operating System which can be run on many architectures.
- [QEMU](https://www.qemu.org/) is a machine emulator.
- [RISC-V](https://riscv.org/) is a free and open source instruction set hardware architecture.
  - RISC-V specifications: https://riscv.org/technical/specifications/
  - RISC-V github repository: https://github.com/riscv
  - GNU toolchain for RISC-V: https://github.com/riscv-collab/riscv-gnu-toolchain
    - These are needed to cross compile (build for RISC-V from non-RISC-V machine, e.g., x86) for RISC-V hardware

The readers are requested to install necessary software in their host machine to try the scripts of this tutorial. This tutorial is developed on `Debian 11.4`.

Let's start.
