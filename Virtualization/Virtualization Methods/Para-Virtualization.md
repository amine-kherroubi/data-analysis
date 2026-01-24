# Para-Virtualization

**Para-virtualization** is a virtualization technique in which the guest operating system kernel is modified to cooperate with the [[Hypervisor|hypervisor]], replacing non-virtualizable instructions with explicit calls to the hypervisor (hypercalls).

---

## Definition

Para-virtualization involves modifying the guest OS kernel to be aware of virtualization and to communicate directly with the hypervisor through a well-defined interface, rather than attempting to execute privileged instructions that would trap or fail.

---

## Core Concept

Instead of attempting to virtualize sensitive instructions through trapping or binary translation, para-virtualization replaces them with **hypercalls**—explicit calls to the hypervisor.

### Standard Virtualization Problem

```
Guest kernel:     LPSW instruction (privileged/sensitive)
                  ↓
                  Trap or requires binary translation
                  ↓
Hypervisor:       Intercept and simulate
```

### Para-Virtualization Solution

```
Modified guest:   HYPERVISOR_set_psw(new_psw)  (hypercall)
                  ↓
                  Direct call to hypervisor
                  ↓  
Hypervisor:       Execute request directly
```

---

## Key Technique: Hypercalls

**Hypercalls** are the fundamental mechanism of para-virtualization:

### Definition

A hypercall is a software interface for guest OSes to explicitly request hypervisor services, similar to how system calls allow applications to request OS services.

### Characteristics

- Explicit, intentional calls to hypervisor
- Well-defined API contract
- More efficient than trap-and-emulate
- Direct communication channel
- Documented interface

### Common Hypercall Categories

1. **Memory Management**: Page table updates, TLB operations
2. **CPU Management**: Virtual CPU control, scheduling hints
3. **I/O Operations**: Device access, interrupt handling
4. **Time Management**: Timer operations, clock access

---

## Kernel Modification Requirements

Para-virtualization requires modifying guest OS kernel code:

### Areas Modified

1. **Privilege-sensitive code**: Replace privileged instructions with hypercalls
2. **Memory management**: Direct page table manipulation becomes hypercalls
3. **Interrupt handling**: Virtualized interrupt delivery
4. **Device drivers**: Paravirtualized driver interfaces

### Example Modifications

```c
// Original kernel code
__asm__ ("cli");  // Disable interrupts (privileged instruction)

// Para-virtualized kernel code  
HYPERVISOR_disable_interrupts();  // Hypercall instead
```

---

## Advantages

### Performance

- **Higher performance than [[Full Virtualization|full virtualization]]**
- Eliminates binary translation overhead
- No privileged instruction traps (replaced with direct calls)
- Reduced virtualization overhead (often 2-5%)
- More efficient I/O through paravirtualized drivers

### Stability

- **More stable and predictable**
- Explicit guest-hypervisor interface
- Fewer edge cases than trap-and-emulate
- Better error handling
- Clearer semantics

### Efficiency

- Fewer mode transitions
- Batch operations possible
- Better memory management
- Optimized I/O paths

### Guest Awareness

- Guest can optimize for virtual environment
- Cooperative resource management
- Better scheduling hints
- Intelligent caching decisions

---

## Disadvantages

### Kernel Modification Required

- **Cannot run unmodified operating systems**
- Must modify guest OS kernel source code
- Requires kernel recompilation
- Ongoing maintenance for kernel updates

### Limited to Open-Source Systems

- **Primarily limited to Linux and BSD**
- Cannot modify closed-source OSes (Windows, macOS without vendor cooperation)
- Requires access to kernel source code
- Licensing considerations

### Maintenance Burden

- Must keep modifications synchronized with upstream kernel
- Compatibility across kernel versions
- Testing for each kernel release
- Potential for fragmentation

### Portability

- Tied to specific hypervisor API
- Migration between hypervisors requires re-modification
- Not standardized (though some convergence with virtio)

---

## Implementation Examples

### Xen

Pioneer of para-virtualization:

- **Domain 0 (dom0)**: Privileged guest for management
- **Domain U (domU)**: Paravirtualized guests
- Hypercall interface well-documented
- Originally para-virtualization only
- Now supports [[Hardware Virtualization|HVM]] as well

### Linux KVM with virtio

Hybrid approach:

- KVM provides [[Hardware Virtualization|hardware virtualization]]
- virtio provides paravirtualized I/O drivers
- Best of both approaches
- Standard in modern Linux virtualization

### VMware (paravirtualized drivers)

Even [[Full Virtualization|full virtualization]] products use para-virtualization:

- Guest tools provide paravirtualized drivers
- VMware Tools includes optimized drivers
- Improved I/O performance
- Enhanced guest integration

---

## Paravirtualized Device Drivers

One of the most successful applications:

### virtio Framework

Standard for paravirtualized devices:

- **virtio-net**: Network devices
- **virtio-blk**: Block devices (disks)
- **virtio-scsi**: SCSI controllers
- **virtio-serial**: Serial devices
- Supported by Linux, FreeBSD, Windows (with drivers)

### Benefits

- Much higher performance than emulated devices
- Lower CPU overhead
- Better throughput
- Reduced latency

---

## Supported Hypervisors

Para-virtualization is supported by both [[Type-1 Hypervisor|Type-1]] and [[Type-2 Hypervisor|Type-2]] hypervisors:

### Type-1 Examples

- **Xen**: Original para-virtualization hypervisor
- **VMware ESXi**: Supports paravirtualized drivers
- **KVM**: virtio paravirtualized devices

### Type-2 Examples

- **VMware Workstation**: VMware Tools paravirtualized drivers
- **VirtualBox**: Guest additions with paravirtualized components

---

## Relationship to VMM Properties

Para-virtualization maintains [[Virtual Machine Monitor|VMM]] properties:

### [[Equivalence Property]]

- Modified guest provides same functionality
- Hypercalls produce equivalent results to privileged instructions
- Application compatibility maintained

### [[Efficiency Property]]

- Enhanced efficiency through direct hypercalls
- Better than [[Full Virtualization|full virtualization]] for many workloads
- Reduced overhead

### [[Resource Control Property]]

- Hypervisor maintains complete control
- Explicit hypercall interface enforces boundaries
- Strong isolation maintained

---

## Modern Hybrid Approach

Contemporary systems combine techniques:

### Hardware Virtualization + Paravirtualized I/O

Most common modern approach:

- [[Hardware Virtualization|Hardware virtualization]] (VT-x/AMD-V) for CPU
- Unmodified guest OS kernel
- Paravirtualized drivers (virtio) for I/O
- Best performance without kernel modification

### Benefits

- No guest kernel modification
- Near-native CPU performance
- Optimized I/O performance
- Supports all operating systems

---

## Historical Significance

Para-virtualization was crucial in virtualization evolution:

### Before Hardware Support (2003-2006)

- Only practical high-performance option
- Xen demonstrated superiority over full virtualization
- Proved viability of production virtualization

### After Hardware Support (2006+)

- Less critical for CPU virtualization
- Still valuable for I/O optimization
- Evolved into hybrid approaches
- virtio standardization

---

## Contrast with Other Techniques

|Feature|Para-Virtualization|[[Full Virtualization]]|[[Hardware Virtualization]]|
|---|---|---|---|
|Guest modification|Required|None|None|
|Performance|High|Moderate|Highest|
|Guest support|Linux, BSD mainly|Any OS|Any OS|
|I/O performance|Excellent|Moderate|Good to excellent|
|Maintenance|High|Low|Low|

---

## Related Concepts

- [[Hypervisor]]: Provides hypercall interface
- [[Type-1 Hypervisor]]: Commonly uses para-virtualization
- [[Hardware Virtualization]]: Modern alternative for CPU virtualization
- [[Full Virtualization]]: Alternative not requiring modification
- [[Virtual Machine Monitor]]: Implements para-virtualization
- [[Hypercall]]: Core mechanism of para-virtualization