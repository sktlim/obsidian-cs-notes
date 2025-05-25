> Choosing page size accurately within the OS is hard because there is no optimum due to balancing competing factors

## Small vs Large Page Sizes
### Argument for Small Page Sizes
- Randomly chosen text, stack, or data segment will not fit a whole page and on average, half the page will be empty
- This is known as **internal fragmentation**
- With $n$ segments in memory, and page size of $p$ bytes, $np/2$ bytes will be wasted on internal fragmentation, which supports the argument for smaller page sizes
- Large page size will cause more wasted memory than a small page size

### Argument against Small Page Sizes
- However, small page sizes mean that there will be a large number of pages, which results in issues when writing back to non-volatile storage (this is usually done one page at a time)
- This is especially apparent in HDDs, as seek and rotation delays will result in large delay times

- Small pages use up scarce space in TLB, making it beneficial to use large pages whenever possible

## Tradeoff
- OSes use different page sizes for different parts of the system
	- Large pages for kernel, small pages for user processes 
- Some OSes even move a process' memory around to find/ create contiguous ranges of memory for a large page (this is known as **transparent huge pages**)

## Performance Analysis
- Space occupied by page table increases as page size decreases, but internal fragmentation decreases
	- Average process size $s$ bytes
	- Average page size: $p$ bytes
	- Each page entry requires $e$ bytes

	- Approximate number of pages needed per process = $s/p$
	- Space occupied = $se/p$ bytes

	- Wasted memory in last page of process due to internal fragmentation is $p/2$ 

	- Total Overhead: $$Overhead = \text{page table size + internal fragmentation} =se/p + p/2$$
- Page table size large if $p$ is small, internal fragmentation large when $p$ is large
- To find optimum, we take derivative w.r.t $p$ : $$-se/p^2 + 1/2 = 0$$
- Optimum page size: $p = \sqrt{2se}$ 

- If $s$ = 1MB, $e$ = 8 bytes, optimum page size = 4KB