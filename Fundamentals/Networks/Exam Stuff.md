## Calculate Network Resilience


### Serial Reliability
In a series network, the **overall reliability** is the **product of the individual reliabilities** of the components.$$R_{\text{serial}} = \prod_{i=1}^n p_i$$
If all components have the same reliability $p$:$$R_{\text{serial}} = p^n$$
### Parallel Reliability
In a parallel network, the overall reliability is the complement of the **probability that all components fail**.$$R_{\text{parallel}} = 1 - \prod_{i=1}^n (1 - p_i)$$
If all components have the same reliability $p$:$$R_{\text{parallel}} = 1 - (1 - p)^n$$

# Channel Utilization: Key Formulas

## Stop-and-Wait ARQ
The channel utilization for Stop-and-Wait ARQ is given by:

$$
U_{\text{Stop-and-Wait}} = \frac{1 - P}{1 + 2a}
$$

Where:
- $P$: Frame error probability.
- $a$: Normalized propagation delay, defined as $a = \frac{T_{\text{propagation}}}{T_{\text{frame}}}$.

---

## Go-Back-N ARQ
For Go-Back-N ARQ, the channel utilization formula is complex. 
Instead, the same utilization formula as Selective Reject ARQ can be used:

$$
U_{\text{ARQ,GBN}} \approx U_{\text{Selective Reject}}
$$

---

## Sliding Window (No Errors)
The channel utilization for Sliding Window without errors is:

$$
U_{\text{Sliding Window}} =
\begin{cases} 
1, & N \geq 2a + 1 \\
\frac{N}{1 + 2a}, & N < 2a + 1
\end{cases}
$$

Where:
- $N$: Window size.

---

## Selective Reject ARQ
The channel utilization for Selective Reject ARQ is given by:

$$
U_{\text{Selective Reject}} =
\begin{cases} 
1 - P, & N \geq 2a + 1 \\
\frac{N(1 - P)}{1 + 2a}, & N < 2a + 1
\end{cases}
$$

Where:
- $P$: Frame error probability.
- $a$: Normalized propagation delay.
- $N$: Window size.

---

## Summary of Parameters
- $P$: Frame error probability.
- $a$: Normalized propagation delay.
- $N$: Window size.
- $U$: Channel utilization (ranges between 0 and 1).

## Formulas

| Protocol                | Formula                                                                                                   | Conditions                   | Parameters                                                                                                                                 |
|-------------------------|-----------------------------------------------------------------------------------------------------------|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| **Stop-and-Wait ARQ**   | $$U_{\text{Stop-and-Wait}} = \frac{1 - P}{1 + 2a}$$                                                       | N/A                          | $P$: Frame error probability, $a$: Normalized propagation delay ($a = \frac{T_{\text{propagation}}}{T_{\text{frame}}}$).                   |
| **Go-Back-N ARQ**       | $$U_{\text{ARQ,GBN}} \approx U_{\text{Selective Reject}}$$                                                | N/A                          | Uses the same utilization formula as Selective Reject ARQ for simplicity.                                                                  |
| **Sliding Window**      | $$U_{\text{Sliding Window}} = \begin{cases} 1, & N \geq 2a + 1 \\ \frac{N}{1 + 2a}, & N < 2a + 1 \end{cases}$$ | $N \geq 2a + 1$ or $N < 2a + 1$ | $N$: Window size, $a$: Normalized propagation delay.                                                                                        |
| **Selective Reject ARQ**| $$U_{\text{Selective Reject}} = \begin{cases} 1 - P, & N \geq 2a + 1 \\ \frac{N(1 - P)}{1 + 2a}, & N < 2a + 1 \end{cases}$$ | $N \geq 2a + 1$ or $N < 2a + 1$ | $P$: Frame error probability, $a$: Normalized propagation delay, $N$: Window size.                                                        |

---

## Notes
1. **Stop-and-Wait ARQ**: Utilization decreases significantly for large $a$ due to waiting time.
2. **Go-Back-N ARQ**: Simplifies computation by reusing Selective Reject ARQ formulas.
3. **Sliding Window**: Achieves perfect utilization ($U = 1$) when $N \geq 2a + 1$.
4. **Selective Reject ARQ**: More efficient than Go-Back-N for handling errors due to selective retransmissions.

# ALOHA and Slotted ALOHA: Summary

## ALOHA
- **Description**: A simple random access protocol where nodes send data whenever they have it. Collisions occur if multiple nodes transmit simultaneously. If collision occurs, node immediately retransmits with probability $p$ Otherwise, node waits for frame transmission time, and then transmit frame with probability $p$, or wait for frame transmission time again with probability $1-p$
- **Key Features**:
  - No time synchronization.
  - Collisions reduce efficiency.

### Formula for Throughput
$$S = G \cdot e^{-2G}$$

Where:
- $S$: Throughput (successful transmissions per unit time).
- $G$: Offered load (average number of transmission attempts per unit time).

- **Maximum Throughput**:
  $$S_{\text{max}} = \frac{1}{2e} \approx 0.184$$

---

## Slotted ALOHA
- **Description**: Time is divided into slots, and nodes can only transmit at the beginning of a slot. This reduces the chance of collisions compared to pure ALOHA. If collide, node detects collision before end of slot, and retransmits its frame in each subsequent frame with probability $p$ until its frame gets retransmitted without a collision
- **Key Features**:
  - Time-synchronized slots.
  - Higher efficiency than ALOHA.

### Formula for Throughput
$$S = G \cdot e^{-G}$$

Where:
- $S$: Throughput.
- $G$: Offered load.

- **Maximum Throughput**:
  $$S_{\text{max}} = \frac{1}{e} \approx 0.368$$

---

## Comparison of ALOHA and Slotted ALOHA

| **Protocol**     | **Time Synchronization** | **Throughput Formula**          | **Maximum Throughput**       |
|-------------------|--------------------------|----------------------------------|------------------------------|
| **ALOHA**         | No                       | $$S = G \cdot e^{-2G}$$          | $$\frac{1}{2e} \approx 0.184$$ |
| **Slotted ALOHA** | Yes                      | $$S = G \cdot e^{-G}$$           | $$\frac{1}{e} \approx 0.368$$  |

---

### Key Takeaways
- Slotted ALOHA improves efficiency by reducing collisions, doubling the maximum throughput compared to ALOHA.
- Both protocols suffer from performance degradation at high offered loads ($G$).

# CSMA (Carrier Sense Multiple Access)

## Non-Persistent CSMA
- **Description**:
  - A station senses the medium before transmitting.
  - **Steps**:
    1. If the medium is idle, transmit immediately.
    2. If the medium is busy, back off for a random amount of time and repeat Step 1.
  - Stations are deferential and respect other transmissions.

- **Performance**:
  - Random delays reduce the probability of collisions, as stations wait for different amounts of time before retrying.
  - Bandwidth may be wasted if the waiting time (backoff) is large, as the medium may remain idle unnecessarily.

---

## 1-Persistent CSMA
- **Description**:
  - A station listens to the medium and transmits as soon as it becomes idle with a probability of 1.
  - **Steps**:
    1. If the medium is idle, transmit immediately.
    2. If the medium is busy, continuously sense the medium until it becomes idle, then transmit immediately with probability 1.
  - Stations are considered "selfish" since they transmit immediately after detecting an idle medium.

- **Performance**:
  - Reduces idle channel time.
  - Collisions are guaranteed if two or more stations become ready at the same time.

---

## P-Persistent CSMA
- **Description**:
  - Time is divided into discrete time slots, typically equal to the maximum propagation delay.
  - **Steps**:
    1. If the medium is idle:
       - Transmit with probability $p$, or
       - Wait one time slot with probability $1-p$, then repeat Step 1.
    2. If the medium is busy, continuously sense the medium until it becomes idle, then repeat Step 1.
  - Stations are "wise" in balancing collisions and idle channel time.

- **Performance**:
  - Reduces collisions (like Non-Persistent CSMA).
  - Minimizes idle channel time (like 1-Persistent CSMA).

---

## Summary of CSMA Types

| **Type**           | **Medium Sensing Behavior**                                              | **Collision Probability**             | **Idle Channel Time**             | **Key Trade-offs**                                           |
|---------------------|-------------------------------------------------------------------------|---------------------------------------|------------------------------------|-------------------------------------------------------------|
| **Non-Persistent**  | Wait a random amount of time if medium is busy, then reattempt.         | Low                                   | High if backoff time is large.    | Avoids collisions but may waste bandwidth during backoff.   |
| **1-Persistent**    | Transmit immediately when the medium is idle.                          | High if multiple stations ready.      | Low                               | Reduces idle time but increases the chance of collisions.   |
| **P-Persistent**    | Transmit with probability $p$ or wait for one time slot with $1-p$.     | Moderate                              | Moderate                          | Balances collisions and idle channel time effectively.      |

# Collision Detection and CSMA/CD Protocol

## Collision Detection
- **Question**: How long does it take to detect a collision?
- **Answer**: In the worst case, it takes **twice the maximum propagation delay** of the medium.
  - Let $a$ represent the maximum propagation delay:
    - Collision detection time = $2a$

---

## CSMA/CD (Carrier Sense Multiple Access with Collision Detection)

### Protocol Steps
1. **Transmission**:
   - Use one of the CSMA persistent algorithms (Non-Persistent, 1-Persistent, or P-Persistent).

2. **Collision Detection**:
   - If a station detects a collision during its transmission:
     - **Abort transmission** immediately.

3. **Jam Signal**:
   - Transmit a **jam signal** (typically 48 bits) to:
     - Notify other stations of the collision.
     - Ensure that the collision is detected by all stations, even the furthest ones.
     - Ensure all stations discard the collided frame.

4. **Backoff**:
   - After sending the jam signal:
     - **Wait for a random amount of time** (backoff period).
     - Retry transmission.

---

### Key Notes
- **Purpose of Jam Signal**:
  - Ensures all stations are aware of the collision.
  - Prevents further transmission attempts during the collision resolution period.
- **Backoff Mechanism**:
  - Introduces random delays to minimize the chance of repeated collisions when stations retry their transmissions.

# Multi-Access Reservation Protocol (MARP)

## Overview
MARP is a two-phase protocol designed to manage multi-access communication.

### Two Phases
1. **Phase 1: Channel Reservation**
   - Stations compete to reserve the channel.
   - Reservation trials are used to determine which station can transmit.

2. **Phase 2: Data Transmission**
   - Once a station successfully reserves the channel, it transmits data.

---

## MARP Transmission Window

### Key Parameters
- $u$: Length of the data transmission phase (Phase 2).
- $v$: Length of the reservation trial phase (Phase 1).
- $S_r$: Channel utilization in the reservation phase.

### Number of Reservation Trial Frames
- **Geometric Distribution**:
  - Probability of success on the $k$-th trial:
    $$P(X = k) = S_r (1 - S_r)^{k-1}$$
  - Expected number of trials:
    $$E[X] = \frac{1}{S_r}$$

### Average Transmission Window
The average time for one complete reservation and transmission cycle:
$$u + \frac{v}{S_r}$$

---

## MARP Utilization

### Formula for Utilization
Utilization $S$ is the ratio of the time used for message transmission to the total transmission window:

1. **Reservation frame not used for message data bits**:
   $$S = \frac{u}{u + \frac{v}{S_r}}$$

2. **Reservation frame used for message data bits**:
   $$S = \frac{u}{(u - v) + \frac{v}{S_r}}$$

---

## Key Points
- **Reservation Phase Efficiency**:
  - The probability of successfully reserving the channel ($S_r$) impacts the protocol's overall efficiency.
  - Higher $S_r$ leads to fewer reservation trials and better utilization.

- **Utilization Insights**:
  - Longer reservation phases ($v$) reduce overall utilization.
  - Efficient use of reservation frames can significantly improve performance.

## MARP Summary Table

| **Phase**                  | **Description**                                                                                     | **Key Parameters**                          | **Formula**                                                                                  |
|----------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|----------------------------------------------------------------------------------------------|
| **Phase 1: Reservation**   | Stations compete to reserve the channel.                                                           | $v$: Reservation phase duration, $S_r$: Reservation success probability. | $P(X = k) = S_r (1 - S_r)^{k-1}$ (geometric distribution), $E[X] = \frac{1}{S_r}$             |
| **Phase 2: Transmission**  | The reserved station transmits its data.                                                           | $u$: Data transmission phase duration.      | Average transmission window: $u + \frac{v}{S_r}$                                             |
| **Utilization (No Data in Reservation Frame)** | Ratio of message transmission time to the total transmission window. | $u$: Transmission phase length, $v$: Reservation phase length, $S_r$: Reservation success probability. | $S = \frac{u}{u + \frac{v}{S_r}}$                                                            |
| **Utilization (Data in Reservation Frame)**    | Utilization when the reservation frame is used for message data.                       | $u$: Transmission phase length, $v$: Reservation phase length, $S_r$: Reservation success probability. | $S = \frac{u}{(u - v) + \frac{v}{S_r}}$                                                      |

---

## Notes
- **Efficiency**: Utilization improves with higher $S_r$ (higher probability of successful reservation).
- **Impact of $v$**: Larger reservation durations ($v$) reduce overall utilization unless used efficiently.
- **Geometric Distribution**: The number of trials for channel reservation follows a geometric distribution with expected value $E[X] = \frac{1}{S_r}$.
