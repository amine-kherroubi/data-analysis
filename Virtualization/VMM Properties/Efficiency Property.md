# Efficiency Property

The **efficiency property** (also called **performance requirement**) is the second essential characteristic that a [[Virtual Machine Monitor|virtual machine monitor]] must satisfy. It requires that a statistically dominant subset of virtual processor instructions execute directly on real hardware with no software intervention.

---

## Formal Definition

A statistically dominant subset of the virtual processor's instructions must be executed directly by the real processor, with no software intervention by the VMM.

"Statistically dominant" means that in typical program execution, the vast majority of instructions (usually well over 90%) fall into this category.

---

## What This Excludes

The efficiency property explicitly rules out several system types:

### Traditional Emulators

Systems that translate every instruction from one architecture to another cannot satisfy this property because all instructions require translation overhead.

### Complete Software Interpreters (Simulators)

Systems that interpret every instruction in software, fetching and decoding each instruction in the VMM, are too slow to be considered virtual machines.

### Instruction-by-Instruction Trapping

Systems where every instruction causes a trap to the VMM fail this requirement due to the overhead of constant intervention.

---

## What Executes Directly

In a properly designed VMM, [[Innocuous Instruction|innocuous instructions]] execute directly:

- Arithmetic operations (ADD, SUBTRACT, MULTIPLY, DIVIDE)
- Logical operations (AND, OR, XOR, NOT)
- Data movement within allocated memory
- Control flow (branches, jumps) within valid address space
- Comparison operations
- Most user-mode instructions

These constitute the vast majority of instructions in typical programs.

---

## What Requires Intervention

Only [[Privileged Instruction|privileged instructions]] and [[Sensitive Instruction|sensitive instructions]] require VMM intervention:

- Instructions that modify processor mode
- Instructions that modify memory allocation (relocation-bounds register)
- Instructions that access I/O devices
- Instructions that modify interrupt state
- Instructions whose behavior depends on processor mode or memory location

These are relatively rare in typical program execution.

---

## Performance Implications

The efficiency property implies:

- Near-native execution speed for compute-intensive applications
- Overhead only proportional to rate of privileged/sensitive instruction execution
- Acceptable performance degradation (typically 5-20% in well-designed systems)

Without this property, virtualized execution would be orders of magnitude slower than native execution.

---

## Modern Hardware Support

Modern processors enhance efficiency through [[Hardware Virtualization|hardware virtualization]] features:

- Intel VT-x (Virtualization Technology)
- AMD-V (AMD Virtualization)

These extensions reduce the performance gap even further by:

- Reducing trap overhead
- Providing hardware-assisted memory management
- Accelerating mode transitions

---

## Measurement

Efficiency can be measured by:

- Percentage of instructions executing directly vs. requiring interpretation
- Performance overhead compared to native execution
- Instructions per second (IPS) ratio between VM and physical machine

---

## Related Concepts

- [[Virtual Machine Monitor]]: Must satisfy efficiency property
- [[Equivalence Property]]: First VMM requirement
- [[Resource Control Property]]: Third VMM requirement
- [[Innocuous Instruction]]: Instructions that execute directly
- [[Privileged Instruction]]: Instructions requiring intervention
- [[Hardware Virtualization]]: Hardware features enhancing efficiency