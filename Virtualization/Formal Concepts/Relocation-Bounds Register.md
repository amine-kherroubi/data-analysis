# Relocation-Bounds Register

The **relocation-bounds register** is a hardware mechanism for memory management and protection that enables dynamic memory allocation and isolation in third-generation computer systems. It is fundamental to the Popek-Goldberg virtualization model.

---

## Structure

The relocation-bounds register consists of two components:

$R = (l, b)$

where:

- $l$ = **relocation** (base): absolute physical address corresponding to logical address 0
- $b$ = **bounds** (limit): size of the allocated virtual memory region (not the largest valid address)

---

## Address Translation

When a program generates logical address $a$, the hardware performs address development:

```
if a + l >= q then
    memory_trap  // Exceeds physical memory
else if a >= b then
    memory_trap  // Exceeds allocated bounds
else
    use physical_memory[a + l]  // Valid access
```

where $q$ is the total physical memory size.

---

## Properties

### 1. Always Active

In the Popek-Goldberg model, the relocation-bounds register is active in both supervisor and user modes (a simplification from some real systems where it may only be active in user mode).

### 2. Provides Isolation

- Programs can only access memory within their allocated region: $[l, l+b)$
- Accesses outside this region cause [[Trap Mechanism|memory traps]]
- [[Virtual Machine Monitor|VMM]] memory is protected from [[Virtual Machine|VM]] access

### 3. Enables Relocation

Programs can be loaded anywhere in physical memory:

- Logical addresses remain unchanged in program
- Relocation value $l$ maps to physical location
- Program unaware of physical placement

---

## Virtual Machine Usage

### Single VM Configuration

When a VMM hosts a single VM:

- VMM occupies locations 0 to $k-1$
- VM occupies locations $k$ to $k+w-1$
- VM's relocation-bounds: $R = (k, w)$
- Full physical memory: $(0, q)$

### Multiple VM Configuration

For multiple concurrent VMs:

- Each VM has distinct relocation-bounds values
- [[Allocator|Allocator]] ensures non-overlapping regions
- VMM maintains table of VM allocations

---

## Modification Control

The relocation-bounds register is a [[Control Sensitive Instruction|control sensitive]] resource:

### User Mode

Attempts to modify $R$ in user mode must trap:

```
if mode = user and instruction_modifies_R then
    privileged_instruction_trap
    // VMM dispatcher gains control
```

### Supervisor Mode

VMM can freely modify $R$:

- Allocate memory to VMs
- Change VM memory boundaries
- Relocate VM in physical memory

---

## Full Memory Access

To access all of physical memory (typically for the VMM):

$R = (0, q-1)$

where:

- $l = 0$ (no relocation)
- $b = q-1$ (entire memory accessible)

This configuration is loaded from location 1 when a [[Trap Mechanism|trap]] occurs.

---

## Examples in Real Systems

### IBM System/360

- Base-limit registers
- Multiple register pairs for segmentation
- Active in problem state (user mode)

### Honeywell 6000

- Base Address Register (BAR)
- Limit Register
- Combined into single mechanism (LBAR instruction)

### DEC PDP-10

- Address relocation and protection
- Different mechanism but similar concept

---

## Behavior Sensitivity Source

The relocation register is the primary source of [[Behavior Sensitive Instruction|behavior sensitivity]]:

Instructions that read or depend on the relocation value $l$:

- Load Real Address (IBM 360/67 LRA)
- Physical address computation instructions
- Position-dependent operations

These instructions violate [[Equivalence Property|equivalence]] if not properly trapped and simulated.

---

## Mathematical Notation

### Relocation Shift Operator

$R \oplus x$ denotes shifting the relocation value:

If $R = (l, b)$ then $R \oplus x = (l+x, b)$

This notation is used in the formal definition of [[Behavior Sensitive Instruction|behavior sensitivity]].

### Memory Slice Notation

$E | R$ denotes the portion of memory accessible with register $R = (l, b)$:

$E | R = {E[l], E[l+1], ..., E[l+b-1]}$

This represents the virtual machine's view of memory.

---

## Virtual Machine Map

The relocation-bounds register is transformed by the [[Virtual Machine Map|VM map]] $f$:

$f(E, M, P, (l, b)) = (E', M', P', (l+k, b))$

where $k$ is the VMM size, shifting the VM's memory region.

---

## Related Concepts

- [[Virtual Machine Monitor]]: Uses relocation-bounds for memory management
- [[Allocator]]: Manages relocation-bounds values for VMs
- [[Control Sensitive Instruction]]: Instructions that modify relocation-bounds
- [[Behavior Sensitive Instruction]]: Instructions whose behavior depends on relocation
- [[Trap Mechanism]]: Triggered by bounds violations
- [[Resource Control Property]]: Enforced through relocation-bounds mechanism