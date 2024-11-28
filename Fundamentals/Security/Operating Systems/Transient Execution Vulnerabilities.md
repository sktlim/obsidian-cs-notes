> Vulnerabilities that exploit CPU optimizations (speculative and out-of-order execution) to leak sensitive data via side channels.

## Key Concepts
1. **Speculative Execution**: 
    - CPU predicts and executes instructions in advance to improve speed.
    - Results are discarded if predictions are incorrect, but traces may remain.
2. **Out-of-Order Execution**:
    - CPU executes instructions non-sequentially for efficiency.
    - Temporary data can be exposed in the cache, creating security risks.
3. **Transient Execution**: 
    - Transient states occur during speculative/out-of-order execution where data is processed but not committed.
    - Can expose data indirectly via the CPU cache.
## Types of Vulnerabilities
1. **Spectre**:
    - Exploits speculative execution.
    - Manipulates branch prediction to leak information via cache side channels.
    - Variants: Spectre v1 (Bounds Check Bypass), Spectre v2 (Branch Target Injection).
2. **Meltdown**:
    - Exploits out-of-order execution.
    - Allows access to restricted memory (e.g., kernel memory).
    - Primarily affects Intel CPUs.
## Data Leakage Mechanism
- **Cache Side Channel**:
    - During speculative execution, data may be loaded into the cache.
    - Attackers use **cache timing attacks** to infer data based on what remains in the cache.
## Mitigations
1. **Software Patches**:
    - OS-level fixes, e.g., Kernel Page Table Isolation (KPTI) for Meltdown.
    - Bounds checking and branch restrictions to limit Spectre attacks.
2. **CPU Microcode Updates**:
    - Updates from CPU vendors to adjust speculative execution and branch prediction.
3. **Hardware Redesign**:
    - Future CPUs aim to reduce vulnerabilities by altering speculative execution and cache management.