OpenSBI acts as a layer between bootloader running in RISC-V M-mode and and operating system running in S-Mode. It helps to get access to hardware from the Operating System. This tutorial uses `v1.1` of OpenSBI.

Run the following from the root working directory:
``` bash
# Download the source code
git clone https://github.com/riscv-software-src/opensbi.git

cd opensbi
git checkout v1.1

# Build for generic platform with U-Boot as bootloader
make PLATFORM=generic FW_PAYLOAD_PATH=../u-boot/u-boot.bin
```

