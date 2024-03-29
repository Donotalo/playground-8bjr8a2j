# Confession

I'm a beginner in the field of U-Boot and QEMU. I felt like it'd be good to have all the resources that I used to run U-Boot and Linux in QEMU in one place. I've written this tutorial with that in mind. At the same time I tried to make it as beginner friendly as possible.

# Prologue

The purpose of this tutorial is to show how to run U-Boot and Linux in QEMU and to list necessary resources all in one place. Because the code will be running on emulated machine, there is no need for real hardware. The chosen architecture is `RISC-V`.

This tutorial is inspired by and heavily followed the contents of this amazing talk: [Embedded Linux from scratch in RISC-V](https://www.youtube.com/watch?v=cIkTh3Xp3dA).

A fairly simple way is illustrated in this tutorial to run U-Boot and Linux kernel in QEMU. Advanced users may try different options when configuring/building various stuff throughout the tutorial.

# Useful Links

- [U-Boot](https://www.denx.de/wiki/U-Boot) is a universal bootloader.
- [Linux](https://www.kernel.org/) is a free Operating System which can be run on many architectures.
- [QEMU](https://www.qemu.org/) is a machine emulator.
- [RISC-V](https://riscv.org/) is a free and open source instruction set hardware architecture.
  - RISC-V specifications: https://riscv.org/technical/specifications/
  - RISC-V github repository: https://github.com/riscv
  - GNU toolchain for RISC-V: https://github.com/riscv-collab/riscv-gnu-toolchain
    - These are needed to cross compile (build for RISC-V from non-RISC-V machine, e.g., x86) for RISC-V hardware. However, this tutorial uses [Buildroot](https://buildroot.org/) to generate RISC-V build toolchain.
- [OpenSBI](https://github.com/riscv-software-src/opensbi) is the open source implementation of supervisor binary interface recommended for RISC-V. It interfaces between Operating System and machine hardware running bootloader in M-mode (machine mode).

The readers are requested to install necessary software in their host machine to try the scripts of this tutorial. This tutorial is developed on [Debian](https://www.debian.org/) 11.6.

Following tutorials and references are handy, but not required to understand this playground.
- Video: [Embedded Linux Booting Process](https://www.youtube.com/watch?v=DV5S_ZSdK0s)
- Video: [Device Tree for Dummies!](https://www.youtube.com/watch?v=m_NyYEBxfn8)
- Video: [Device Tree: hardware description for everybody!](https://www.youtube.com/watch?v=Nz6aBffv-Ek)
- [Device Tree Specifications](https://www.devicetree.org/specifications/)

Let's start.
