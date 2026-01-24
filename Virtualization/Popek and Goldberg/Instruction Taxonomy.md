# Instruction Taxonomy

## Privileged Instructions

To execute **privileged instructions**, the processor must be in a particular mode: the **privileged mode**. If the processor is not in this state, an interrupt is triggered and the instruction is **trapped**. The interrupt consists of a context change during which the system (whether the host OS or the hypervisor) decides what action to take on this instruction.

---

## Configuration-Sensitive Instructions

**Configuration-sensitive instructions** are the instructions that have the ability to query or modify system configuration such as processor mode or the amount of memory allocated to a particular process.

---

## Behavior-Sensitive Instructions

**Behavior-sensitive instructions** are instructions whose behavior or result will depend on the state or availability of resources (whether the processor is in privileged mode or not, etc.).