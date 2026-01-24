# Innocuous Instruction

An **innocuous instruction** is an instruction that is neither [[Control Sensitive Instruction|control sensitive]] nor [[Behavior Sensitive Instruction|behavior sensitive]]. These instructions form the vast majority of instructions executed in typical programs.

---

## Definition

An instruction $i$ is innocuous if it is **not** a [[Sensitive Instruction|sensitive instruction]]. That is:

- It is not control sensitive
- It is not behavior sensitive

---

## Characteristics

Innocuous instructions have the following properties:

### 1. No Resource Effects

- Do not modify processor mode
- Do not change relocation-bounds register
- Do not affect memory allocation
- Do not alter I/O device assignments

### 2. Configuration Independence

- Behavior does not depend on processor mode
- Behavior does not depend on memory location (relocation value)
- Produce identical results regardless of configuration
- No position-dependent addressing

### 3. Direct Execution

- Can execute directly on hardware
- Require no [[Virtual Machine Monitor|VMM]] intervention
- No trapping necessary
- No interpretation needed

---

## Examples

### Arithmetic Operations

```
ADD  R1, R2      // Add register contents
SUB  R3, R4      // Subtract
MUL  R5, R6      // Multiply  
DIV  R7, R8      // Divide
INC  R9          // Increment
DEC  R10         // Decrement
```

### Logical Operations

```
AND  R1, R2      // Bitwise AND
OR   R3, R4      // Bitwise OR
XOR  R5, R6      // Bitwise XOR
NOT  R7          // Bitwise NOT
SHL  R8, 2       // Shift left
SHR  R9, 3       // Shift right
```

### Data Movement (Within Allocated Memory)

```
MOV  R1, R2      // Move register to register
LOAD R3, [R4]    // Load from memory (within bounds)
STORE R5, [R6]   // Store to memory (within bounds)
PUSH R7          // Push to stack
POP  R8          // Pop from stack
```

### Control Flow (Within Allocated Memory)

```
JMP  label       // Unconditional jump
JZ   label       // Jump if zero
JNZ  label       // Jump if not zero
CALL function    // Call subroutine
RET              // Return from subroutine
```

### Comparison Operations

```
CMP  R1, R2      // Compare registers
TEST R3, R4      // Test bits
```

---

## Importance for Virtualization

Innocuous instructions are crucial for the [[Efficiency Property|efficiency property]]:

**All innocuous instructions execute directly on the physical processor with no VMM intervention.**

This ensures:

- Near-native execution speed
- Low overhead (only sensitive instructions require interpretation)
- Statistically dominant instruction subset executes at full speed
- Typical programs spend 90%+ of time executing innocuous instructions

---

## Formal Behavior in Virtual Machines

For any innocuous instruction $i$ and states $S$ and $S' = f(S)$ where $f$ is the [[Virtual Machine Map|virtual machine map]]:

$i(S') = f(i(S))$

That is, executing the instruction in the virtual machine produces the same state transformation as executing it on the real machine, modulo the VM mapping.

---

## Why They Work Without Intervention

Innocuous instructions work correctly in VMs because:

1. **Memory Protection**: The relocation-bounds register ensures memory accesses stay within allocated region
    - Accesses within bounds succeed normally
    - Accesses outside bounds cause memory trap to VMM
2. **Mode Independence**: Results don't depend on mode    
    - No need to check or simulate mode
    - Same operation in supervisor or user mode
3. **Location Independence**: Results don't depend on physical location
    - Relocation handled by hardware
    - Program sees logical addresses
    - No awareness of physical placement

---

## Proportion in Typical Programs

In well-designed third-generation systems:

- 90-95%+ of executed instructions are innocuous
- Only 5-10% require VMM intervention
- This distribution makes virtualization practical
- Performance overhead remains acceptable

---

## Instruction Classification Summary

|Type|Traps?|Direct Execution?|Percentage|
|---|---|---|---|
|Innocuous|No|Yes|~90-95%|
|[[Privileged Instruction\|Privileged]] & Sensitive|Yes|No|~5-10%|
|Sensitive but not Privileged|No|Yes|0% (if virtualizable)|

---

## Related Concepts

- [[Sensitive Instruction]]: Complement of innocuous instructions
- [[Privileged Instruction]]: Instructions that trap in user mode
- [[Efficiency Property]]: Depends on innocuous instructions executing directly
- [[Virtual Machine Monitor]]: Does not intervene for innocuous instructions
- [[Popek-Goldberg Virtualization Theorem]]: Assumes innocuous instructions are safe to execute directly