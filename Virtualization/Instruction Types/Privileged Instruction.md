# Privileged Instruction

A **privileged instruction** is an instruction that can only execute when the processor is in supervisor mode. When attempted in user mode, it causes a trap, transferring control to the [[Virtual Machine Monitor|VMM]] or operating system.

---

## Formal Definition

Instruction $i$ is privileged if and only if for any pair of states:

- $S_1 = (e, s, p, r)$ (supervisor mode)
- $S_2 = (e, u, p, r)$ (user mode)

where $i(S_1)$ and $i(S_2)$ do not memory trap:

$i(S_2)$ traps and $i(S_1)$ does not

The states differ only in processor mode: $S_1$ has mode supervisor, $S_2$ has mode user.

---

## Key Characteristics

### 1. Mode-Dependent Trapping

- Executes successfully in supervisor mode
- Always traps when attempted in user mode
- Trap is called a "privileged instruction trap"

### 2. Complete Trapping Required

Simply preventing execution (NOPing) without trapping is insufficient. The instruction must:

- Trap to VMM/OS
- Allow intervention and simulation
- Not just silently fail

---

## Hardware Mechanism

Privileged instructions rely on hardware support:

1. Processor maintains current mode (supervisor/user bit)
2. Each instruction has mode requirements
3. Instruction decode logic checks mode
4. Mode violation triggers privileged instruction trap
5. Trap mechanism saves state and transfers control

---

## Common Examples

### Memory Management

```
LPSW    // Load Program Status Word (IBM System/360)
LBAR    // Load Base and Address Register (Honeywell 6000)
DATAO   // Data Output to APR (DEC PDP-10)
```

### Mode Control

```
Change to supervisor mode
Modify processor state
Set interrupt masks
```

### I/O Operations

```
Start I/O channel
Test I/O status
Control peripheral devices
```

---

## Distinction from Other Instruction Types

|Instruction Type|User Mode Behavior|Supervisor Mode Behavior|
|---|---|---|
|Privileged|Always traps|Executes normally|
|[[Sensitive Instruction\|Sensitive]]|May trap or behave differently|May trap or behave differently|
|[[Innocuous Instruction\|Innocuous]]|Executes normally|Executes normally|

---

## Importance for Virtualization

Privileged instructions are crucial for the [[Popek-Goldberg Virtualization Theorem|Popek-Goldberg theorem]]:

**For a virtualizable architecture, all [[Sensitive Instruction|sensitive instructions]] must be privileged.**

This ensures that:

- VMM can intercept operations affecting resources
- VMM can simulate privileged operations correctly
- [[Efficiency Property|Efficiency]] is maintained (only trapping when necessary)
- [[Resource Control Property|Resource control]] is guaranteed

---

## Non-Privileged Instructions

Instructions that are **not** privileged:

- Execute identically in both supervisor and user modes
- Do not require mode checking
- Include most arithmetic, logical, and data movement operations
- May still be [[Sensitive Instruction|sensitive]] (problematic for virtualization)

---

## Problematic Cases

Some architectures have [[Sensitive Instruction|sensitive instructions]] that are **not** privileged:

**Example**: DEC PDP-10 `JRST 1` (return to user mode)

- Is [[Control Sensitive Instruction|control sensitive]] (changes processor mode)
- Does not trap in supervisor mode
- Violates virtualization requirements
- Makes PDP-10 non-virtualizable

---

## Related Concepts

- [[Sensitive Instruction]]: Instructions affecting resources or configuration
- [[Control Sensitive Instruction]]: Instructions modifying resources or mode
- [[Behavior Sensitive Instruction]]: Instructions whose behavior depends on configuration
- [[Innocuous Instruction]]: Instructions that are not sensitive
- [[Trap Mechanism]]: Hardware mechanism for privileged instruction handling
- [[Popek-Goldberg Virtualization Theorem]]: Requires sensitive ⊆ privileged