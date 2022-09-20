# Need for SPL

There are many boards with very limited memory that's available for the CPU to work with after a reset. These boards don't have enough memory to load U-Boot, OpenSBI, Linux kernel all initially and start the kernel. They often have at least 2 levels of memory: one is immediately accessible to CPU after reset, another is a larger one that may require initialization. This is where U-Boot SPL comes handy.

U-Boot SPL is a stripped down bootloader. The size of U-Boot SPL is very small compared to the complete U-Boot. SPL stands for Secondary Program Loader. U-Boot SPL can be loaded into smaller memory. Its functionality is very limited. But it typically performs very important task: loading necessary bootloader and other firmware into larger memory.
