# Equivalence Property

The **equivalence property** (also called **fidelity**) is the first essential characteristic that a [[Virtual Machine Monitor|virtual machine monitor]] must satisfy. It guarantees that programs execute with identical functional behavior in a [[Virtual Machine|virtual machine]] as they would on real hardware.

---

## Formal Definition

Any program $K$ executing with a VMM resident must perform in a manner functionally indistinguishable from the case when the VMM did not exist and $K$ had direct access to hardware, with two acceptable exceptions.

---

## Mathematical Characterization

For any instruction sequence $t = i_1 i_2 ... i_n$ and any real machine state $S_1$:

If $S_2 = t(S_1)$ on the real machine, then $f(S_2) = t'(f(S_1))$ in the virtual machine system

where:

- $f$ is the [[Virtual Machine Map|virtual machine map]]
- $t'$ is the corresponding instruction sequence in the virtualized environment
- The final states are equivalent under the mapping $f$

---

## Two Permitted Exceptions

The equivalence property allows two categories of acceptable differences:

### 1. Resource Availability

The virtual machine may have different quantities of resources than the physical machine:

- Less memory due to VMM occupying space
- Fewer or different I/O devices
- Limited disk space for virtual machine storage

The VM behaves equivalently to a smaller or differently configured physical machine.

### 2. Timing Dependencies

Instruction sequences may execute more slowly due to:

- VMM intervention when handling [[Privileged Instruction|privileged instructions]]
- Context switches between concurrent VMs
- Overhead of [[Trap Mechanism|trap]] and interpretation mechanisms

Programs with timing dependencies (timeouts, real-time constraints) may observe these differences.

---

## What Is Not Equivalent

The equivalence property explicitly excludes traditional time-sharing operating systems because:

- They do not provide the same instruction set to programs
- They intercept and modify system calls
- They provide abstractions rather than hardware duplication

---

## Verification

Equivalence is verified by showing that for any program:

- If the program halts in state $S_2$ on real hardware starting from $S_1$
- Then it halts in state $f(S_2)$ on the virtual machine starting from $f(S_1)$

This holds for all programs that do not depend on specific resource amounts or precise timing.

---

## Implementation Guarantee

The equivalence property is guaranteed by:

- Correct [[Interpreter Routines|interpreter routines]] for all [[Privileged Instruction|privileged instructions]]
- Direct execution of [[Innocuous Instruction|innocuous instructions]]
- Proper state mapping between real and virtual machines
- Accurate simulation of hardware behavior

---

## Contrast with Other Systems

|System Type|Equivalence|
|---|---|
|VMM|Yes (with exceptions)|
|Emulator|No (different architecture)|
|Simulator|No (too slow, different interface)|
|OS|No (different abstraction)|

---

## Related Concepts

- [[Virtual Machine Monitor]]: Must satisfy equivalence property
- [[Efficiency Property]]: Second VMM requirement
- [[Resource Control Property]]: Third VMM requirement
- [[Virtual Machine Map]]: Mathematical formalization of equivalence
- [[Interpreter Routines]]: Ensure equivalent instruction behavior