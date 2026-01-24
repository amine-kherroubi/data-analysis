# Behavior Sensitive Instruction

A **behavior sensitive instruction** is a type of [[Sensitive Instruction|sensitive instruction]] whose execution behavior or result depends on the processor mode or the instruction's location in real memory (as determined by the relocation register).

---

## Formal Definition

Instruction $i$ is behavior sensitive if there exist an integer $x$ and states:

- $S_1 = (e | r, m, p, r)$
- $S_2 = (e | r \oplus x, m, p, r \oplus x)$

where $r \oplus x$ denotes the relocation register shifted by $x$: if $r = (l, b)$ then $r \oplus x = (l+x, b)$

such that:

- $i(S_1) = (e_1 | r, m_1, p_1, r)$
- $i(S_2) = (e_2 | r \oplus x, m_2, p_2, r \oplus x)$

where neither $i(S_1)$ nor $i(S_2)$ memory trap, and either:

1. $e_1 | r \neq e_2 | r \oplus x$ (different memory effects), or
2. $p_1 \neq p_2$ (different program counter values)

---

## Intuitive Explanation

An instruction is behavior sensitive if its execution behavior changes when:

- The program is relocated to a different memory location, or
- The processor mode changes

This violates [[Equivalence Property|equivalence]] because identical programs at different memory locations or in different modes would produce different results.

---

## Two Subtypes

### Location Sensitive

The instruction's behavior depends on its physical memory location (relocation value).

**Examples:**

- Instructions that return physical addresses
- Instructions that reference absolute memory locations
- Instructions that depend on memory-mapped I/O locations

### Mode Sensitive

The instruction's behavior depends on the current processor mode (supervisor vs. user).

**Examples:**

- Instructions that access different memory based on mode
- Instructions with mode-dependent addressing
- Instructions that read mode-dependent registers

---

## Concrete Examples

### Location Sensitive Instructions

**IBM System/360-67 LRA (Load Real Address):**

```
LRA R1, address
// Loads the physical address corresponding to virtual address
// Result depends on current relocation register value
// Same instruction at different locations yields different results
```

**DEC PDP-10 Physical Address Instructions:**

```
// Instructions that return or use physical addresses
// Result depends on where program is loaded in memory
```

---

### Mode Sensitive Instructions

**DEC PDP-11/45 MFPI (Move From Previous Instruction space):**

```
MFPI address
// Forms effective address using mode-dependent information
// In supervisor mode, accesses supervisor space
// In user mode, accesses previous mode's space
// Same instruction, different behavior based on mode
```

---

## Why Behavior Sensitivity Is Problematic

Behavior sensitive instructions threaten [[Equivalence Property|equivalence]]:

1. **Location Dependency**: If a program's behavior depends on where it's loaded in physical memory, moving it (e.g., for memory management) changes its results
2. **Mode Dependency**: If instruction behavior depends on mode, and the [[Virtual Machine Monitor|VMM]] simulates mode differently, programs may behave incorrectly
3. **No Automatic Trap**: Unlike [[Control Sensitive Instruction|control sensitive instructions]], behavior sensitive instructions may not trap—they simply produce wrong results

---

## Importance for Virtualization

According to the [[Popek-Goldberg Virtualization Theorem|Popek-Goldberg theorem]]:

**All behavior sensitive instructions must be [[Privileged Instruction|privileged]]** for an architecture to be virtualizable.

If behavior sensitive instructions are not privileged:

- They execute directly without VMM intervention
- VMM cannot correct their mode- or location-dependent behavior
- [[Equivalence Property|Equivalence]] is violated
- Architecture requires [[Hybrid Virtual Machine|hybrid virtualization]]

---

## VMM Handling

When a behavior sensitive instruction traps:

1. **[[Dispatcher|Dispatcher]]** receives control
2. Determines instruction is behavior sensitive
3. Invokes appropriate **[[Interpreter Routines|interpreter routine]]**
4. Interpreter simulates correct behavior:
    - Uses simulated mode instead of real mode
    - Uses virtual address instead of physical address
    - Produces result consistent with virtual machine state
5. Updates virtual machine state
6. Returns control to VM

---

## Special Considerations

### Relocation Independence

The Popek-Goldberg definition excludes behavior sensitivity in supervisor mode from the virtualization requirement in some interpretations:

- Virtual machines run in user mode
- Supervisor mode behavior sensitivity doesn't affect VM code
- Only user-mode behavior sensitivity must be privileged

### Memory Mapping

In systems with complex memory mapping:

- More instructions may be behavior sensitive
- Hardware virtualization support becomes more important
- Software techniques may be insufficient

---

## Contrast with Control Sensitivity

|Behavior Sensitive|Control Sensitive|
|---|---|
|Result depends on configuration|Changes configuration|
|Violates equivalence|Violates resource control|
|Example: Read physical address|Example: Modify relocation register|
|Requires interpreter routine|Requires allocator|

---

## Related Concepts

- [[Sensitive Instruction]]: Parent category
- [[Control Sensitive Instruction]]: Other type of sensitive instruction
- [[Privileged Instruction]]: Must include all behavior sensitive instructions
- [[Equivalence Property]]: Violated without proper behavior sensitivity handling
- [[Interpreter Routines]]: VMM component that handles behavior sensitive instructions
- [[Relocation-Bounds Register]]: Source of location dependency
- [[Popek-Goldberg Virtualization Theorem]]: Requires behavior sensitive ⊆ privileged