# Installing Pre-requisites

On a fresh Debian machine, necessary software to build software packages can be installed by the following command:

```bash
sudo apt install build-essential
```

[Buildroot](https://buildroot.org/) is a software package that can generate necessary tools for cross compiling code base. This tutorial will use `v2022.05.1`.

```bash
# Download the file
wget https://buildroot.org/downloads/buildroot-2022.05.1.tar.gz

# Extract from compressed file
tar xf buildroot-2022.05.1.tar.gz
```
`tar` parameters:
- x = Extract files
- f = Use archive file

```bash
cd buildroot-2022.05.1

# To generate toolchain
make menuconfig
```
