### Some definitions
**Word line:** A **word line** is a horizontal control line that connects to a row of memory cells. When a word line is activated (set high), it enables access to all the memory cells in that row, allowing them to connect to their respective bit lines for reading or writing data.

**Bit Line**: A **bit line** is a vertical line that connects to each memory cell in a column. Bit lines carry the actual data signals (either 0 or 1) during read and write operations. In DRAM and SRAM, bit lines are used to read the stored value from a memory cell or to write a new value into the cell by charging or discharging the connected capacitor (in DRAM) or adjusting the transistor latch state (in SRAM).

![[Word and bit lines.png]]
### SRAM
- Static Random Access Memory
- **Implementation**
	- Uses flip flops to store each bit
	- A flip-flop for a memory cell takes 4 or 6 transistors along with some wiring, but never has to be refreshed, making it faster than DRAM, but conversely more expensive due to the higher number of transistors taking up more space, meaning less memory per chip
- **Properties**
	- Volatile memory; Loses data when power is removed
	- Exhibits properties of [data remanence](https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-536.pdf) (residual data when data is supposed to be cleared)
	- Good performance and reliability; Power consumption low when idle
- **R/W operations**
	- Write operation:
		- **Enable the Word Line**: The word line (WL) connected to the cell is activated, connecting the SRAM cell to the bit lines.
		- **Set Bit Lines**: Bit lines (BL and BL') are driven to the desired values (high or low) to represent the bit being written.
		- **Latch Data**: The flip-flop in the SRAM cell latches onto the bit line values, storing the bit.
		- **Deactivate Word Line**: After the data is written, the word line is disabled, isolating the cell from the bit lines.
	- Read operation:
		- **Enable the Word Line**: The word line is activated, connecting the cell to the bit lines.
		- **Sense Amplifier**: The bit lines are pre-charged, and as the cell is connected, a small voltage difference appears between BL and BL', depending on the stored bit (0 or 1).
		- **Amplify and Read**: A sense amplifier detects the voltage difference and amplifies it, interpreting the value as either 0 or 1.
		- **Deactivate Word Line**: The word line is turned off to end the read cycle.


### DRAM
- Dynamic Random Access Memory
- **Implementation** 
	- Uses capacitors to store bit
	- Most designs use 1 capacitor, 1 transistor
	- To prevent capacitor from discharging and memory from being lost, DRAM uses an external memory refresh circuit to refresh each row (typically every 64ms) 
- **Properties**
	- Volatile memory
	- Exhibits limited data remanence
- **R/W Operations**
	- Write operation
		- **Activate Word Line**: The word line for the desired row is enabled, connecting the row of DRAM cells to the bit lines.
		- **Set Bit Line**: The bit line is driven to a voltage representing the data bit (high for 1, low for 0).
		- **Store Charge in Capacitor**: The transistor in each cell allows the capacitor to charge or discharge according to the bit line, storing the bit.
		- **Deactivate Word Line**: After writing, the word line is disabled, isolating the cell and preserving the capacitor’s charge.
	- Read Operation:
		- **Activate Word Line**: The word line is enabled, connecting the row of cells to the bit lines.
		- **Sense Amplifier**: The capacitor’s small charge affects the bit line, creating a slight voltage difference.
		- **Amplify Signal**: A sense amplifier detects and amplifies this difference to read the stored bit (0 or 1).
		- **Restore Charge (Refresh)**: Reading in DRAM is destructive; it depletes the capacitor’s charge. To retain data, the cell is immediately refreshed after each read or every 64ms refresh cycle