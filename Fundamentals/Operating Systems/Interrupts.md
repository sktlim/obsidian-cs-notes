

## Interrupts for IO devices
- Interrupts happen at inconvenient moments, hence CPU has a way to disable and reenable them later
- When interrupts disabled, devices can still assert interrupt signals, but CPU does not process until interrupts enabled again 

![[Interrupt example.png]]
**Part a**
1) Driver tells controller what to do by writing into its device registers; Controller starts device. 
2) When controller finishes R/W, it signals interrupt controller using certain bus lines
3) If interrupt controller ready to handle interrupt (may not if handling higher priority one), it asserts a pin on CPU telling it 
4) Interrupt controller puts number of device on bus so that CPU can read and know which device finished

**Part b**
1) Once CPU decides to take interrupt, PC and PSW are pushed onto current stack and CPU switch into kernel mode. Device number may be used as index to part of memory(interrupt vector) to find address of interrupt handler for device 
2) Once interrupt handler started, it removes stacked programs and PSW and saves them, then queries device to learn status
	1) When handler finished, returns to previously running program



