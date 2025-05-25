See also [[MBR vs GPT]], [[BIOS vs UEFI]] 

![[Example fs layout.png]]
## Master Boot Record (MBR)
- Sector 0 of disk
- End of MBR contains partition table which gives starting and ending address of each partition

- 1 of the partitions is marked as active
- When computer is booted, BIOS reads in and executes MBR
- MBR locates active partition, reads the first block (**boot block**) and executes it
- Program in boot block loads OS contained in that partition
- For uniformity, all partitions begin with a boot block

- **Superblock**
	- Contains key params about filesystem and will load into memory when booted (or when file system just touched)
	- Includes magic number to identify system-type, number of blocks in system


## GUID Partition Table (GPT) through Unified Extension Firmware Interface (UEFI)
- **GUID**: Globally Unique Identifiers
![[UEFI Layout.png]]
- Offers fast booting, supports different architectures, and disk sizes up to 8 ZiB
- UEFI looks for location of **GPT** in 2nd block of device
	- UEFI keeps a backup of GPT in the last block
	- Reserves 1st block as a special marker for software expecting MBR

- GPT contains start and end of each partition
- Once GPT is found, firmware has enough functionality to read file systems of some types 
	- UEFI standard dictates firmware should at least read FAT filesystems
	- The **EFI System Partition (ESP)**, placed in a special disk partition, can also be read

- Boot process can now use proper filesystem instead of a magic MBR 
-  UEFI also expects firmware to be able to execute programs in **Portable Executable (PE)** format
