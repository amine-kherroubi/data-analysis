# Full Virtualization

**Full virtualization** is a virtualization technique that allows unmodified guest operating systems to run in [[Virtual Machine|virtual machines]] by providing complete emulation of the underlying hardware.

---

## Definition

Full virtualization creates virtual environments that are complete replicas of physical machines, allowing any operating system designed for the target architecture to run without modification. Guest operating systems are completely unaware that they are virtualized.

---

## Core Principles

Full virtualization relies on two fundamental techniques:

### 1. Binary Translation

Privileged instructions from the guest OS kernel are dynamically intercepted and translated into safe instruction sequences.

**Process:**

- Guest kernel code is scanned as it executes
- [[Privileged Instruction|Privileged]] and [[Sensitive Instruction|sensitive instructions]] are detected
- Problematic instructions are replaced with safe sequences
- Translated code is cached for reuse
- Safe sequences call [[Hypervisor|hypervisor]] functions

**Example:**

```
Guest kernel attempts:    LPSW  (Load PSW - privileged)
Binary translator:        Converts to safe sequence calling hypervisor
Hypervisor:              Simulates LPSW effect in virtual state
```

### 2. Direct Execution

User-mode application code (non-privileged instructions) executes directly on the physical CPU without translation.

**Characteristics:**

- [[Innocuous Instruction|Innocuous instructions]] run at native speed
- No interpretation or translation needed
- Supports the [[Efficiency Property|efficiency property]]
- Typically 90%+ of instructions execute directly

---

## Key Characteristics

### Guest OS Unawareness

The most critical aspect of full virtualization:

- **No kernel modifications required**
- Standard OS installations work unchanged
- Guest OS believes it controls physical hardware
- Complete compatibility with existing software

### Architecture Limitation

- Can only virtualize OSes of the **same architecture** as the host
- x86 hypervisor can only run x86 guest OSes
- ARM hypervisor can only run ARM guest OSes
- Cannot virtualize across architectures (that requires emulation)

### Complete Hardware Emulation

The [[Hypervisor|hypervisor]] provides virtualized versions of:

- CPU (through binary translation and direct execution)
- Memory (through address translation)
- Disk controllers
- Network interfaces
- Video adapters
- Other peripherals

---

## Implementation Approach

### Without Hardware Support (Classical Full Virtualization)

When hardware virtualization extensions are unavailable:

1. **Scan and Translate**: Monitor guest kernel code execution
2. **Cache Translations**: Store translated code blocks for reuse
3. **Execute Safely**: Run translated privileged code in safe manner
4. **Direct Execution**: Run user code without intervention
5. **Simulate Effects**: Update virtual machine state correctly

### With Hardware Support (Hardware-Assisted Full Virtualization)

Modern processors provide virtualization extensions:

- Intel VT-x (Virtual Machine Extensions)
- AMD-V (AMD Virtualization)

These extensions:

- Eliminate need for binary translation
- Provide hardware trap mechanisms for sensitive instructions
- Accelerate guest OS execution
- Reduce hypervisor complexity

---

## Supported by Type-2 Hypervisors

Full virtualization is the characteristic technique of [[Type-2 Hypervisor|Type-2 hypervisors]]:

### VMware Workstation / Fusion

- Pioneer of binary translation
- Sophisticated translation engine
- Hardware acceleration support
- Optimized performance

### Oracle VirtualBox

- Open-source full virtualization
- Cross-platform support
- Hardware virtualization when available
- Software fallback when not

### Parallels Desktop

- macOS-focused full virtualization
- Optimized for Mac hardware
- Windows integration features

---

## Advantages

### User Convenience

- **Simplest to use**: No guest OS modification needed
- Standard OS installation procedures
- Familiar guest OS behavior
- Easy migration from physical machines

### Compatibility

- Runs any OS for the target architecture
- No guest OS cooperation required
- Supports closed-source operating systems
- Legacy OS support

### Transparency

- Complete abstraction of underlying hardware
- Guest OS sees consistent hardware
- Hardware independence
- Easy portability between hosts

---

## Disadvantages

### Performance Overhead

- Binary translation adds CPU overhead
- Instruction scanning and caching
- Translation cache management
- Typically 10-30% performance penalty without hardware support
- 2-10% with hardware virtualization

### Complexity

- Sophisticated binary translation engine required
- Complex state management
- Large hypervisor codebase
- More potential for bugs

### Architecture Restriction

- Cannot cross architecture boundaries
- Limited to OSes of same architecture
- Requires emulation for different architectures

---

## Performance Considerations

### Binary Translation Overhead

The cost of translation varies:

- **User code**: 0% (executes directly)
- **Kernel code**: Varies based on:
    - Frequency of privileged instructions
    - Translation cache hit rate
    - Quality of translation engine

### Hardware Acceleration Impact

With Intel VT-x or AMD-V:

- Eliminates binary translation
- Hardware traps replace translation
- Near-native performance achievable
- Overhead reduced to 2-10%

---

## Relationship to VMM Properties

Full virtualization satisfies [[Virtual Machine Monitor|VMM]] properties:

### [[Equivalence Property]]

- Guest OS behavior identical to physical machine
- Binary translation preserves instruction semantics
- Correct simulation of all hardware

### [[Efficiency Property]]

- User-mode code executes directly
- Kernel code translated and cached
- Statistically dominant subset runs at full speed

### [[Resource Control Property]]

- Hypervisor interposes on privileged operations
- Complete control over resource allocation
- VM isolation maintained

---

## Contrast with Other Techniques

|Feature|Full Virtualization|[[Para-Virtualization]]|[[Hardware Virtualization]]|
|---|---|---|---|
|Guest modification|None|Required|None|
|Performance|Moderate|High|Highest|
|Guest support|Any OS|Modified only|Any OS|
|Complexity|High|Moderate|Low (hardware-assisted)|
|Typical use|Desktop/development|Enterprise Linux|Modern data centers|

---

## Evolution and Modern Usage

### Historical

- Developed when hardware virtualization unavailable
- Binary translation was revolutionary
- VMware pioneered commercial implementation

### Current

- Still relevant for:
    - Older hardware without VT-x/AMD-V
    - CPU compatibility modes
    - Nested virtualization scenarios
- Often combined with hardware virtualization
- Fallback when hardware features unavailable

---

## Related Concepts

- [[Type-2 Hypervisor]]: Primary users of full virtualization
- [[Binary Translation]]: Core technique for privileged instructions
- [[Hardware Virtualization]]: Modern complement/alternative
- [[Para-Virtualization]]: Alternative requiring guest modification
- [[Virtual Machine Monitor]]: Implements full virtualization
- [[Sensitive Instruction]]: Instructions requiring translation
- [[Efficiency Property]]: Maintained through direct execution