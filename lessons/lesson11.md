# Need for SPL

There are many boards with very limited memory that's available for the CPU to work with after a reset. These boards don't have enough memory to load U-Boot, OpenSBI, Linux kernel all initially and start the kernel. They often have at least 2 levels of memory: one is immediately accessible to CPU after reset, another is a larger one that may require initialization.
