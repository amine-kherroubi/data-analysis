# Allocator

The **allocator** is a component of a [[Hypervisor|hypervisor]] or [[Virtual Machine Monitor|VMM]] responsible for determining which system resources should be provided to each [[Virtual Machine|virtual machine]].

---

## Primary Responsibilities

The allocator manages resource allocation with two key objectives:

1. **Isolation**: Ensure that each VM receives its allocated resources without interference
2. **Exclusivity**: Prevent the same resource from being simultaneously allocated to multiple VMs

---

## Resource Management

In the Popek-Goldberg model, the primary resource is memory, managed through the relocation-bounds register. In practical systems, the allocator manages:

- Physical memory allocation and mapping
- Virtual memory spaces
- I/O devices and peripherals
- Processor time (in time-sharing VMMs)

---

## Invocation

The allocator is invoked by the [[Dispatcher|dispatcher]] when a [[Virtual Machine|VM]] attempts to execute a [[Privileged Instruction|privileged instruction]] that would modify its resource allocation. Examples include:

- Attempting to change the relocation-bounds register
- Requesting additional memory
- Attempting to access I/O devices
- Executing HALT (if processor is treated as a resource)

---

## Implementation

The allocator maintains resource tracking data structures:

- **Resource tables**: Track which resources are allocated to which VMs
- **Free resource lists**: Track available unallocated resources
- **Allocation policies**: Determine how to satisfy or deny resource requests

---

## Single VM Case

For a single VM, the allocator's task is simplified:

- Separate the VM from the VMM in memory
- Ensure the VM cannot access VMM memory or data structures
- Provide the VM with a contiguous or managed memory space

---

## Multiple VM Case

With multiple concurrent VMs, the allocator must:

- Prevent resource conflicts between VMs
- Arbitrate competing resource requests
- Maintain fairness or priority policies
- Handle resource reclamation when VMs terminate

---

## Resource Denial

The allocator may deny resource requests when:

- Requested resources are unavailable
- Request would violate isolation
- Request exceeds VM's allocation limits

When denial occurs, the VM experiences behavior equivalent to running on a smaller physical machine.

---

## Related Concepts

- [[Hypervisor]]: Contains the allocator as a component
- [[Dispatcher]]: Invokes the allocator when needed
- [[Resource Control Property]]: VMM property ensured by the allocator
- [[Relocation-Bounds Register]]: Primary mechanism for memory resource control
- [[Control Sensitive Instruction]]: Instructions that affect resource allocation