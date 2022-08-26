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
```
