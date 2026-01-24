# Type-0 Hypervisor

A **Type-0 hypervisor** (also called **firmware hypervisor**) is implemented directly in firmware, providing virtualization support at the lowest level of the system software stack, below even the operating system kernel.

---

## Definition

Type-0 hypervisors are built into system firmware and provide fundamental virtualization capabilities through firmware-level interfaces. They represent the most hardware-integrated form of [[Hypervisor|hypervisor]].

---

## Characteristics

### 1. Firmware Implementation

- Embedded in system firmware (BIOS/UEFI)
- Executes before any operating system loads
- Has direct access to all hardware
- Cannot be replaced without firmware update

### 2. Hardware Integration

- Tightly integrated with specific hardware
- May use proprietary hardware features
- Optimized for particular system architecture
- Often vendor-specific implementation

### 3. Pre-Boot Environment

- Initializes before operating systems
- Sets up virtualization structures early
- Configures hardware virtualization features
- Establishes security foundations

---

## Primary Use Cases

### Mainframe Systems

Traditional mainframe environments where firmware-level virtualization provides:

- Logical partitioning (LPARs)
- Hardware resource isolation
- Maximum performance
- [[RAS|Reliability, Availability, Serviceability (RAS)]] features

### Large Enterprise Servers

Medium to large-sized servers requiring:

- Firmware-level partition management
- Strong isolation guarantees
- Mission-critical reliability
- Pre-OS security verification

---

## Examples

### IBM Z Series Mainframes

- PR/SM (Processor Resource/Systems Manager)
- Firmware-level LPAR support
- Manages logical partitions at firmware level
- Provides hardware-enforced isolation

### Oracle SPARC Systems

- Logical Domains (LDOMs) firmware support
- Hypervisor embedded in firmware
- Hardware-assisted virtualization
- Strong partition isolation

### IBM Power Systems

- PowerVM firmware
- Partition management in firmware
- Hardware resource virtualization
- Enterprise-grade RAS features

---

## Advantages

### Performance

- Minimal software overhead
- Direct hardware access
- Optimized for specific hardware
- No additional software layer

### Security

- Firmware-level isolation
- Executes in most privileged mode
- Hardware-enforced boundaries
- Difficult to compromise

### Reliability

- Integrated with firmware reliability features
- Hardware error detection
- Automatic recovery mechanisms
- Enterprise-grade availability

---

## Disadvantages

### Flexibility

- Vendor-specific implementation
- Difficult to modify or update
- Limited to supported hardware
- Proprietary features

### Portability

- Not portable across platforms
- Tied to specific hardware
- Vendor lock-in
- Migration challenges

### Accessibility

- Requires special hardware
- High cost
- Limited to enterprise environments
- Complex management

---

## Relationship to VMM Properties

Type-0 hypervisors satisfy [[Virtual Machine Monitor|VMM]] requirements:

### [[Equivalence Property]]

- Provides faithful hardware emulation
- Firmware-level correctness
- Hardware-validated behavior

### [[Efficiency Property]]

- Maximum performance
- Minimal overhead
- Hardware-accelerated operations

### [[Resource Control Property]]

- Firmware-enforced isolation
- Hardware partition boundaries
- Complete resource control

---
## Contrast with Other Types

|Feature|Type-0|[[Type-1 Hypervisor\|Type-1]]|[[Type-2 Hypervisor\|Type-2]]|
|---|---|---|---|
|Layer|Firmware|Bare-metal software|Application|
|Portability|Hardware-specific|Limited|High|
|Performance|Highest|High|Moderate|
|Flexibility|Low|Moderate|High|
|Cost|Very high|High|Low|

---

## Modern Developments

### UEFI Hypervisors

Some modern UEFI implementations include:

- Hypervisor functionality
- Pre-boot VM support
- Security verification
- Measured boot with virtualization

### Secure Boot Integration

Type-0 hypervisors increasingly integrate:

- Secure boot verification
- Trusted platform module (TPM) support
- Hardware root of trust
- Attestation capabilities

---

## Related Concepts

- [[Hypervisor]]: Type-0 is one category
- [[Type-1 Hypervisor]]: Software-based alternative
- [[Virtual Machine Monitor]]: Properties satisfied by Type-0
- [[Hardware Virtualization]]: Complementary hardware features
- [[RAS]]: Key benefit of Type-0 implementation