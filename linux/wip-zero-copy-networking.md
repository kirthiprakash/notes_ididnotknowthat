# \[WIP\] Zero Copy Networking

There needs to be some place where data is stored before writing into a network interface. To trasfer a static file, stored on disk, the file contents needs to be copied into memory. To put it in terms of OS jargon, the data is copied to the user space, only to be copied back to the kernel space, adding an unnecessary copy operation.

We can avoid the round trip to the userspace by using specfic system calls which remaps the pages and do not copy any data.

Linux system calls like `splice`, `sendfile` support zero-copy mechanism and improves I/O performance.

