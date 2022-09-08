Let's build [QEMU](https://www.qemu.org/) from source code so that it can simulate a `RISC-V` host.

For this tutorial, QEMU `v7.1.0` will be used. Let's checkout the source code and build QEMU for RISC-V. Run the following in the root working directory:
``` bash
# Download source code
git clone https://gitlab.com/qemu-project/qemu.git

# Checkout required version
cd qemu
git checkout v7.1.0

# Prepare QEMU for build
git submodule init
git submodule update --recursive

# Build QEMU for RISC-V
./configure --target-list=riscv64-softmmu
make -j$(nproc)
```
> - `--target-list=riscv64-softmmu` = Configure QEMU to run 64 bit RISC-V architecture code only, without this parameter QEMU will be built for all available targets

The executable is `./build/qemu-system-riscv64`.
