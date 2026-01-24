# Hypervisor

A **hypervisor** is a synonym for [[Virtual Machine Monitor|virtual machine monitor (VMM)]]. It is a lightweight software layer that enables the creation and management of [[Virtual Machine|virtual machines]] while allowing maximum allocation of physical resources to those virtual machines.

---

## Terminology

The terms "hypervisor" and "virtual machine monitor" are used interchangeably in the virtualization literature. The Popek and Goldberg formalization uses "virtual machine monitor," while industry practice often prefers "hypervisor."

---

## Classification

Hypervisors are commonly classified into types based on their implementation:

- [[Type-0 Hypervisor]]: Firmware-based implementation
- [[Type-1 Hypervisor]]: Bare-metal system implementation
- [[Type-2 Hypervisor]]: Hosted application implementation

---

## Essential Properties

As a VMM, a hypervisor must satisfy three fundamental properties:

1. **[[Equivalence Property]]**: Programs execute with identical effects as on real hardware
2. **[[Efficiency Property]]**: Most instructions execute directly without intervention
3. **[[Resource Control Property]]**: Complete control over system resources

---

## Architecture

A hypervisor consists of three functional components:

### Dispatcher (Regulator)

The highest-level control module that receives control when privileged operations occur and determines which other component should handle the operation.

### Allocator

Determines which system resources should be allocated to each virtual environment. Ensures that the same resource is never simultaneously allocated to multiple virtual machines. Called by the dispatcher when a [[Virtual Machine|VM]] attempts to modify its resource allocation.

### Interpreter Routines

A set of interpretation routines, one for each [[Privileged Instruction|privileged instruction]], that simulate the effect of instructions that cannot execute directly. These routines reproduce the state changes that would occur if the instruction executed on real hardware.

---

## Related Concepts

- [[Virtual Machine Monitor]]: Formal term for hypervisor
- [[Virtual Machine]]: Environment created by the hypervisor
- [[Dispatcher]]: Top-level control component
- [[Allocator]]: Resource management component
- [[Interpreter Routines]]: Instruction simulation components