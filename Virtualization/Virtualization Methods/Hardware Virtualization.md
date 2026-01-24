# Hardware Virtualization

**Hardware virtualization** (also called **hardware-assisted virtualization** or **accelerated virtualization**) is a virtualization approach that leverages specialized processor features to improve the performance and simplify the implementation of [[Virtual Machine|virtual machines]].

---

## Definition

Hardware virtualization uses CPU extensions specifically designed to support virtualization, enabling [[Hypervisor|hypervisors]] to run guest operating systems more efficiently and with less software complexity than traditional virtualization techniques.

---

## Core Concept

Instead of relying on software techniques like binary translation or guest OS modification, hardware virtualization provides:

1. **New processor execution modes** for running guest code
2. **Hardware management** of sensitive instructions
3. **Accelerated memory management** for virtual machines
4. **Direct I/O device assignment** capabilities

---

## Major Processor Extensions

### Intel VT-x (Virtual Machine Extensions)

Introduced in 2005-2006:

**Key Features:**

- **VMX Operation**: Virtual Machine Extensions mode
    - VMX root operation (hypervisor runs here)
    - VMX non-root operation (guest VMs run here)
- **VMCS (Virtual Machine Control Structure)**: Hardware-managed VM state
- **VM Entry/Exit**: Hardware-assisted transitions
- **EPT (Extended Page Tables)**: Hardware-accelerated memory virtualization

**Instructions:**

- `VMXON`: Enter VMX operation
- `VMLAUNCH/VMRESUME`: Start/resume guest execution
- `VMCALL`: Hypercall from guest to hypervisor
- `VMREAD/VMWRITE`: Access VMCS fields

### AMD-V (AMD Virtualization)

AMD's equivalent technology (also called SVM - Secure Virtual Machine):

**Key Features:**

- **Guest mode**: Special execution mode for VMs
- **VMCB (Virtual Machine Control Block)**: Hardware VM state
- **Automatic state save/restore**
- **NPT (Nested Page Tables)**: AMD's memory virtualization
- **AVIC (Advanced Virtual Interrupt Controller)**: Interrupt virtualization

**Instructions:**

- `VMRUN`: Enter guest mode
- `VMMCALL`: Hypercall from guest
- `VMLOAD/VMSAVE`: State management

---

## How Hardware Virtualization Works

### VM Entry and Exit

**VM Entry (hypervisor → guest):**

```
1. Hypervisor configures VMCS/VMCB
2. Executes VMLAUNCH/VMRUN instruction
3. Hardware loads guest state from VMCS/VMCB
4. Switches to VMX non-root / guest mode
5. Guest begins executing
```

**VM Exit (guest → hypervisor):**

```
1. Sensitive operation or event occurs
2. Hardware automatically:
   - Saves guest state to VMCS/VMCB
   - Loads hypervisor state
   - Returns to VMX root operation
3. Hypervisor handles the exit reason
4. Hypervisor can re-enter guest via VM Entry
```

### Two-Dimensional Paging

Memory virtualization through hardware:

**Without Hardware Support:**

- Guest virtual → Guest physical (guest OS manages)
- Guest physical → Host physical (hypervisor manages via shadow page tables)
- Complex, slow, error-prone

**With EPT/NPT:**

- Guest virtual → Guest physical (guest page tables)
- Guest physical → Host physical (EPT/NPT - hardware-managed)
- Single lookup, hardware-accelerated
- Dramatically reduced overhead

---

## Advantages

### Performance

- **Near-native execution speed**
- Eliminates binary translation overhead
- Hardware-managed state transitions
- Optimized memory virtualization
- Typical overhead: 2-5% (vs. 10-30% with software techniques)

### Simplicity

- **Simplified hypervisor implementation**
- Less software complexity
- Hardware handles sensitive instructions automatically
- Reduced hypervisor code size
- Fewer potential bugs

### Compatibility

- **Runs unmodified guest operating systems**
- No guest kernel modification required
- Support for closed-source OSes
- Legacy OS compatibility

### Security

- Hardware-enforced isolation
- Reduced trusted computing base
- Clearer security boundaries
- Hardware protection mechanisms

---

## Improved Efficiency Property

Hardware virtualization enhances the [[Efficiency Property|efficiency property]]:

### Before Hardware Support

- [[Innocuous Instruction|Innocuous instructions]]: Direct execution ✓
- [[Sensitive Instruction|Sensitive instructions]]: Software intervention required
- Binary translation or [[Para-Virtualization|para-virtualization]] needed

### With Hardware Support

- Innocuous instructions: Direct execution ✓
- Sensitive instructions: Hardware-managed trapping
- **Near-zero overhead for mode transitions**
- Maintains or exceeds efficiency requirement

---

## Extended Features

Modern hardware virtualization includes advanced capabilities:

### I/O Virtualization (VT-d/AMD-Vi)

- **IOMMU (I/O Memory Management Unit)**
- Direct device assignment to VMs
- DMA remapping
- Interrupt remapping
- Improved I/O performance and security

### Nested Virtualization

- Running hypervisors inside VMs
- Supports [[Popek-Goldberg Recursion Theorem|recursive virtualization]]
- Cloud development and testing
- VM-based sandboxing

### Virtual Interrupt Delivery

- Hardware-accelerated interrupt handling
- Reduced VM exits for interrupts
- Posted interrupts
- Lower latency

---

## Solving Non-Virtualizable Architectures

Hardware virtualization solved the x86 virtualization problem:

### x86 Before VT-x/AMD-V

The x86 architecture violated the [[Popek-Goldberg Virtualization Theorem|Popek-Goldberg theorem]]:

- Some [[Sensitive Instruction|sensitive instructions]] were not [[Privileged Instruction|privileged]]
- Failed silently instead of trapping
- Required complex workarounds

**Example Problems:**

```
POPF    // Pop flags - behavior depends on mode, doesn't trap
PUSHF   // Push flags - reads mode without trapping  
SGDT    // Store GDT - reads sensitive state without trapping
```

### Hardware Virtualization Solution

- New execution modes for guests
- All sensitive operations cause VM exits
- Hardware properly traps or handles all cases
- Made x86 truly virtualizable

---

## Supported by Type-0 and Type-1 Hypervisors

Hardware virtualization is primarily used by:

### [[Type-1 Hypervisor|Type-1 Hypervisors]]

- **VMware ESXi**: Requires VT-x/AMD-V for modern versions
- **KVM**: Leverages hardware virtualization extensively
- **Xen**: HVM mode uses VT-x/AMD-V
- **Microsoft Hyper-V**: Built on hardware virtualization
- **Nutanix AHV**: KVM-based with hardware support

### [[Type-0 Hypervisor|Type-0 Hypervisors]]

- Firmware hypervisors can leverage processor features
- Enhanced isolation and performance
- Complementary to firmware-level virtualization

### Type-1.5: General-Purpose OS Hypervisors

- **Linux KVM**: Turns Linux into hypervisor via VT-x/AMD-V
- **Windows Hyper-V**: Hypervisor role using VT-x/AMD-V

---

## Virtual Machine Management Tools

Hardware virtualization often requires management tools:

### vSphere Client (VMware)

- Configure and manage ESXi hosts
- VM creation and management
- Resource allocation
- Live migration (vMotion)

### Proxmox VE

- Open-source virtualization platform
- Web-based management
- KVM-based virtualization
- Container support

### oVirt / Red Hat Virtualization

- Open-source management platform
- Based on KVM
- Enterprise features
- Data center virtualization

---

## Relationship to VMM Properties

Hardware virtualization strongly supports [[Virtual Machine Monitor|VMM]] properties:

### [[Equivalence Property]]

- Hardware ensures correct instruction behavior
- Proper sensitive instruction handling
- Accurate state transitions

### [[Efficiency Property]]

- **Optimal efficiency**
- Minimal software overhead
- Hardware-accelerated operations
- Near-native performance

### [[Resource Control Property]]

- Hardware-enforced isolation
- Memory protection via EPT/NPT
- I/O protection via VT-d/AMD-Vi
- Strong security boundaries

---

## Hybrid Approaches

Modern systems often combine techniques:

### Hardware Virtualization + Paravirtualized I/O

The most common production configuration:

- VT-x/AMD-V for CPU virtualization
- EPT/NPT for memory management
- [[Para-Virtualization|virtio]] paravirtualized drivers for I/O
- Best overall performance

This approach:

- Requires no guest OS modification (unlike pure para-virtualization)
- Achieves excellent performance
- Supports all operating systems
- Optimizes I/O operations

---

## Contrast with Other Techniques

|Feature|Hardware Virtualization|[[Full Virtualization]]|[[Para-Virtualization]]|
|---|---|---|---|
|Guest modification|None|None|Required|
|Performance|Highest (2-5% overhead)|Moderate (10-30%)|High (2-5%)|
|Guest support|Any OS|Any OS|Linux, BSD mainly|
|Hypervisor complexity|Low|High|Moderate|
|CPU support required|Yes (VT-x/AMD-V)|No|No|

---

## Checking for Hardware Support

### Linux

```bash
# Intel VT-x
grep vmx /proc/cpuinfo

# AMD-V  
grep svm /proc/cpuinfo
```

### Windows

- Check BIOS/UEFI settings
- Use CPU-Z or similar tools
- Windows features for Hyper-V

### macOS

- Intel Macs: Generally have VT-x
- M1/M2 Macs: ARM virtualization extensions

---

## Related Concepts

- [[Type-1 Hypervisor]]: Primary users of hardware virtualization
- [[Type-0 Hypervisor]]: Can leverage hardware features
- [[Virtual Machine Monitor]]: Simplified by hardware support
- [[Efficiency Property]]: Optimized by hardware virtualization
- [[Popek-Goldberg Virtualization Theorem]]: Hardware makes architectures virtualizable
- [[Para-Virtualization]]: Often combined with hardware virtualization