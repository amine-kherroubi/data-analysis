# Hybrid Virtual Machine

A **hybrid virtual machine** (HVM) is a relaxed form of virtualization that allows more instructions to be interpreted rather than executed directly, enabling virtualization on architectures that violate the [[Popek-Goldberg Virtualization Theorem|Popek-Goldberg theorem]].

---

## Definition

A hybrid virtual machine monitor is a [[Virtual Machine Monitor|VMM]] that interprets **all instructions executed in virtual supervisor mode**, while still allowing direct execution of user-mode instructions. This approach is less efficient than a pure VMM but more broadly applicable.

---

## Motivation

Many third-generation architectures are not classically virtualizable because they contain [[Sensitive Instruction|sensitive instructions]] that are not [[Privileged Instruction|privileged]]. Hybrid virtualization provides a solution.

### The Problem

For an architecture to support a pure VMM, the [[Popek-Goldberg Virtualization Theorem|Popek-Goldberg theorem]] requires:

**Sensitive ⊆ Privileged**

When this condition is violated:

- Some sensitive instructions don't trap
- VMM cannot intercept them
- Classical virtualization fails

### The Hybrid Solution

Instead of requiring all sensitive instructions to trap, hybrid virtualization:

- **Interprets all supervisor-mode code**
- **Directly executes all user-mode code**
- Guarantees control over sensitive operations
- Works even when sensitive ⊈ privileged

---

## User-Sensitive vs. Supervisor-Sensitive Instructions

Hybrid virtualization distinguishes between two subsets of sensitive instructions:

### User-Sensitive Instructions

An instruction $i$ is **user-sensitive** if there exists a state $S = (E, u, P, R)$ (note: mode = user) for which $i$ is [[Control Sensitive Instruction|control sensitive]] or [[Behavior Sensitive Instruction|behavior sensitive]].

**Characteristics:**

- Sensitive when executed in user mode
- Must be privileged for hybrid virtualization
- Relatively rare in guest kernels
- Critical for user-level program correctness

### Supervisor-Sensitive Instructions

An instruction $i$ is **supervisor-sensitive** if there exists a state $S = (E, s, P, R)$ (note: mode = supervisor) for which $i$ is control sensitive or behavior sensitive.

**Characteristics:**

- Sensitive when executed in supervisor mode
- Need not be privileged
- Common in guest kernels
- Handled through interpretation in HVM

---

## Hybrid Virtual Machine Theorem

**A hybrid virtual machine monitor may be constructed for any conventional third generation machine in which the set of user-sensitive instructions is a subset of the set of privileged instructions.**

Formally:

$$\text{HVM possible} \iff \text{User-Sensitive} \subseteq \text{Privileged}$$

### Comparison to Pure VMM Theorem

|Requirement|Pure VMM|Hybrid VMM|
|---|---|---|
|User-sensitive ⊆ Privileged|Yes|Yes|
|Supervisor-sensitive ⊆ Privileged|Yes|**No**|
|Interpretation scope|Only trapped instructions|All supervisor-mode code|

---

## Architecture: How HVM Works

### Execution Modes

**Virtual User Mode:**

- Guest application code
- Executes directly on hardware
- Near-native performance
- [[Innocuous Instruction|Innocuous]] and user-mode instructions run at full speed

**Virtual Supervisor Mode:**

- Guest kernel/OS code
- **Completely interpreted**
- Every instruction fetched and interpreted by HVM
- Significantly slower than direct execution

### Control Flow

```
Guest User Code:
  - Executes directly
  - Fast, efficient
  - Traps on user-sensitive instructions
  ↓
  Trap to HVM
  ↓
Guest Supervisor Code:
  - Completely interpreted
  - HVM fetches each instruction
  - HVM simulates effect
  - Slower but guaranteed correct
```

---

## Example: DEC PDP-10

The PDP-10 demonstrates why hybrid virtualization is needed:

### The Problem Instruction

```
JRST 1  // Jump and Restore, mode bit 1
```

**Behavior:**

- Returns from supervisor mode to user mode
- Is [[Control Sensitive Instruction|control sensitive]] (changes processor mode)
- Is NOT privileged in supervisor mode
- Does not trap when executed in supervisor mode
- Violates Popek-Goldberg theorem

### Why Pure VMM Fails

If a pure VMM tried to run PDP-10 guest kernel:

1. Guest kernel executes `JRST 1` in virtual supervisor mode
2. Instruction executes directly (no trap)
3. Actually changes real mode to user
4. VMM loses control
5. Virtualization broken

### How HVM Succeeds

With hybrid virtualization:

1. Guest kernel runs in interpretation mode
2. HVM fetches `JRST 1` instruction
3. HVM simulates mode change in virtual state
4. HVM maintains control
5. Virtualization works correctly

---

## Performance Characteristics

### Direct Execution (User Mode)

- **Performance:** Near-native (98-99%)
- **Overhead:** Minimal, only traps
- **Frequency:** 90%+ of instructions
- Maintains [[Efficiency Property|efficiency]] for applications

### Interpretation (Supervisor Mode)

- **Performance:** Significantly degraded (10-50% of native)
- **Overhead:** Instruction fetch, decode, simulate per instruction
- **Frequency:** 10% or less of total instructions
- Acceptable for guest OS code

### Overall Impact

Since guest kernels execute far fewer instructions than applications:

- Typical workloads: 80-90% of native performance
- Compute-intensive: 85-95% of native performance
- I/O-intensive: 70-85% of native performance

---

## Guarantees

Hybrid virtualization ensures:

### Equivalence

- User-level programs execute identically
- Supervisor-level programs produce correct effects (though slowly)
- All [[Sensitive Instruction|sensitive instructions]] caught and handled

### Resource Control

- HVM interposes on all supervisor operations
- User-sensitive instructions trap
- Supervisor code interpreted completely
- [[Resource Control Property|Resource control]] maintained

### Efficiency (Modified)

- User-mode code executes at full speed
- Supervisor-mode code interpreted
- Still qualifies as "statistically dominant subset" direct execution
- Most CPU time spent in user mode

---

## Implementation

### HVM Monitor Components

**[[Dispatcher]]:**

- Receives control on traps
- Switches between interpretation and direct execution
- Manages mode transitions

**[[Allocator]]:**

- Manages resources
- Validates allocation requests
- Maintains isolation

**Interpreter:**

- **Enhanced for supervisor mode**
- Fetches and decodes all supervisor instructions
- Simulates effects
- Maintains virtual machine state

**Instruction Fetch-Decode-Execute Loop:**

```
while (virtual_mode == supervisor) {
    instruction = fetch_next_instruction(virtual_PC);
    decode(instruction);
    simulate_effect(instruction, virtual_state);
    update_virtual_PC();
}
```

---

## Practical Applications

### DEC PDP-10

- Cannot support pure VMM
- Can support HVM
- User programs run efficiently
- Operating system code interpreted

### Early x86 (Pre-VT-x)

Before hardware virtualization:

- Multiple sensitive instructions not privileged
- `POPF`, `PUSHF`, `SGDT`, others problematic
- Binary translation similar to HVM approach
- VMware used sophisticated techniques

### Other Examples

Any architecture where:

- User-sensitive ⊆ Privileged
- Supervisor-sensitive ⊈ Privileged
- Can use hybrid virtualization

---

## Advantages Over Pure VMM

### Broader Applicability

- Works on more architectures
- Doesn't require full Popek-Goldberg compliance
- Practical solution for real hardware

### Guaranteed Correctness

- All supervisor operations controlled
- No sensitive instruction can escape
- Strong security guarantees

---

## Disadvantages

### Performance

- Supervisor code significantly slower
- OS-intensive workloads affected more
- Not ideal for OS development/testing
- Context switch overhead higher

### Complexity

- More complex than pure VMM
- Instruction interpreter required
- State management more intricate
- Larger trusted computing base

---

## Comparison Table

|Feature|Pure VMM|Hybrid VMM|[[Full Virtualization]]|
|---|---|---|---|
|User code execution|Direct|Direct|Direct (or binary translation)|
|Supervisor code|Direct (traps only)|Interpreted|Binary translation|
|Architecture requirement|Sensitive ⊆ Privileged|User-Sensitive ⊆ Privileged|None (software handles)|
|Performance|Best|Good|Moderate|
|Applicability|Limited|Moderate|Broad|

---

## Modern Relevance

### Historical Importance

- Demonstrated virtualization possible on non-virtualizable hardware
- Proved commercial viability
- Led to [[Hardware Virtualization|hardware virtualization]] features

### Current Use

- Mostly superseded by hardware virtualization
- Conceptually similar to binary translation
- Educational value for understanding virtualization
- Foundation for modern techniques

---

## Related Concepts

- [[Virtual Machine Monitor]]: Pure VMM vs. hybrid VMM
- [[Popek-Goldberg Virtualization Theorem]]: Defines when pure VMM possible
- [[Sensitive Instruction]]: Instructions requiring special handling
- [[Privileged Instruction]]: Instructions that trap
- [[Full Virtualization]]: Alternative approach using binary translation
- [[Hardware Virtualization]]: Modern solution to virtualizability