# Virtual Machine

A **virtual machine (VM)** is an efficient, isolated duplicate of a real computer system. It represents a virtualized execution environment in which guest operating systems and applications run.

---

## Formal Definition

According to Popek and Goldberg, a virtual machine is the environment created by a [[Virtual Machine Monitor|virtual machine monitor (VMM)]]. It provides programs with an environment that is essentially identical to the original machine, with only minor acceptable differences.

---

## Characteristics

A virtual machine exhibits three essential characteristics through its VMM:

1. **Functional Equivalence**: Programs running in the VM produce effects identical to those they would produce on the real machine, except for differences in resource availability and timing dependencies.
2. **Performance**: Programs execute with at worst only minor decreases in speed, as a statistically dominant subset of instructions execute directly on physical hardware.
3. **Safety and Isolation**: The VM is completely isolated from other VMs and from direct access to physical resources not explicitly allocated to it.

---

## Implementation

A VM is typically represented by configuration files that describe:

- Virtual hardware specifications (CPU, memory, disk, network interfaces)
- Machine state information
- Resource allocations
- Virtual device configurations

---

## State Representation

In the Popek-Goldberg model, a virtual machine state consists of four components:

$S = (E, M, P, R)$

Where:

- $E$ = Executable storage (memory)
- $M$ = Processor mode (supervisor or user)
- $P$ = Program counter
- $R$ = Relocation-bounds register (memory management)

---

## Acceptable Differences from Physical Machines

Two categories of differences are permitted:

1. **Resource Availability**: The VM may have different amounts of memory or other resources than the physical machine
2. **Timing Dependencies**: Instruction sequences may take longer due to VMM intervention and concurrent VM execution

---

## Related Concepts

- [[Virtual Machine Monitor]]: The software that creates and manages VMs
- [[Virtual Machine Monitor]]: Alternate term for VMM
- [[Equivalence Property]]: Guarantee of identical functional behavior
- [[Efficiency Property]]: Guarantee of near-native performance
- [[Resource Control Property]]: Guarantee of isolation and safety