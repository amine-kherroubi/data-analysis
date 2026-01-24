# Popek-Goldberg Virtualization Theorem

The **Popek-Goldberg virtualization theorem** (also called **Theorem 1**) provides a formal, testable condition for determining whether a computer architecture can support a [[Virtual Machine Monitor|virtual machine monitor]]. It was published by Gerald J. Popek and Robert P. Goldberg in 1974.

---

## Theorem Statement

**For any conventional third generation computer, a virtual machine monitor may be constructed if and only if the set of [[Sensitive Instruction|sensitive instructions]] for that computer is a subset of the set of [[Privileged Instruction|privileged instructions]].**

Formally:

$$\text{VMM exists} \iff \text{Sensitive} \subseteq \text{Privileged}$$

---

## Interpretation

### Sufficient Condition

If all sensitive instructions are privileged, then:

- A VMM can be constructed for the architecture
- The VMM will satisfy all three essential properties
- Virtualization is possible

### Necessary Condition (with caveats)

While the theorem provides sufficiency, necessity is more nuanced:

- Some architectures violating the condition can still support limited virtualization
- Special hardware support may enable virtualization despite violations
- [[Hybrid Virtual Machine|Hybrid VMMs]] may work when pure VMMs cannot

---

## What Makes a Machine "Conventional"

A conventional third generation computer must have:

### 1. Dual-Mode Operation

- Supervisor mode (privileged mode)
- User mode (unprivileged mode)
- Mode-based instruction protection

### 2. Memory Protection

- [[Relocation-Bounds Register|Relocation-bounds]] or equivalent mechanism
- Address translation hardware
- Bounds checking with [[Trap Mechanism|trap]] on violation

### 3. Trap Mechanism

- Automatic state save
- Control transfer to handler
- Deterministic trap conditions

### 4. General-Purpose Instruction Set

- Sufficient to implement [[Dispatcher|dispatcher]]
- Sufficient to implement [[Allocator|allocator]]
- Sufficient to implement table lookup (for [[Interpreter Routines|interpreter routines]])

---

## Why This Condition Works

The theorem ensures the three VMM properties:

### 1. Resource Control

All [[Control Sensitive Instruction|control sensitive instructions]] are privileged:

- Trap to VMM when attempted in user mode
- [[Allocator|Allocator]] can validate resource requests
- VMM maintains complete control
- [[Resource Control Property|Resource control]] guaranteed

### 2. Equivalence

All [[Behavior Sensitive Instruction|behavior sensitive instructions]] are privileged:

- Trap to VMM when attempted in user mode
- [[Interpreter Routines|Interpreter routines]] simulate correct behavior
- Mode and location dependencies handled correctly
- [[Equivalence Property|Equivalence]] guaranteed

### 3. Efficiency

[[Innocuous Instruction|Innocuous instructions]] are not privileged:

- Execute directly without trapping
- No VMM intervention needed
- Statistically dominant subset executes at full speed
- [[Efficiency Property|Efficiency]] guaranteed

---

## Proof Sketch

### Construction

Given an architecture where sensitive ⊆ privileged, construct a VMM:

1. **Build [[Dispatcher|dispatcher]]**: Entry point at trap location
2. **Build [[Allocator|allocator]]**: Resource management component
3. **Build [[Interpreter Routines|interpreter routines]]**: One per [[Privileged Instruction|privileged instruction]]

### Verification

Prove the VMM satisfies the three properties:

**Efficiency**: Innocuous instructions execute directly (trivial - they don't trap)

**Resource Control**: Control sensitive instructions trap (by subset assumption) and are handled by allocator

**Equivalence**: For any instruction sequence:

- Innocuous instructions: Execute identically (Lemma 1)
- Sensitive instructions: Correctly simulated (Lemma 2)
- Instruction sequences: Composition preserves equivalence (Lemma 3)

---

## Virtualizable Architectures

### Examples That Satisfy the Theorem

**IBM System/360 (with modifications)**

- All sensitive instructions are privileged
- Proper trap mechanisms
- Can host CP/CMS, VM/370

**IBM System/370 and successors**

- Designed with virtualization in mind
- Clean separation of privileged and sensitive
- Successful VM implementations

### Examples That Violate the Theorem

**DEC PDP-10**

- `JRST 1` is control sensitive but not privileged in supervisor mode
- Cannot support pure VMM
- Requires [[Hybrid Virtual Machine|hybrid virtualization]]

**Intel x86 (pre-VT-x)**

- Multiple sensitive instructions not privileged
- Some instructions fail silently rather than trapping
- Paravirtualization or binary translation required

**Motorola 68000**

- MOVE from SR (status register) is sensitive but not privileged
- Returns different values in different modes without trapping
- Problematic for pure virtualization

---

## Design Implications

The theorem provides guidance for architecture design:

### For Virtualizable Architectures

Ensure that every instruction that:

- Modifies processor mode → is privileged
- Modifies memory allocation → is privileged
- Depends on processor mode → is privileged
- Depends on memory location → is privileged

### Testing Procedure

To verify an architecture:

1. List all sensitive instructions
2. List all privileged instructions
3. Check if sensitive ⊆ privileged
4. If yes, VMM is possible; if no, VMM is not possible (without special techniques)

---

## Limitations and Extensions

### Not Covered by Basic Theorem

- I/O operations (can be added as extension)
- Asynchronous interrupts (requires additional analysis)
- Multi-processor systems (more complex)
- Modern virtualization extensions (VMX, SVM)

### Modern Relevance

Despite being 50 years old:

- Core principles remain valid
- Guides modern processor design (Intel VT-x, AMD-V)
- Forms theoretical foundation for cloud computing
- Influences container and unikernel designs

---

## Related Concepts

- [[Popek-Goldberg Recursion Theorem|Theorem 2]]: Recursive virtualization
- [[Sensitive Instruction]]: Instructions requiring VMM intervention
- [[Privileged Instruction]]: Instructions that trap in user mode
- [[Virtual Machine Monitor]]: Software proven constructible by theorem
- [[Hybrid Virtual Machine]]: Alternative when theorem violated
- [[Hardware Virtualization]]: Modern extensions enhancing virtualizability