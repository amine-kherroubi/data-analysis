# Interpreter Routines

**Interpreter routines** (also called **interpretation routines** or **IRs**) are components of a [[Hypervisor|hypervisor]] that simulate the effect of [[Privileged Instruction|privileged instructions]] that trap when executed in a [[Virtual Machine|virtual machine]].

---

## Purpose

When a privileged instruction executes in a VM and traps to the [[Virtual Machine Monitor|VMM]], an interpreter routine reproduces the state changes that would have occurred if the instruction had executed directly on real hardware.

---

## One Routine Per Instruction

The VMM contains one interpreter routine for each privileged instruction in the architecture (except those handled by the [[Allocator|allocator]]). If there are $m$ privileged instructions, there are $m$ corresponding interpreter routines: ${v_i}$, where $i = 1$ to $m$.

---

## Function

An interpreter routine $v_i$ for instruction $i$ satisfies:

$v_i(S_1) = S_2$ if and only if $i(S_1) = S_2$

where:

- $S_1$ is the virtual machine state before instruction execution
- $S_2$ is the state after execution
- $v_i$ is a sequence of instructions that simulates $i$

The routine produces the identical state transformation that the original instruction would produce.

---

## Implementation Approaches

### State Transition Tables (Theoretical)

For formal proof purposes, Popek and Goldberg describe interpreter routines using state transition tables:

- Each table maps $(E|R, M, P, R)$ states to resulting states
- One table per privileged instruction
- Table size limited by restricting virtual machine memory size $w$

This approach is complete but impractical for real implementations.

### Direct Simulation (Practical)

In practice, interpreter routines directly simulate instruction effects:

- Examine trapped instruction and its operands
- Compute resulting state changes
- Update virtual machine state accordingly
- Return control to the [[Dispatcher|dispatcher]]

---

## Example: Load PSW

For a "Load Program Status Word" instruction:

```
if M = supervisor then
    load new PSW into (M, P, R)
else
    trap
```

The interpreter routine would:

1. Check simulated mode stored by VMM
2. If supervisor, update virtual PSW in VMM data structures
3. If user, simulate a privileged instruction trap
4. Return control with appropriate virtual state

---

## Memory Access

Interpreter routines have access to:

- Virtual machine state saved in location 0
- VMM data structures (simulated mode, virtual PSW, etc.)
- That portion of memory allocated to the VM

They execute in supervisor mode with appropriate memory access privileges.

---

## Related Concepts

- [[Hypervisor]]: Contains interpreter routines as components
- [[Dispatcher]]: Invokes appropriate interpreter routine
- [[Privileged Instruction]]: Instructions requiring interpretation
- [[Equivalence Property]]: Ensured by correct interpreter routines
- [[Virtual Machine Monitor]]: Overall system containing interpreters