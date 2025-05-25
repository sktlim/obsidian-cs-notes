## When power button is pressed
1) Signal is sent from motherboard to PSU (Power supply unit), which provides constant supply to computer. Once motherboard receives power good signal, it tries to start CPU
2) CPU starts working in **real mode**
	- **real mode:** addresses correspond directly to physical address in memory
	- In general, real mode's memory map is as follows:
		```
		0x00000000 - 0x000003FF - Real Mode Interrupt Vector Table
		0x00000400 - 0x000004FF - BIOS Data Area
		0x00000500 - 0x00007BFF - Unused
		0x00007C00 - 0x00007DFF - Our Bootloader
		0x00007E00 - 0x0009FFFF - Unused
		0x000A0000 - 0x000BFFFF - Video RAM (VRAM) Memory
		0x000B0000 - 0x000B7777 - Monochrome Video Memory
		0x000B8000 - 0x000BFFFF - Color Video Memory
		0x000C0000 - 0x000C7FFF - Video ROM BIOS
		0x000C8000 - 0x000EFFFF - BIOS Shadow Area
		0x000F0000 - 0x000FFFFF - System BIOS
		```
	- **Intel 8086** contains 16-bit registers but 20-bit address bus, so it uses memory segmentation to work with the `0xFFFFF` (1MB/20-bit) address space
		- 16-bits allow us to work with `0xFFFF` (64kB/16-bit) address space
		- Address contains 2 parts: **segment selector** and **offset**
			- In real-mode, associated base address of segment is `segment selector * 16`
			- `Physical Address = segment selector * 16 + offset`
				- `CS:IP = 0x2000:0x0010` means `Physical Addr: 0x20010 (0x2000 << 4 + 00x10)`
			- **Note:** Max limit addressable is meant to be 1MB, but `0xffff:0xffff = 0x10ffef` which is 65520 bytes above 1MB.
				- A20 line disabled to deny access to memory addresses above 1MB
				- Addresses beyond will just wrap around
	- See [[Intel i386]] for more details on registers and microarchitecture
	- **CS (Code Segment) Register** loaded with `0xf000` and base address loaded with `0xffff0000`
	- Starting address is `0xfffff000` (16 bytes below 4GB) which is the **reset vector**
	- **Reset vector:** Memory location of first instruction after reset; contains `jmp` instruction that points to BIOS
		- Coreboot source code example:
			```
			    .section ".reset", "ax", %progbits //defines reset section. ax marks section as allocatable and executable; progbits specifies program contains program data
			    .code16                // Switches assembler to 16-bit mode
			.globl    _start           // declares _start as global label
			_start:
			    .byte  0xe9
			    .int   _start16bit - ( . + 2 ) // 
			...
			```
			- `jmp` opcode: `0xe9`, destination address `_start16bit - ( . + 2 )`
			- `.int _start16bit - ( . + 2 )`: Defines the 16-bit offset for the `jmp` instruction.
				- `_start16bit`: The target address for the jump, which is defined elsewhere.
				- `.`: Represents the current address in the section.
				- `( . + 2 )`: Accounts for the 2 bytes taken up by the offset in the `jmp` instruction.
				- The offset is calculated as `_start16bit - ( . + 2 )` to ensure the jump lands exactly at `_start16bit`.
		- Reset section (16 bytes):
			```
			SECTIONS {
			    /* Trigger an error if I have an unuseable start address */
			    _bogus = ASSERT(_start16bit >= 0xffff0000, 
				    "_start16bit too low. Please report.");
			    _ROMTOP = 0xfffffff0;
			    . = _ROMTOP; // Sets the current location counter . to _ROMTOP
			    .reset . : {
			        *(.reset);  // Includes all .reset sections from input files into location
					. = 15;  // Moves location ctr to 15 bytes into section (reserves 15 bytes for .reset)
			        BYTE(0x00); //Places byte 0x00 at cur location (padding)
			    }
			}
			```
			- Compiled to start from `0xffff0000`
	- BIOS starts
		- Initializes and checks hardware
		- BIOS attempts to boot from hard drive
			- BIOS config stores boot order controlling devices to boot from
			- For hard drives partitioned with **Master Boot Record (MBR)**, bootloader is stored in first 446 bytes of first sector (sector of size 512 bytes)
				- Final 2 bytes of boot sector is `0x55` and `0xaa` designating bootable sector
			- Boot Sector Code Example:
				```
				;
				; Note: this example is written in Intel Assembly syntax
				;
				[BITS 16] //informs assembler code written for 16 bit mode
				
				boot:
					mov al, '!'     //Moves `!` into AL register
					mov ah, 0x0e    //Sets func code for BIOS interrupt 0x10 to 0x0e (teletype output function)
					mov bh, 0x00    //Specifies page number (0x00 for active display page)
					mov bl, 0x07    //Sets the text attribute to `0x07`, which is the standard gray-on-black text in VGA text mode.
				
					int 0x10        //Executes 0x10 interrupt which basically just prints whatever in al
					jmp $           //Causes infinite loop (otherwise CPU might execute garbage instructions)
				
				times 510-($-$$) db 0 //Fills rest of file with 0 until last byte
				
				db 0x55
				db 0xaa
				```
				Build and run this with:
				
				```
				nasm -f bin boot.nasm && qemu-system-x86_64 boot
				```
	- At start of execution, BIOS in ROM, not RAM

## Bootloader
- BIOS chosen boot device and transferred control to boot sector code
- Execution starts from `boot.img` (contains pointer to core image of bootloaders like GRUB)
- Core image begins with 