### 1. Power Supply Activation

- When you press the power button, it creates a closed circuit that signals the power supply unit (PSU) to start up. Most modern computers have an _ATX_ power supply, which remains in a low-power standby mode when the computer is off. This "soft-off" state allows the system to be powered on with the press of a button or other remote signals (such as Wake-on-LAN).
- The PSU then converts AC (alternating current) from the wall outlet into various DC (direct current) voltages (usually 12V, 5V, and 3.3V) needed by different computer components.
- Once it verifies that the required voltages are stable, the PSU sends a "Power Good" signal to the motherboard, and the **clock generator** circuits start, initializing a stable system clock.
### 2. CPU Initialization
- Once power and a stable clock signal are available, CPU starts executing code from a predefined **reset vector** (fixed address hard-coded into the CPU), typically located in the ROM where the BIOS/UEFI firmware is stored.
- This address contains the very first instruction for the CPU to execute and points to firmware on motherboard
- When the CPU powers on and receives the **Power Good** signal along with a clock, it jumps to reset vector, initiating the POST process in the BIOS/UEFI firmware.
### 3. Power-On Self-Test (POST)

- The first instructions at the reset vector are typically firmware routines that initialize the CPU registers and set up basic configurations.
- The firmware then performs the **Power-On Self Test (POST)**, initializing and testing critical hardware components to ensure they are functional.
- POST is stored in the BIOS (Basic Input/ Output System) or UEFI (Unified Extensible Firmware Interface). See [[BIOS vs UEFI]]
- **POST Tasks:**
    - **Memory (RAM) Testing and Initialization**:
	    - POST’s first hardware test is often a **basic RAM check**. The BIOS/UEFI tests a small amount of RAM (to verify that it can reliably store values and be used for further testing).
	    - After initial verification, the BIOS/UEFI will address and count the installed RAM modules.
	    - The memory controller is configured to ensure that data can be stored and retrieved, testing each module by writing and reading values across addresses.
	- **Basic Peripheral Checks**:
	    - The POST performs basic checks on essential hardware, such as the keyboard controller, graphics card, storage drives, and other peripherals connected to the motherboard.
	    - For instance, the graphics card BIOS is initialized, setting up a basic video mode to display error messages if needed.
	    - If an issue is detected (e.g., no keyboard or faulty RAM), POST may emit a series of beeps or display an error code. Each motherboard vendor defines unique beep codes, and these signals can indicate specific hardware failures.
- Any critical hardware issues during POST will prevent the computer from booting further. If POST is successful, the computer beeps once (on many systems) and hands over control to the BIOS/UEFI firmware.

### 4. BIOS/UEFI Initialization
- **Processor and Memory Configuration**:
    - The UEFI firmware initializes the **processor configuration** further, including core configuration, voltage settings, and frequency scaling based on the system’s power management policies.
    - It also performs more advanced memory initialization, setting up memory timings, speeds, and channel configurations to maximize memory performance.
    - **Memory-mapped I/O** (MMIO) regions are configured, allowing the CPU to communicate with peripheral devices through designated memory addresses.
- **Peripheral and I/O Device Initialization**:
    - UEFI initializes connected devices, including USB, SATA, PCIe, and network adapters.
    - **PCIe device enumeration**: UEFI performs PCI Express enumeration, assigning addresses to all devices on the PCIe bus. Each device is probed and assigned resources, such as memory addresses and interrupts.
    - **Graphics Initialization**: UEFI initializes the GPU or integrated graphics by communicating with the graphics card BIOS/UEFI, setting up a low-resolution display mode suitable for the boot screen and initial UEFI interface.
- **Secure Boot and Firmware Verification**:
    - **Secure Boot** is a security feature in UEFI that ensures only signed and trusted bootloaders can be loaded, protecting against malware at the boot stage.
    - If Secure Boot is enabled, UEFI checks the digital signature of the bootloader against trusted certificates stored in firmware. If the signature doesn’t match, the system won’t boot.
- **Boot Manager Initialization**:
	- The UEFI Boot Manager searches for bootable devices by looking for an **EFI System Partition (ESP)** on attached storage devices. The ESP contains bootloaders, drivers, and UEFI applications.
	- UEFI reads from a **Boot Order list**, which specifies the devices and partitions to try in order. Once it finds a valid bootloader (such as GRUB for Linux), it hands control over to that bootloader.

### 5. Bootloader Execution

- #### Phase 1 bootloader:
	- The bootloader is a small program stored in the first 512 bytes sector (**Master Boot Record** (MBR)) of the bootable storage device, responsible for loading the operating system into memory.
	- For UEFI systems, the firmware directly accesses the **EFI System Partition (ESP)**, a dedicated partition formatted as FAT32, where it locates the bootloader file (e.g., `grubx64.efi` for GRUB).
	- Common bootloaders include:
	    - **Windows Boot Manager** (for Windows)
	    - **GRUB** (Grand Unified Bootloader, often used in Linux)
	    - **LILO** (older Linux Loader)
- #### Phase 2: Second-Stage Bootloader
	- In the second stage, the bootloader (like GRUB) provides a user interface or menu that allows you to select the operating system, kernel version, or boot options.
	- The bootloader reads its configuration file (e.g., `grub.cfg` for GRUB) to display menu entries and kernel parameters.
	- If you select an OS or the default option is chosen automatically, the bootloader proceeds to load the selected **Linux kernel** and **initial RAM disk (initrd/initramfs)** into memory.
- **Bootloader Tasks:**
    - The bootloader loads the operating system kernel, which is the core part of the OS responsible for managing resources and system calls.
    - It may display a boot menu if multiple operating systems are installed, allowing the user to choose one.
    - If any special boot parameters are configured (e.g., Safe Mode or debugging), the bootloader will pass these instructions to the kernel.

### 6. Operating System Kernel Initialization

- With the kernel loaded into RAM, it begins setting up the system's fundamental components and processes.
- **Hardware Abstraction Layer (HAL)**: The kernel initializes the HAL, which serves as a bridge between the operating system and the hardware, allowing software to interface with hardware components without knowing specific details about them.
- **Memory Management**: The kernel sets up virtual memory, mapping physical memory addresses to virtual addresses for each process. This enables efficient memory use and ensures each program runs in a protected memory space.
- **Device Drivers**: The kernel loads essential drivers needed for communication with hardware devices (e.g., graphics card, storage drives, network card).
- **Process Manager**: The kernel starts the process manager, which manages process creation, scheduling, and termination. This is what allows multiple programs to run simultaneously in a multi-tasking environment.
- **System Services and Daemons**: Various system services (on Windows) or daemons (on Linux/macOS) start up. These background processes provide core functions, like managing networking, user sessions, and file systems.
- **Kernel Mode to User Mode Transition**: The kernel then transitions control from kernel mode (privileged mode) to user mode (restricted mode) as it loads the initial system processes that make up the user interface.

### 7. User Interface and Login Screen

- At this point, the OS loads the graphical user interface (GUI), showing the login screen (if required).
- The system loads a display manager, like the Windows login screen or a Linux display manager (GDM, LightDM, etc.), which manages user logins and sessions.
- After the user logs in, the OS completes the startup process by loading user-specific settings, desktop icons, and any programs set to run at startup.
- Background processes continue to run for core system services, while the CPU schedules tasks to allocate resources efficiently between system processes and user applications.


### For Linux kernel
#### Bootloader
1. The **bootloader** (e.g., **GRUB**) loads the **Linux kernel** and **initial RAM-based filesystem (initramfs)** into memory. The kernel needs `initramfs` to provide essential drivers and tools required for mounting the actual root filesystem.
2. The **initial RAM disk** (`initrd` or `initramfs`) is a temporary, minimal root filesystem loaded into memory. It contains drivers and programs necessary to locate and mount the real root filesystem. This includes disk drivers, mass storage controllers, and filesystem drivers.
#### Kernel Initialization
1. Once the kernel is loaded, it begins its initialization, configuring memory, initializing hardware, and loading any user-space programs required for further setup.
2. The `initramfs` filesystem contains essential programs, binaries, and the **udev** facility (for **U**ser **Dev**ice) which identifies the devices present, locates required drivers, and loads them.
3. Once the root filesystem is located, it is checked for integrity and mounted.
#### Mounting the Root Filesystem
1. The `mount` command is used to tell the OS that the root filesystem is ready, associating it with a specific mount point in the filesystem hierarchy.
	1. When mounting is successful, the kernel discards the `initramfs` from RAM and executes the `init` program located on the root filesystem at `/sbin/init`.
#### `init` Process and System Initialization
1. The **init process** (`/sbin/init`), now the first process started by the kernel with Process ID (PID) 1, becomes the "parent" process of all user-space programs.
2. `init` mounts and pivots to the final root filesystem, which it will manage throughout the system's runtime. If any special hardware drivers are needed before accessing mass storage, they must be included in the `initramfs` image.
3. `init` starts other essential processes to fully initialize the system. It ultimately traces back as the origin for most user-space processes, except for kernel-specific processes, which are started by the kernel itself.
#### Runlevels and System V Legacy
1. Traditionally, the init process managed **runlevels** in the style of **System V UNIX**. Each runlevel defines a system state, with services started or stopped based on the specified mode.
2. Although newer Linux distributions often use `systemd` instead of traditional System V init, they maintain compatibility with runlevel conventions. `systemd` offers more flexible and parallelized service management, reducing boot time.
3. The init process, or `systemd`, is responsible for keeping the system running, starting services, managing processes, and shutting down the system cleanly when required.
#### Graphical Login: Display Manager and X Window System
1. As the final step in the boot process, the **X Window System** is loaded.
2. A service called the **display manager** (e.g., **GDM** or **LightDM**) manages displays, loading the **X server** or **Wayland** (the graphical display system).
3. The X server provides graphical services to applications, which are referred to as X clients. The display manager also handles user logins and, upon successful login, launches the specified desktop environment (e.g., GNOME, KDE) for the user.