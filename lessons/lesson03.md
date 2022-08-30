QEMU needs to be prepared so that it can run RISC-V executable file. As a reference how to do stuff, QEMU will be built from the source. code

For this tutorial, QEMU `v7.0.0` is used. Let's checkout the source code and build QEMU for RISC-V.

``` bash
# Download source code
git clone https://gitlab.com/qemu-project/qemu.git

# Checkout v7.0.0
cd qemu
git checkout v7.0.0

# Build QEMU for RISC-V
git submodule init
git submodule update --recursive

./configure --target-list=riscv64-softmmu
make -j$(nproc)
```
> - `--target-list=riscv64-softmmu` = Configure QEMU to run 64 bit RISC-V architecture code only, without this parameter QEMU will be built for all available targets
> - `-j$(nproc)` = `$(nproc)` will expand to number of available processing units, `-j` flag will paralellize build utilizing the output indicated by `$(nproc)`

The executable is at `./build/qemu-system-riscv64`.
