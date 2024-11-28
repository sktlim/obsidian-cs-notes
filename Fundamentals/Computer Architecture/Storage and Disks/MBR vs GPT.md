### Summary Table

| Feature             | MBR                            | GPT                                                 |
| ------------------- | ------------------------------ | --------------------------------------------------- |
| **Partition Limit** | 4 primary partitions           | 128 partitions (default)                            |
| **Disk Size Limit** | 2 TB                           | Over 2 TB (up to 18 exabytes theoretically)         |
| **Boot Sector**     | Single boot sector (MBR)       | Protective MBR and GPT header                       |
| **Redundancy**      | None                           | Backup partition table and CRC checks               |
| **Compatibility**   | Works with BIOS and older OSes | Requires UEFI for booting; supported by modern OSes |
| **Security**        | Vulnerable to corruption       | More secure with error checking and backup          |
**Partition limit**
- MBR reserves the first 512 bytes of the disk as the "Master Boot Record" sector, which contains both the bootloader code and the partition table.
- Out of these 512 bytes, only 64 bytes are allocated for partition information
- In those 64 bytes, MBR divides the space equally into four 16-byte sections, with each section representing a single partition entry.
- This layout only allows for a maximum of **four entries**, leading to a maximum of **four primary partitions**.

**Disk size limit**
- MBR limited to 2TB due to 32-bit addressing system it uses, meaning it can only address up to 2^32 sectors
- GPT uses 64-bit addressing, meaning it can address 2^64 sectors (18 exabytes theoretically)

**Boot sector**
- **MBR:** When BIOS starts, it reads MBR, executes bootloader code, and uses it to load OS
- **GPT:** GPT uses a larger "Protective MBR" and "GPT header", preventing older MBR-based utilities from misinterpreting a GPT disk; UEFI loads bootloader directly from EFI System Partition on GPT, bypassing need for dedicated boot sector and allowing a more flexible and secure boot process

