# Virtualization

**Virtualization** is a framework for dividing the resources of a computer into multiple execution environments by applying concepts such as hardware and software partitioning, time-sharing, partial or complete machine simulation, emulation, and resource sharing.

---

## Core Principle

The fundamental principle of virtualization is **resource sharing** through abstraction. It creates a virtual (rather than actual) version of computing resources such as processors, memory, storage devices, or network resources.

---

## Essential Properties

For virtualization to be effective, it must satisfy two fundamental properties:

1. **Isolation**: Each virtualized environment must be independent and cannot interfere with others. Programs running in one [[Virtual Machine|virtual machine]] cannot affect the state or resources of another.
2. **Transparency**: The execution of applications and services within a virtualized environment must be functionally identical to execution on physical hardware, with only minor acceptable differences in timing and resource availability.

---

## Key Concepts

### Abstraction

Virtualization abstracts computing resources to:

- Hide physical characteristics of resources, simplifying interactions with systems, applications, and users
- Present one physical resource as multiple logical resources (partitioning)
- Present multiple physical resources as a single logical resource (aggregation)

### Resource Distribution

Physical resources are distributed through virtualization tools, enabling multiple virtual environments to share them simultaneously while maintaining isolation.

### Virtual Environments

The creation of isolated execution contexts that appear to have dedicated resources, managed by a [[Virtual Machine Monitor|hypervisor or virtual machine monitor]].

---

## Related Concepts

- [[Virtual Machine]]: The actual virtualized execution environment
- [[Virtual Machine Monitor]]: The software layer that manages virtualization
- [[Full Virtualization]]: Complete hardware emulation approach
- [[Para-Virtualization]]: Cooperative virtualization approach
- [[Hardware Virtualization]]: Processor-assisted virtualization
- [[Containerization]]: OS-level virtualization