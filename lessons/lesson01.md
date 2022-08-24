# Prologue

This tutorial is designed to be beginner friendly, for those who want to start with embedded devices running Linux and U-Boot. The purpose of this tutorial is to run U-Boot and Linux in QEMU and to list necessary resources all in one place. Because the code will be running on emulated machine, there is no need for real hardware. The chosen architecture is RISC-V.

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

Let's get started.
