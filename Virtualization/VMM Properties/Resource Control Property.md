# Resource Control Property

The **resource control property** (also called **safety requirement**) is the third essential characteristic that a [[Virtual Machine Monitor|virtual machine monitor]] must satisfy. It requires the VMM to maintain complete control over all system resources.

---

## Formal Definition

The VMM must have complete control of system resources, where complete control means:

1. **No Unauthorized Access**: It is impossible for a program running in a [[Virtual Machine|virtual machine]] to access any resource not explicitly allocated to it
2. **Resource Reclamation**: It is possible under certain circumstances for the VMM to regain control of resources already allocated

---

## Controlled Resources

In the Popek-Goldberg model, resources include:

### Memory

- Physical memory allocation
- Memory protection boundaries
- Virtual address spaces
- Memory-mapped I/O regions

### Peripherals

- Disk drives
- Network interfaces
- Display devices
- Input devices

### System State

- Processor mode (supervisor/user)
- Relocation-bounds registers
- Interrupt masks
- Special-purpose registers

Note: The processor itself is not necessarily treated as a resource in the basic model, allowing a single VM to run continuously.

---

## Enforcement Mechanisms

Resource control is enforced through:

### 1. Trap on Sensitive Operations

All [[Control Sensitive Instruction|control sensitive instructions]] must trap to the VMM, ensuring the VMM can:

- Validate resource requests
- Update resource allocation tables
- Deny unauthorized access attempts

### 2. Hardware Protection

The relocation-bounds register mechanism ensures:

- VMs can only access memory within their allocated region
- Memory accesses outside bounds cause traps to VMM
- VMM memory is protected from VM access

### 3. Allocator Intervention

The [[Allocator|allocator]] component:

- Manages all resource allocation decisions
- Maintains resource ownership tables
- Prevents resource conflicts between VMs

---

## Examples of Control

### Memory Access Control

If a VM attempts to access memory outside its allocated bounds:

```
if (address + relocation) > bounds then
    memory_trap  // VMM gains control
```

### Mode Control

If a VM attempts to enter supervisor mode:

```
if instruction_is_privileged and mode = user then
    privileged_instruction_trap  // VMM gains control
```

### Resource Modification Control

If a VM attempts to modify its resource allocation:

```
if instruction_modifies_resources and mode = user then
    trap  // VMM validates and possibly grants request
```

---

## Isolation Guarantee

Resource control provides the isolation property essential for virtualization:

- One VM's malfunction cannot affect another VM
- VMs cannot observe each other's memory or data
- VMM remains protected from all VMs

---

## Failure Cases

Without proper resource control:

- VMs could access arbitrary physical memory
- VMs could interfere with each other
- VMs could access VMM data structures
- System security and stability would be compromised

---

## Relation to Security

Resource control is fundamental to virtualization security:

- Provides isolation boundaries between VMs
- Prevents privilege escalation attacks
- Enables secure multi-tenant environments
- Forms basis for cloud computing security models

---

## Related Concepts

- [[Virtual Machine Monitor]]: Must satisfy resource control property
- [[Equivalence Property]]: First VMM requirement
- [[Efficiency Property]]: Second VMM requirement
- [[Allocator]]: Component that enforces resource control
- [[Control Sensitive Instruction]]: Instructions affecting resource allocation
- [[Relocation-Bounds Register]]: Primary memory control mechanism
- [[Trap Mechanism]]: Hardware mechanism enabling control