> Fast Boot is a feature that speeds up system startup by skipping initialization steps

### Fast Boot in Windows (Fast Startup)
In Windows, Fast Startup is a hybrid of a cold shutdown and hibernation. Here's how it works:
- **During shutdown**:
    - Windows logs off all users, ending their sessions.
    - Instead of fully shutting down the system, the kernel session is **hibernated**—i.e., memory contents related to system state (kernel, drivers, services) are saved to disk in a file (`hiberfil.sys`).
- **During the next boot**:
    - The system loads the hibernated kernel session directly from disk back into memory.
    - Since the drivers and system services are already initialized, boot time is significantly reduced.

This avoids reinitializing the kernel and drivers, which are some of the slowest parts of the boot process.

**Important note**: Fast Startup only applies to shutdowns, not reboots. A reboot performs a full system initialization.

### Fast Boot in UEFI/BIOS Firmware
Modern UEFI firmware also includes a **Fast Boot** option at the firmware level. When enabled, the system skips or minimizes certain Power-On Self Test (POST) steps, including:
- Full memory (RAM) tests
- Detection and initialization of USB devices (e.g., keyboards, flash drives)
- Loading of legacy boot devices or PXE/network boot configurations

This results in a faster handoff from firmware to the operating system loader.

However, because of skipped checks, USB keyboards might not function early in the boot process, and it might be harder to access the BIOS/UEFI setup screen.
### Advantages
- Significantly faster startup times
- Less energy usage during startup
- Preserves driver and system state (in Windows)
### Disadvantages and Trade-offs
- Can interfere with dual-boot setups (e.g., Linux and Windows sharing a disk), because the disk is not truly unmounted
- Some firmware devices (USB peripherals, boot devices) might not initialize properly
- Can make it harder to enter BIOS/UEFI setup during boot
- May lead to inconsistent system states if drivers or hardware configurations change between boots