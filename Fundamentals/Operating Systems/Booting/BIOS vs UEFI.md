The main difference between UEFI (Unified Extensible Firmware Interface) and BIOS (Basic Input/ Output System) lies in how they operate, the features they support, and the overall architecture they use. Here’s a breakdown of the key differences:

### 1. Architecture and Interface
   - **BIOS**:
     - BIOS is the older, legacy firmware interface that initializes hardware and loads the operating system.
     - It’s based on a 16-bit processor mode, which restricts it to 1 MB of executable space, limiting its features and speed.
     - BIOS uses text-based interfaces and keyboard navigation, and its code is usually stored in a small firmware chip on the motherboard.

   - **UEFI**:
     - UEFI is the modern replacement for BIOS, providing a more powerful and flexible interface.
     - UEFI operates in 32-bit or 64-bit mode, giving it access to more memory and allowing for richer, GUI-based interfaces with mouse support.
     - UEFI firmware is usually stored in a file on a dedicated partition (EFI System Partition) on the disk rather than in a ROM chip.

### 2. Boot Process
   - **BIOS**:
     - BIOS uses the Master Boot Record (MBR) method to locate the bootloader, which is stored in the first sector of the disk.
     - The MBR is limited to 2 TB of addressable space and supports only four primary partitions per disk.
     - BIOS loads the bootloader into memory and then hands over control to it, starting the OS.

   - **UEFI**:
     - UEFI uses the GUID Partition Table (GPT) method, which supports disks larger than 2 TB and allows for an almost unlimited number of partitions (usually 128 per disk by default).
     - UEFI directly loads bootloaders or OS kernels from the EFI System Partition, bypassing the restrictions of MBR.
     - It can also support faster boot times, as it can load the OS directly without going through a bootloader if the OS is configured for UEFI.
 - See [[MBR vs GPT]]

### 3. Boot Mode: Legacy vs. Secure Boot
   - **BIOS**:
     - BIOS operates in "Legacy" mode and doesn’t support advanced security features like Secure Boot.
     - Since it has no mechanism to verify the bootloader or OS, it’s more vulnerable to malware attacks (e.g., bootkits).

   - **UEFI**:
     - UEFI introduces a feature called Secure Boot, which requires all software loaded during the boot process (bootloaders, drivers, and OS) to be signed with a trusted digital certificate.
     - Secure Boot helps protect against unauthorized code (like rootkits or malware) from loading at startup.
     - UEFI can operate in either Legacy mode (for backward compatibility) or native UEFI mode.

### 4. Extensibility and Flexibility
   - **BIOS**:
     - BIOS is limited in functionality and extensibility due to its design and architecture.
     - It relies heavily on interrupt calls and has limited support for modern hardware, networks, and storage types.

   - **UEFI**:
     - UEFI supports a modular and extensible framework, allowing manufacturers to add drivers and applications (such as diagnostics, network capabilities, and system monitoring).
     - UEFI includes built-in support for networking, which allows for remote diagnostics and configuration (useful for troubleshooting or managing computers over a network).
     - It can also access larger storage volumes and more advanced hardware directly during boot, making it more versatile and capable.

### 5. Storage Support
   - **BIOS**:
     - Limited to MBR-based drives with a 2 TB maximum size.
     - Only supports up to four primary partitions per disk.

   - **UEFI**:
     - Supports both MBR and GPT-based drives.
     - GPT allows disks larger than 2 TB and supports up to 128 partitions by default on Windows.

### 6. Speed and Efficiency

   - **BIOS**:
     - BIOS takes more time to initialize hardware and load the OS because of its older, less efficient design.
     - It lacks optimizations for modern, faster hardware.

   - **UEFI**:
     - UEFI can offer faster boot times because it doesn’t have to go through as many steps as BIOS and can access hardware more directly.
     - Many UEFI systems implement features like “[[Fast Boot]],” which bypasses certain POST checks for an even quicker startup.

### 7. Support for Modern Features

   - **BIOS**:
     - Lacks support for many modern features, such as large storage volumes, advanced security, and high-performance I/O.
     - It’s gradually being phased out, with newer systems increasingly defaulting to UEFI.

   - **UEFI**:
     - UEFI is built with modern systems in mind, supporting larger storage devices, faster boot processes, and improved security.
     - It’s compatible with modern CPUs, GPUs, SSDs, and other components, making it a better choice for high-performance and secure environments.

---

### Summary Table

| Feature                | BIOS                        | UEFI                      |
|------------------------|-----------------------------|----------------------------|
| **Bit Mode**           | 16-bit                      | 32-bit or 64-bit           |
| **User Interface**     | Text-based, keyboard only   | GUI with mouse support     |
| **Partition Style**    | MBR (max 2 TB, 4 partitions)| GPT (supports >2 TB, more partitions) |
| **Boot Speed**         | Slower                      | Faster, with options like Fast Boot |
| **Boot Security**      | No Secure Boot              | Secure Boot available      |
| **Extensibility**      | Limited                     | Highly extensible, supports add-ons |
| **Storage**            | Limited to 2 TB             | Supports larger than 2 TB  |
| **Network Support**    | Minimal                     | Built-in support           |
| **Future**             | Legacy, being phased out    | Modern standard, actively developed |

UEFI is the newer and more advanced replacement for BIOS, supporting modern hardware, security features, and larger storage devices. Most newer computers now ship with UEFI as the default boot firmware.

TARGET DECK: Operating Systems::Booting
## Flashcards

Q: What architectural limitations make BIOS less powerful than UEFI?
A: BIOS operates in 16-bit mode with only 1 MB of executable space, limiting speed and features, while UEFI operates in 32/64 bit.
<!--ID: 1748147348097-->

Q: How does UEFI’s boot process eliminate the limitations of MBR used by BIOS?
A: UEFI uses GPT, allowing disks >2 TB and more than four partitions, and can load OS kernels directly from the EFI System Partition.
<!--ID: 1748147348106-->


Q: What security mechanism does UEFI provide that BIOS lacks?
A: UEFI supports Secure Boot, which ensures only trusted, signed software can run at boot.
<!--ID: 1748147348109-->


Q: Why is BIOS more vulnerable to boot-time malware than UEFI?
A: BIOS lacks mechanisms to verify bootloaders or OS integrity, making it susceptible to bootkits.
<!--ID: 1748147348111-->


Q: How does UEFI enable greater extensibility and flexibility than BIOS?
A: UEFI supports modular drivers, applications, and built-in networking, while BIOS is limited by its legacy architecture.
<!--ID: 1748147348114-->


Q: In what way does UEFI improve boot speed over BIOS?
A: UEFI can bypass legacy boot steps and utilize features like Fast Boot for quicker startup.
<!--ID: 1748147348116-->


Q: What allows UEFI to support remote diagnostics and pre-boot networking?
A: UEFI has built-in networking capabilities, unlike BIOS which offers minimal or no network support.
<!--ID: 1748147348118-->

Q: Summarize the key differences between UEFI and BIOS.
A: Bit mode, partition style, Fast Boot, Secure Boot and 2TB storage limit. 
<!--ID: 1748183455171-->
