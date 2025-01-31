# 101.1: System Architecture

Understanding your system’s architecture is crucial for Linux administration. This section covers how to identify hardware components, understand the differences in CPU architectures, and explore how Linux interacts with the underlying hardware.

---

## Table of Contents
1. [Introduction](#introduction)  
2. [CPU Architecture and Identification](#cpu-architecture-and-identification)  
   - [Checking CPU Details](#checking-cpu-details)  
   - [Common Architectures](#common-architectures)  
3. [BIOS and UEFI](#bios-and-uefi)  
4. [Hardware Discovery Tools](#hardware-discovery-tools)  
   - [Listing PCI Devices](#listing-pci-devices)  
   - [Listing USB Devices](#listing-usb-devices)  
   - [Listing Block Devices](#listing-block-devices)  
   - [Listing Loaded Kernel Modules](#listing-loaded-kernel-modules)  
5. [Kernel and System Boot Overview](#kernel-and-system-boot-overview)  
6. [Practice Commands](#practice-commands)  

---

## Introduction

In Linux, "System Architecture" doesn’t just refer to your CPU type (32-bit vs. 64-bit, x86 vs. ARM), but also to the broader framework of how the BIOS/UEFI, kernel, and other components interact with your hardware. Becoming comfortable with system architecture helps you:

- Choose the correct distribution packages (e.g., 32-bit vs. 64-bit).
- Troubleshoot hardware recognition issues.
- Optimize performance and manage resources effectively.

---

## CPU Architecture and Identification

### Checking CPU Details

Linux provides multiple commands to inspect your CPU architecture and capabilities. Here are some common ones:

- **`lscpu`**  
  Displays detailed information about the CPU architecture, number of cores, CPU op-modes (32-bit, 64-bit), byte ordering, and more.

  ```bash
  $ lscpu
  Architecture:        x86_64
  CPU op-mode(s):      32-bit, 64-bit
  Byte Order:          Little Endian
  ...
  ```

- **`uname`** 
  Shows basic system information, including kernel version and hardware architecture.

    ```bash
    $ uname -m
    x86_64

    $ uname -r
    5.15.0-67-generic
    ```

- **`cat /proc/cpuinfo`**
   Provides low-level details about each CPU core, caches, and flags.

   ```bash
   $ cat /proc/cpuinfo | grep "model name"
    model name  : Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
    ...
    ```

### Common Architectures
- x86 (32-bit): An older standard on many legacy systems.
- x86_64 (64-bit): Most common on modern desktops and servers.
- ARM: Popular in mobile devices and some servers (Raspberry Pi, for example).
- PowerPC, RISC-V, etc.: Less common, but still found in specialized environments.

---

## BIOS and UEFI

Your system may use one of the following firmware interfaces to initialize hardware and boot the OS:

- `BIOS` (Basic Input/Output System): The traditional firmware interface. Relies on the Master Boot Record (MBR) partitioning scheme.
- `UEFI` (Unified Extensible Firmware Interface): Modern replacement for BIOS, supports larger disks (using GPT) and offers advanced features like Secure Boot.

Identifying whether your system uses BIOS or UEFI can be done by checking your motherboard settings or looking for specific directories (e.g., /sys/firmware/efi presence often indicates UEFI).

---

## Hardware Discovery Tools

### Listing PCI Devices

- **`lspci`**
    Lists all PCI buses and devices. Helpful for identifying network cards, sound cards, and other PCI hardware.
    ```bash
    $ lspci
    00:00.0 Host bridge: Intel Corporation Device 3e20
    00:02.0 VGA compatible controller: Intel Corporation UHD Graphics 620
    00:14.0 USB controller: Intel Corporation Device 9ded
    ...
    ```

### Listing USB Devices

- **`lsusb`**
Shows USB buses and connected devices. Useful for troubleshooting USB peripherals (keyboard, mouse, storage, etc.).

    ```bash
    $ lsusb
    Bus 001 Device 002: ID 04f2:b5f7 Chicony Electronics Co., Ltd
    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    ...
    ```
### Listing Block Devices
- **`lsblk`**
Displays information about block devices (disks, partitions, LVM volumes). Provides a tree view of devices and mount points.

    ```bash
    $ lsblk
    NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sda      8:0    0 238.5G  0 disk
    ├─sda1   8:1    0   512M  0 part /boot/efi
    ├─sda2   8:2    0   237G  0 part /
    └─sda3   8:3    0   1.0G  0 part [SWAP]
    ...
    ```
### Listing Loaded Kernel Modules
- **`lsmod`**
Shows which kernel modules (drivers) are currently loaded. Use it to confirm if hardware drivers (e.g., network adapters) are active.

    ```bash
    $ lsmod
    Module                  Size  Used by
    vfat                   20480  1
    fat                    65536  1 vfat
    i2c_dev                20480  0
    ...
    ```
---

## Kernel and System Boot Overview

1. `POST (Power-On Self-Test)`: When your system powers on, BIOS or UEFI performs checks to ensure hardware is working.
2. `Bootloader (GRUB, etc.)`: The bootloader then loads the kernel and any required initial RAM filesystem (initramfs).
3. `Kernel Initialization`: The Linux kernel initializes hardware drivers, memory management, and schedules processes.
4. `init or systemd`: Finally, the system moves into user space where init or systemd starts services and the user environment.

Understanding this flow is key to troubleshooting boot issues and optimizing system startup.

---

### Practice Commands

**Try these commands to explore your system’s architecture:**

1- **View CPU and Architecture:**
```bash
lscpu
```

2- **Check Kernel Version and Architecture:**
```bash
uname -r
uname -m
```

3- **List PCI and USB Devices:**
```bash
lspci
lsusb
```

4- **Inspect Block Devices and Mount Points:**
```bash
lsblk
df -h
```

5- **Check Loaded Kernel Modules:**
```bash
lsmod
```

**Tip: Combine these commands with `grep` or `less` for more targeted searching or easier reading. For example:**
```bash
lsmod | grep usb
```
---

### Next Steps
- Familiarize yourself with the output of each command.
- Note how your system’s hardware is detected and managed by the kernel.
- Move on to the next section for deeper insights into Linux installation, package management, and more.
