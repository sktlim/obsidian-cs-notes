> Most popular P2P protocol

- Collection of all participating peers is called a **torrent**
- Peers download equal size chunks from each other (typical chunk size 256kBs)
- Any peer may leave torrent at any time with only a subset of chunks, and rejoin at a later time
- Each torrent has an infrastructure node called **tracker**

### General Process
1) When peer Alice joins torrent, it registers itself with tracker and periodically informs tracker its still in 
2) Tracker randomly selects 50 peers and sends IP address of peers to Alice, 
3) Alice attempts to establish concurrent TCP connections with all peers 
	1) Successful connections are known as **neighboring peers**
	2) Number fluctuates as peers may leave and join
4) Each peer will have subset of chunks from file, with different peers having different subsets. Alice periodically asks for chunks she doesn't have from neighboring peers 

## Trading Mechanism
### Rarest First
1) Alice uses technique called **rarest first** to decide which chunks to get
	1) Idea: Determine rarest chunk among neighbors and request those chunks first
	2) Rarest chunk gets distributed more quickly, equalizing number of copies everyone has

### Tit-for-Tat
1) Alice gives priority to neighbors supply her data at highest rate
	1) For each of her neighbors, Alice measures rate at which she receives bits and determines 4 peers feeding her at the highest rate
	2) Reciprocates and sends chunks to these peers 
	3) Refreshes this every 10 seconds 
	4) These 4 peers are referred to as **unchoked**
	5) Every 30s, Alice picks an additional neighbor Bob and send him chunks (**optimistically unchoked** neighbor)
		1) Since Alice is sending Bob chunks, she might become 1 of his top 4, then Bob will send her chunks

## Pieces 

## Pipelining

## Random First Selection

## Endgame mode

## Anti-snubbing

