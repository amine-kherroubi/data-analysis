# Type-1 Hypervisor

A **Type-1 hypervisor** (also called **bare-metal hypervisor** or **native hypervisor**) runs directly on the physical hardware, without an underlying operating system. It provides the foundation for [[Virtual Machine|virtual machines]] with minimal software overhead.

---

## Definition

A Type-1 hypervisor is system software that executes directly on hardware and manages virtual machines. It serves as both the hypervisor and a minimal operating system optimized for virtualization.

---

## Architecture

```
┌─────────────────────────────────────┐
│   VM 1    │   VM 2    │   VM 3      │  Virtual Machines
├─────────────────────────────────────┤
│         Type-1 Hypervisor           │  Bare-metal layer
├─────────────────────────────────────┤
│         Physical Hardware           │  Server hardware
└─────────────────────────────────────┘
```

The hypervisor has direct access to all hardware resources and provides them to virtual machines.

---

## Key Characteristics

### 1. Direct Hardware Execution

- Runs directly on physical hardware
- No host operating system
- First software to execute after firmware
- Complete control over all hardware

### 2. Minimal Software Footprint

- Lightweight compared to full OS
- Optimized specifically for virtualization
- Reduced attack surface
- Lower memory overhead

### 3. Hardware Resource Management

- Direct hardware access and control
- Native driver support for physical devices
- Efficient resource scheduling
- Direct I/O operations

---

## Virtualization Techniques

Type-1 hypervisors commonly use:

### [[Hardware Virtualization]]

- Processor-assisted virtualization (Intel VT-x, AMD-V)
- Hardware-accelerated memory management
- Direct device assignment
- IOMMU support

### [[Para-Virtualization]]

- Modified guest kernels for cooperation
- Hypercalls for privileged operations
- Optimized I/O through paravirtualized drivers
- Reduced virtualization overhead

---

## Common Examples

### VMware ESXi

- Industry-leading bare-metal hypervisor
- Proprietary implementation
- Advanced features (vMotion, HA, DRS)
- Enterprise focus

### Microsoft Hyper-V

- Integrated with Windows Server
- Technically runs as a role above the kernel
- Type-1 functionality and performance
- Windows and Linux guest support

### Xen

- Open-source hypervisor
- Supports both paravirtualization and HVM
- Used by cloud providers (AWS, Rackspace)
- Strong security model

### KVM (Kernel-based Virtual Machine)

- Turns Linux kernel into hypervisor
- Integrated into Linux kernel
- Leverages Linux device drivers
- Full virtualization with hardware support

### Nutanix AHV

- Based on KVM
- Integrated with Nutanix HCI platform
- Enterprise features
- No licensing cost

### Citrix Hypervisor (formerly XenServer)

- Based on Xen
- Enterprise features
- Free and commercial editions
- Server virtualization focus

---

## Advantages

### Performance

- Minimal overhead between VMs and hardware
- Direct hardware access
- Optimized for virtualization workloads
- Better resource utilization than Type-2

### Security

- Smaller attack surface than full OS
- Isolation between VMs enforced at hypervisor level
- No host OS vulnerabilities
- Simpler security model

### Scalability

- Can support many concurrent VMs
- Efficient resource management
- High VM density per physical host
- Enterprise-scale deployments

### Management

- Centralized VM management
- Consistent administration
- Live migration capabilities
- High availability features

---

## Disadvantages

### Hardware Compatibility

- Requires specific hardware support
- Driver availability limitations
- Vendor-specific features
- Less flexible than Type-2

### Complexity

- More complex initial setup
- Specialized knowledge required
- Less familiar interface than desktop OS
- Requires dedicated management tools

### Cost

- Often requires licensing (except open-source options)
- Specialized hardware recommended
- Training costs
- Enterprise support costs

---

## Special Case: Hybrid Type-1

Some systems blur the line between Type-0 and Type-1:

### Microsoft Hyper-V Architecture

- Hypervisor layer sits below Windows kernel
- Windows Server runs as "parent partition"
- Has Type-1 performance characteristics
- Leverages Windows management

### KVM Architecture

- Linux kernel becomes the hypervisor
- Full OS functionality available
- Type-1 performance
- Type-2-like flexibility

---

## Relationship to VMM Properties

Type-1 hypervisors satisfy [[Virtual Machine Monitor|VMM]] requirements:

### [[Equivalence Property]]

- VMs behave identically to physical machines
- Correct instruction interpretation
- Proper trap-and-emulate or hardware virtualization

### [[Efficiency Property]]

- Direct hardware execution for most instructions
- Minimal intervention overhead
- Hardware-assisted virtualization
- Near-native performance

### [[Resource Control Property]]

- Complete control over all hardware
- Strong VM isolation
- Secure resource allocation
- Hardware-enforced boundaries

---

## Use Cases

### Data Centers

- Server consolidation
- Multi-tenant hosting
- Cloud infrastructure
- Development/testing environments

### Cloud Computing

- Infrastructure-as-a-Service (IaaS)
- Private cloud platforms
- Public cloud providers
- Hybrid cloud deployments

### Enterprise IT

- Production workloads
- Business-critical applications
- High-availability systems
- Disaster recovery

---

## Contrast with Other Types

|Feature|Type-1|[[Type-2 Hypervisor\|Type-2]]|[[Type-0 Hypervisor\|Type-0]]|
|---|---|---|---|
|Runs on|Hardware|Host OS|Firmware|
|Performance|High|Moderate|Highest|
|Flexibility|Moderate|High|Low|
|Use case|Data center|Desktop/development|Mainframe|
|Cost|High|Low|Very high|

---

## Related Concepts

- [[Hypervisor]]: Type-1 is one category
- [[Virtual Machine Monitor]]: Formal model Type-1 implements
- [[Hardware Virtualization]]: Technology used by Type-1
- [[Para-Virtualization]]: Alternative technique for Type-1
- [[Type-2 Hypervisor]]: Alternative architecture
- [[Virtual Machine]]: Created and managed by Type-1