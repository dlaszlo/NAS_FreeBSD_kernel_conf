# Custom FreeBSD kernel configuration for home server

This guide provides instructions on setting up and installing a custom kernel configuration specifically tailored for my FreeBSD home server, primarily functioning as a NAS (Network Attached Storage). The configuration has been optimized by removing unnecessary modules and adding custom modifications tailored for my machine. Note that this configuration is designed to work exclusively with my server and may not be suitable for other systems.

The custom configuration is based on the GENERIC FreeBSD kernel, with unnecessary modules marked and removed using ###.

## Custom modifications

- **ZFS**: A robust file system and logical volume manager designed for data integrity, scalability, and ease of administration.
- **File Systems**: Supports additional file systems like File Descriptor Filesystem (FDESCFS), NullFS (for mounting a null layer), and UnionFS (for combining multiple directories).
- **Device Support**: Adds support for various devices including AMD temperature sensors (amdtemp), AMD System Management Network (amdsmn), CPU control (cpuctl), and more.
- **Networking**: Adds support for network bridging (if_bridge) and various USB devices including Human Interface Devices (uhid, usbhid, hidbus). This is essential for managing jails and virtual machines (VMs).

## FreeBSD source code

To begin, source the FreeBSD source code by cloning the appropriate branch:
   ```sh
   git clone -b releng/14.1 https://git.freebsd.org/src.git /usr/src
   ```

For more details on building and installing a FreeBSD kernel, refer to [Chapter 9. Building and Installing a FreeBSD Kernel](https://docs.freebsd.org/en/books/developers-handbook/kernelbuild/).
For instructions on configuring the FreeBSD kernel, refer to [Chapter 10. Configuring the FreeBSD Kernel](https://docs.freebsd.org/en/books/handbook/kernelconfig/).

## Linking the configuration File

To link the NAS configuration file to the appropriate location in your FreeBSD system, follow these steps:

1. **Navigate to the FreeBSD configuration directory:**:
   ```sh
   cd /usr/src/sys/amd64/conf
   ```

2. **Create a symbolic link to the NAS configuration file:**:
   ```sh
   ln -s /path/to/NAS_FreeBSD_kernel_conf/sys/amd64/conf/NAS NAS
   ```

## Building and installing the custom kernel

1. **Navigate to the FreeBSD source directory:**
   ```sh
   cd /usr/src
   ```

2. **Clean the previous build environment:**
   ```sh
   make -j12 clean
   ```

3. **Build the kernel with the custom configuration using parallel jobs:**
   ```sh
   make -j12 buildkernel KERNCONF=NAS
   ```

4. **Install the newly built kernel:**
   ```sh
   make installkernel KERNCONF=NAS
   ```

5. **Reboot the system to load the new kernel:**
   ```sh
   reboot
   ```
---
This guide should help set up a FreeBSD system with a custom kernel configuration optimized for NAS home server needs, including sourcing the FreeBSD source code and linking the configuration file appropriately.
