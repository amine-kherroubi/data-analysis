# Trap Mechanism

A **trap** is a hardware mechanism that automatically saves the current machine state and transfers control to a predefined handler routine when certain conditions occur. Traps are fundamental to [[Virtual Machine Monitor|virtual machine monitors]] and operating systems.

---

## Formal Definition

An instruction $i$ is said to trap if:

$i(E_1, M_1, P_1, R_1) = (E_2, M_2, P_2, R_2)$

where:

- $E_2[j] = E_1[j]$ for all $0 \leq j < q$ (memory unchanged except locations 0 and 1)
- $E_2[0] = (M_1, P_1, R_1)$ (old PSW saved in location 0)
- $(M_2, P_2, R_2) = E_1[1]$ (new PSW loaded from location 1)

---

## Trap Sequence

When a trap occurs:

### 1. Save Current State

The processor status word (PSW) is saved to a predetermined location:

- Current mode ($M$)
- Program counter ($P$)
- Relocation-bounds register ($R$)

In the Popek-Goldberg model, the PSW is saved to memory location 0.

### 2. Load Handler State

A new PSW is loaded from a predetermined location to transfer control:

- New mode (typically supervisor)
- New program counter (address of handler)
- New relocation-bounds (typically 0, $q-1$ for full memory access)

In the Popek-Goldberg model, the new PSW is loaded from memory location 1.

### 3. Transfer Control

Execution continues at the address specified in the new program counter, typically the [[Dispatcher|dispatcher]] of the VMM or OS.

---

## Types of Traps

### Privileged Instruction Trap

Occurs when a [[Privileged Instruction|privileged instruction]] is attempted in user mode:

```
if instruction_is_privileged and mode = user then
    save_state_to_location_0
    load_state_from_location_1  
    // Control transferred to VMM dispatcher
```

### Memory Trap

Occurs when a memory access violates bounds:

```
if address + relocation >= bounds then
    trap
if address + relocation >= physical_memory_size then
    trap
```

### Other Traps (Extended Model)

- I/O operation traps
- Interrupt handling
- Exception conditions (overflow, divide-by-zero)
- Protection violations

---

## Importance for Virtualization

Traps are essential for VMM operation:

### 1. Interception

Traps allow the VMM to intercept [[Sensitive Instruction|sensitive instructions]]:

- [[Control Sensitive Instruction|Control sensitive]] instructions trap
- [[Behavior Sensitive Instruction|Behavior sensitive]] instructions trap
- VMM can validate and simulate their effects

### 2. Resource Control

Traps enforce the [[Resource Control Property|resource control property]]:

- Memory access violations trap to VMM
- Unauthorized resource access attempts trap
- VMM maintains complete control

### 3. Transparency

Traps enable transparent virtualization:

- Programs unaware of VMM presence
- Trap-and-emulate preserves program semantics
- [[Equivalence Property|Equivalence]] maintained through correct simulation

---

## Performance Implications

Trap overhead affects virtualization performance:

### Trap Cost

Each trap involves:

- State save operation
- Mode transition
- Cache/pipeline flush (on some architectures)
- Handler execution
- State restore
- Return transition

### Frequency Matters

- [[Innocuous Instruction|Innocuous instructions]] don't trap (efficient)
- Only [[Privileged Instruction|privileged instructions]] trap
- Trap frequency typically 5-10% of instructions
- Acceptable overhead for [[Efficiency Property|efficiency property]]

---

## Hardware Support

### Traditional Mechanisms

- Dedicated trap vectors
- PSW save/restore locations
- Mode transition logic
- Protection checking hardware

### Modern Extensions

[[Hardware Virtualization|Hardware virtualization]] reduces trap overhead:

- Intel VT-x: Virtual Machine Control Structure (VMCS)
- AMD-V: Virtual Machine Control Block (VMCB)
- Faster trap/return mechanisms
- Reduced overhead per trap

---

## Trap vs. Interrupt

|Trap|Interrupt|
|---|---|
|Synchronous|Asynchronous|
|Caused by instruction execution|Caused by external event|
|Deterministic|Non-deterministic timing|
|Examples: privileged instruction, bounds violation|Examples: timer, I/O completion|

---

## VMM Trap Handler Structure

When a trap occurs in a VMM system:

1. **[[Dispatcher|Dispatcher]] receives control**
    - Examines saved state in location 0
    - Determines trap cause
2. **Appropriate component invoked**
    - [[Allocator|Allocator]] for resource requests
    - [[Interpreter Routines|Interpreter routine]] for instruction simulation
3. **Simulation performed**
    - Effect of instruction computed
    - Virtual machine state updated
4. **Control returned**
    - Virtual machine PSW loaded
    - Execution resumes in VM

---

## Relaxed Definition

The Popek-Goldberg definition can be relaxed to include cases where:

- Trap occurs after instruction completion (rather than blocking it)
- State is reversible to point before instruction
- VMM can still correctly simulate instruction effect

This flexibility accommodates various hardware trap mechanisms.

---

## Related Concepts

- [[Privileged Instruction]]: Instructions that trap in user mode
- [[Sensitive Instruction]]: Instructions that should trap for virtualization
- [[Dispatcher]]: VMM component that receives trap control
- [[Virtual Machine Monitor]]: Uses traps to maintain control
- [[Resource Control Property]]: Enforced through trap mechanism
- [[Hardware Virtualization]]: Enhanced trap mechanisms