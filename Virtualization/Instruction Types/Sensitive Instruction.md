# Sensitive Instruction

A **sensitive instruction** is an instruction that either affects system resource allocation or whose behavior depends on system configuration. Sensitive instructions are critical to virtualization because they require [[Virtual Machine Monitor|VMM]] intervention.

---

## Definition

An instruction $i$ is sensitive if it is either:

1. **[[Control Sensitive Instruction|Control sensitive]]**, or
2. **[[Behavior Sensitive Instruction|Behavior sensitive]]**

---

## Two Types of Sensitivity

### Control Sensitive

Instructions that attempt to change:

- Amount of memory resources available (relocation-bounds register)
- Processor mode (supervisor/user)
- System configuration affecting resource allocation

### Behavior Sensitive

Instructions whose execution behavior or result depends on:

- Current processor mode
- Location in real memory (relocation value)
- System configuration state

---

## Formal Characterization

### Control Sensitivity

Instruction $i$ is control sensitive if there exists state $S_1 = (e_1, m_1, p_1, r_1)$ where:

$i(S_1) = S_2 = (e_2, m_2, p_2, r_2)$

such that $i(S_1)$ does not memory trap, and either:

- $r_1 \neq r_2$ (relocation-bounds changed), or
- $m_1 \neq m_2$ (mode changed), or both

### Behavior Sensitivity

Instruction $i$ is behavior sensitive if there exist integer $x$ and states:

- $S_1 = (e | r, m, p, r)$
- $S_2 = (e | r \oplus x, m, p, r \oplus x)$

where $r \oplus x$ means relocation shifted by $x$, such that:

- $i(S_1) = (e_1 | r, m_1, p_1, r)$
- $i(S_2) = (e_2 | r \oplus x, m_2, p_2, r \oplus x)$

and either:

- $e_1 | r \neq e_2 | r \oplus x$ (different memory effects), or
- $p_1 \neq p_2$ (different program counter), or both

---

## Importance for Virtualization

According to the [[Popek-Goldberg Virtualization Theorem|Popek-Goldberg theorem]]:

**A machine is virtualizable if and only if all sensitive instructions are [[Privileged Instruction|privileged instructions]].**

This ensures:

- VMM can intercept all operations that affect resources
- VMM can intercept all operations whose behavior depends on configuration
- [[Resource Control Property|Resource control]] and [[Equivalence Property|equivalence]] are guaranteed

---

## Examples

### Control Sensitive Instructions

```
LPSW        // Load PSW (changes mode/memory) - IBM 360
LBAR        // Load bounds-address register - Honeywell 6000
JRST 1      // Return to user mode - DEC PDP-10
```

### Behavior Sensitive Instructions

**Location Sensitive:**

```
LRA         // Load Real Address - IBM 360/67
            // Returns physical address (depends on relocation)
```

**Mode Sensitive:**

```
MFPI        // Move From Previous Instruction space - PDP-11/45
            // Behavior depends on current processor mode
```

---

## Problematic Case: Non-Privileged Sensitive Instructions

Some architectures contain sensitive instructions that are **not** privileged:

**Example**: DEC PDP-10

- `JRST 1` is control sensitive (changes mode)
- Does not trap when executed in supervisor mode
- Violates virtualization requirement
- Makes PDP-10 non-virtualizable by standard VMM

Such architectures require [[Hybrid Virtual Machine|hybrid virtualization]] approaches.

---

## Complement: Innocuous Instructions

Instructions that are **not** sensitive are called [[Innocuous Instruction|innocuous instructions]]. These:

- Do not affect resource allocation
- Have behavior independent of configuration
- Can execute directly without VMM intervention
- Form the majority of instructions in typical programs

---

## Relationship to VMM Components

Sensitive instructions require VMM intervention:

- [[Dispatcher|Dispatcher]] receives control when they trap
- [[Allocator|Allocator]] handles control sensitive instructions
- [[Interpreter Routines|Interpreter routines]] simulate their effects
- Must execute correctly to maintain [[Equivalence Property|equivalence]]

---

## Related Concepts

- [[Control Sensitive Instruction]]: First type of sensitive instruction
- [[Behavior Sensitive Instruction]]: Second type of sensitive instruction
- [[Privileged Instruction]]: Must include all sensitive instructions for virtualizability
- [[Innocuous Instruction]]: Instructions that are not sensitive
- [[Popek-Goldberg Virtualization Theorem]]: Requires sensitive ⊆ privileged
- [[Virtual Machine Monitor]]: Must handle all sensitive instructions