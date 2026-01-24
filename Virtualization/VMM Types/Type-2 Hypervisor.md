# Type-2 Hypervisor

A **Type-2 hypervisor** (also called **hosted hypervisor**) is virtualization software that runs as an application within a conventional operating system. It provides [[Virtual Machine|virtual machine]] capabilities on top of an existing OS.

---

## Definition

A Type-2 hypervisor is an application that runs on a standard operating system (the **host OS**) and provides virtual machine management features to **guest operating systems**. Guest OSes run at a third level above the physical hardware.

---

## Architecture

```
┌────────────────────────────────────┐
│   Guest OS 1   │   Guest OS 2      │  Virtual Machines
├────────────────┴───────────────────┤
│      Type-2 Hypervisor App         │  Application layer
├────────────────────────────────────┤
│         Host Operating System      │  Host OS (Windows, Linux, macOS)
├────────────────────────────────────┤
│         Physical Hardware          │  Server/Desktop hardware
└────────────────────────────────────┘
```

The hypervisor depends on the host OS for hardware access and resource management.

---

## Key Characteristics

### 1. Application-Level Software

- Runs as a standard application
- Managed by host operating system
- Subject to host OS scheduling
- Uses host OS device drivers

### 2. Host OS Dependency

- Requires functioning host OS
- Uses host OS for hardware access
- Relies on host OS security
- Cannot start before host OS boots

### 3. User-Friendly

- Familiar OS interface for management
- Easy installation and configuration
- Integration with desktop environment
- Accessible to non-experts

---

## Virtualization Technique

Type-2 hypervisors primarily use:

### [[Full Virtualization]]

- Complete hardware emulation
- Guest OS requires no modification
- Binary translation for privileged instructions
- Direct execution for user-mode code

Key principles:

1. **Binary Translation**: Privileged instructions from guest kernel are dynamically translated to safe sequences
2. **Direct Execution**: User application code executes directly on CPU

---

## Common Examples

### VMware Workstation

- Desktop virtualization leader
- Windows and Linux host support
- Advanced features (snapshots, clones)
- Professional/development use

### VMware Fusion

- VMware's macOS product
- Run Windows/Linux on Mac
- Desktop integration
- Development and testing

### Oracle VirtualBox

- Open-source (mostly)
- Cross-platform (Windows, Linux, macOS, Solaris)
- Free for personal use
- Extensible through APIs

### Parallels Desktop

- macOS virtualization
- Windows integration on Mac
- Performance optimization
- Consumer and business editions

### QEMU (in user-mode)

- Open-source emulation and virtualization
- Can run as Type-2 on various hosts
- Supports many guest architectures
- Often used with KVM for acceleration

---

## Advantages

### Ease of Use

- Install like any application
- Familiar management interface
- No special boot procedures
- Quick setup and configuration

### Flexibility

- Run alongside host OS applications
- Easy switching between host and guests
- Share files and clipboard
- Integration features (drag-and-drop, shared folders)

### Compatibility

- Works on consumer hardware
- No special hardware requirements (though VT-x/AMD-V help)
- Broad host OS support
- Multiple guest OS support

### Cost

- Often free or low-cost
- No enterprise infrastructure needed
- Lower training costs
- Suitable for individual users

### Development and Testing

- Ideal for software development
- Test multiple OS configurations
- Snapshot and rollback capabilities
- Network simulation

---

## Disadvantages

### Performance

- Additional overhead from host OS
- Three levels of software above hardware
- Host OS resource consumption
- Context switching overhead
- Generally 10-30% performance penalty

### Scalability

- Limited VM density
- Host OS resource constraints
- Not designed for many concurrent VMs
- Single point of failure (host OS)

### Security

- Host OS vulnerabilities affect hypervisor
- Larger attack surface
- Less isolation than Type-1
- Dependent on host OS security

### Resource Competition

- Competes with host OS for resources
- Host OS may prioritize its own processes
- Less predictable performance
- I/O bottlenecks through host OS

---

## Guest OS Awareness

A critical characteristic of Type-2 hypervisors using [[Full Virtualization|full virtualization]]:

**Guest OSes are unaware they are virtualized**, meaning:

- No kernel modifications required
- Standard OS installation works
- Runs unmodified operating systems
- Maintains OS compatibility

This is achieved through:

- Binary translation of privileged instructions
- Hardware virtualization extensions (Intel VT-x, AMD-V)
- Trap-and-emulate for sensitive operations

---

## Relationship to VMM Properties

Type-2 hypervisors satisfy [[Virtual Machine Monitor|VMM]] requirements with caveats:

### [[Equivalence Property]]

- Guest behavior matches physical machine
- Subject to host OS interference
- Timing differences more pronounced

### [[Efficiency Property]]

- Direct execution where possible
- More overhead than Type-1
- Binary translation adds cost
- Still qualifies (statistically dominant direct execution)

### [[Resource Control Property]]

- Control mediated through host OS
- Resource isolation maintained
- Depends on host OS capabilities
- VM-to-VM isolation enforced

---

## Use Cases

### Software Development

- Test applications on multiple OSes
- Development environment isolation
- Version testing
- Cross-platform development

### Education and Learning

- Learn different operating systems
- Practice system administration
- Safe experimentation environment
- Training scenarios

### Desktop Virtualization

- Run Windows on Mac (or vice versa)
- Legacy application support
- Isolated browsing environment
- Personal use cases

### Testing and Quality Assurance

- Software compatibility testing
- Multiple configuration testing
- Snapshot-based test scenarios
- Regression testing

---

## Contrast with Other Types

|Feature|Type-2|[[Type-1 Hypervisor\|Type-1]]|[[Type-0 Hypervisor\|Type-0]]|
|---|---|---|---|
|Installation|Application|Bare-metal|Firmware|
|Performance|Moderate|High|Highest|
|Ease of use|Easiest|Moderate|Complex|
|Use case|Desktop/Dev|Data center|Mainframe|
|Cost|Low|High|Very high|
|Portability|High|Limited|None|

---

## Performance Optimization

Modern Type-2 hypervisors improve performance through:

### Hardware Virtualization Support

- Intel VT-x / AMD-V utilization
- Reduces binary translation overhead
- Hardware-assisted memory management
- Improves near-native performance

### Paravirtualized Drivers

- Guest additions/tools
- Optimized network drivers
- Enhanced graphics drivers
- Shared folder acceleration

### Host Integration

- Memory ballooning
- Dynamic resource allocation
- Intelligent caching
- Host-aware optimizations

---

## Related Concepts

- [[Hypervisor]]: Type-2 is one category
- [[Virtual Machine Monitor]]: Formal model Type-2 implements
- [[Full Virtualization]]: Primary technique used
- [[Type-1 Hypervisor]]: Higher-performance alternative
- [[Binary Translation]]: Key technique for instruction handling
- [[Virtual Machine]]: Created and managed by Type-2