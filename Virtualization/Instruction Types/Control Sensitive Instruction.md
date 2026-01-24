# Control Sensitive Instruction

A **control sensitive instruction** is a type of [[Sensitive Instruction|sensitive instruction]] that attempts to change the amount of resources available or affects the processor mode without going through the memory trap sequence.

---

## Formal Definition

Instruction $i$ is control sensitive if there exists a state:

$S_1 = (e_1, m_1, p_1, r_1)$

such that:

$i(S_1) = S_2 = (e_2, m_2, p_2, r_2)$

where $i(S_1)$ does not memory trap, and either:

1. $r_1 \neq r_2$ (relocation-bounds register changed), or
2. $m_1 \neq m_2$ (processor mode changed), or both

---

## What Makes an Instruction Control Sensitive

An instruction is control sensitive if it has the potential to:

### 1. Modify Resource Allocation

- Change the relocation-bounds register
- Modify memory allocation boundaries
- Alter available memory size
- Change I/O device assignments (in extended models)

### 2. Change Processor Mode

- Switch from user to supervisor mode
- Switch from supervisor to user mode
- Modify privilege level
- Alter protection rings

---

## Key Characteristic: Resource Control

Control sensitive instructions are critical for [[Resource Control Property|resource control]] because they:

- Directly affect system resource distribution
- Can violate isolation between [[Virtual Machine|virtual machines]]
- Must be intercepted by the [[Virtual Machine Monitor|VMM]]
- Require [[Allocator|allocator]] intervention for validation

---

## Examples

### Memory Resource Control

```
LPSW    // Load Program Status Word - IBM System/360
        // Can modify relocation-bounds register and mode

LBAR    // Load Base and Address Register - Honeywell 6000  
        // Directly modifies memory allocation boundaries
```

### Mode Control

```
JRST 1  // Jump and Restore - DEC PDP-10
        // Returns to user mode from supervisor mode
        // Control sensitive but NOT privileged (problematic!)

RTI     // Return from Interrupt - PDP-11
        // Restores previous processor mode
```

### Combined Control

```
RFE     // Return From Exception - MIPS
        // Modifies both mode and potentially memory state
```

---

## Importance for Virtualization

Control sensitive instructions are fundamental to the [[Popek-Goldberg Virtualization Theorem|virtualization theorem]]:

**All control sensitive instructions must be [[Privileged Instruction|privileged]]** for an architecture to be virtualizable.

If a control sensitive instruction is not privileged:

- It will not trap when executed by a VM
- The VMM cannot intercept it
- Resource control is violated
- The architecture is not classically virtualizable

---

## Handling by VMM

When a control sensitive instruction traps:

1. **[[Dispatcher|Dispatcher]]** receives control from trap
2. Identifies instruction as control sensitive
3. Invokes **[[Allocator|allocator]]** component
4. Allocator validates resource request:
    - Checks if resources available
    - Verifies request doesn't violate isolation
    - Updates resource tables if approved
5. Simulates instruction effect or denies request
6. Returns control to VM

---

## Problematic Architectures

### DEC PDP-10

The `JRST 1` instruction:

- Is control sensitive (changes mode from supervisor to user)
- Does not trap in supervisor mode
- Is therefore not privileged
- Makes PDP-10 non-virtualizable without modification

### IBM System/360 (Early Models)

Some early models had instructions that could:

- Modify storage protection without trapping
- Read real addresses without proper protection

---

## Contrast with Behavior Sensitivity

|Control Sensitive|Behavior Sensitive|
|---|---|
|Changes resources/mode|Behavior depends on configuration|
|Affects allocation|Affects execution result|
|Requires allocator|Requires interpretation|
|Example: Load relocation register|Example: Load real address|

---

## Related Concepts

- [[Sensitive Instruction]]: Parent category
- [[Behavior Sensitive Instruction]]: Other type of sensitive instruction
- [[Privileged Instruction]]: Must include all control sensitive instructions
- [[Resource Control Property]]: Violated without proper control sensitivity handling
- [[Allocator]]: VMM component that handles control sensitive instructions
- [[Popek-Goldberg Virtualization Theorem]]: Requires control sensitive ⊆ privileged