![[Error detection and correction scenario.png|500]]
- At sending node, data $D$ is appended with **Error Detection and Correction (EDC)** bits
- At receiving node, data $D'$ and $EDC'$ are received
- Receiving node tries to detect errors (there may still be undetected bit errors)
	- More sophisticated EDC techniques require more overhead

## Detection Techniques
### Parity Checks
> Use a single **parity bit** 

#### Even Parity Scheme
- Data $D$ has $d$ data bits
- A parity bit is appended such that the $d+1$ bits have an <u>even number of ones</u>
	- E.g. $d = 11100$, parity bit = $1$ to make number of ones even
	- $d= 11110$, parity bit = 0 as number of ones is already even

#### Odd Parity Scheme
- Same as even scheme, just that parity bit is chosen such that number of 1s is odd

#### Observation of single-bit parity scheme
- Receiver knows that data is wrong if the parity bit does not reflect the expected value from the parity scheme. 
- However, if there are an even number of bit flips, this would be an **undetected error**
- Measurements have shown that errors occurs in bursts and this results in high undetected error rates using single bit parity
#### 2D parity
![[2d bit parity 1.png|300]]![[2d bit parity 2.png|357]]
- $D$ divided into $i$ rows and $j$ columns
- Parity value computed for each row and column
- Resulting $i+j+1$ parity bits comprise the error detection bits

- Single error is now detectable and correctable (since the exact bit error can be found)
- Multiple errors can be detected but not corrected

- Ability of receiver to correct errors is known as **forward error correction (FEC)**
	- Common in audio storage and playback devices
	- Allow for error correction without waiting for retransmission and wasting RTT

### Checksum
- $d$ data bits treated as sequences of $k$-bit integers
- Simple method is to sum all sequences and use it as a checksum
	- Method used by Internet checksum (data treated as 16-bit integers and summed, with receiver checking 1s complement of summed data)
- Requires relatively little overhead and is already done in transport layer
- More complex operations like **Cyclic Redundancy Checks (CRC)** can be done in dedicated hardware which can perform it faster

### Cyclic Redundancy Check (CRC)
- Also known as **polynomial codes**

- Sender wants to send $d$ bits of data to receiver
- Sender and receiver agrees on $r+1$ bit pattern known as a **generator** $G$
	- Most significant (leftmost) bit of $G$ must be 1
- Sender then chooses $r$ additional bits and appends them to $d$ such that $(d+r) \% G == 0$ 
- Remainder checks if $(d+r) \% G == 0$ 
	- If true, no errors
	- If false (non-zero), receiver knows error has occurred

- All CRC calculations done in **modulo-2 arithmetic** without carries in addition or borrows in subtraction
	- Addition and subtraction give the same results and are equivalent to **bitwise XOR**
		- $1011 - 0101 == 1011 + 0101 == 1011\oplus0101$
	- Multiplication and division done without carries or borrows
		- Multiplication by $2^k$ bits shifts bit pattern left by k places
#### How sender computes $R$
- Find $R$ such that there is an $n$ such that $D⋅2^r⊕R=nG$  
	- Choose $R$ such that $G$ divides by $D⋅2^r⊕R$ without remainder
$$
\begin{aligned}
D \cdot 2^r \oplus R &= nG \\
D \cdot 2^r &= nG \oplus R \\
\therefore R &= \text{remainder of } \frac{D \cdot 2^r}{G}
\end{aligned}
$$
#### Example CRC Calculation
- **Given**:
	- $D = 101110$ (data bits).
	- $d = 6$ (number of data bits).
	- $G = 1001$ (generator polynomial).
	- $r = 3$ (degree of the generator polynomial).
- **Step 1: Compute $D \cdot 2^r$**:
  $$
  D \cdot 2^r = 101110 \cdot 2^3 = 101110000
  $$
- **Step 2: Perform Modulo-2 Division**:
  - Divide $D \cdot 2^r$ by $G$ to find the remainder $R$:
  $$
  R = \text{remainder of } \frac{D \cdot 2^r}{G}
  $$
- **Step 3: Calculate the Transmitted Code**:
  $$
  \text{Transmitted Code} = D \cdot 2^r \oplus R
  $$
  - Substituting values:
  $$
  \text{Transmitted Code} = 101110000 \oplus R = 101110011
  $$
- **Step 4: Verify**:
  - Receiver checks to ensure:
  $$
  (D \cdot 2^r) \mod G = R
  $$
#### Real-world usage
1. **Standard Generator Polynomials**:
    - CRC-32: `G=1 0000 0100 1100 0001 0001 1101 1011 0111`
    - Defined for **8-, 12-, 16-, and 32-bit generators**.
2. **Error Detection Capabilities**:
    - Detects all burst errors of fewer than $r+1$ bits.
    - Detects burst errors longer than $r+1$ with a probability of $1-2^{-r}$.
    - Detects any odd number of bit errors