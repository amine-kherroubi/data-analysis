# Containerization

**Containerization** is an operating system-level virtualization technique that creates isolated execution environments (containers) by leveraging kernel features, without requiring a [[Hypervisor|hypervisor]] or separate guest operating systems.

---

## Definition

Containerization virtualizes at the **kernel level** of the host operating system, creating lightweight, isolated environments that share the host OS kernel while maintaining separation of processes, filesystems, and network stacks.

---

## Architecture

```
┌───────────────────────────────────────────────┐
│  Container 1  │  Container 2  │  Container 3  │  Isolated user spaces
├───────────────┴───────────────┴───────────────┤
│         Container Engine (Docker, etc.)       │  Management layer
├───────────────────────────────────────────────┤
│         Host Operating System Kernel          │  Shared kernel
├───────────────────────────────────────────────┤
│         Physical Hardware                     │  
└───────────────────────────────────────────────┘
```

**Key difference from VMs:** No hypervisor layer, no guest OS kernels.

---

## Core Concept

Containerization differs fundamentally from [[Virtual Machine|virtual machine]] virtualization:

### Traditional VM Approach

- Full OS stack per VM
- [[Hypervisor|Hypervisor]] manages multiple guest kernels
- Hardware virtualization or software emulation
- Heavy resource usage

### Container Approach

- Shared host OS kernel
- Isolated user spaces
- OS-level isolation primitives
- Lightweight resource usage

---

## Kernel-Level Virtualization Features

Containers rely on operating system kernel features:

### Linux Kernel Features

**Namespaces** - Isolation of system resources:

- **PID namespace**: Process ID isolation
- **Network namespace**: Network stack isolation
- **Mount namespace**: Filesystem mount points
- **UTS namespace**: Hostname and domain name
- **IPC namespace**: Inter-process communication
- **User namespace**: User and group ID mapping
- **Cgroup namespace**: Control group view isolation

**Control Groups (cgroups)** - Resource limitation:

- CPU time allocation
- Memory limits
- Disk I/O bandwidth
- Network bandwidth
- Device access control

**Union File Systems** - Layered filesystem:

- OverlayFS, AUFS, Btrfs
- Copy-on-write layers
- Efficient storage sharing
- Fast container creation

**Linux Security Modules**:

- SELinux
- AppArmor
- Seccomp
- Capabilities

---

## Characteristics

### 1. No Hypervisor

- **Hypervisor does not exist** in containerization
- Direct kernel features provide isolation
- No virtualization layer
- No hardware emulation

### 2. Shared Kernel

- All containers share the host OS kernel
- Same kernel version for all containers
- Kernel compatibility required
- Cannot run different OS families (Linux container cannot run Windows natively)

### 3. Lightweight

- Minimal overhead compared to VMs
- Fast startup times (milliseconds vs. seconds)
- Low memory footprint
- High density (100s-1000s per host)

### 4. Portable

- Container images are portable
- Consistent environment across systems
- "Build once, run anywhere" (with compatible kernel)
- Standard image formats (OCI)

---

## Container Engines

Container engines manage container lifecycle:

### Docker

- Most popular container platform
- Docker Engine manages containers
- Docker Hub for image registry
- Comprehensive tooling ecosystem

### containerd

- Industry-standard container runtime
- Used by Docker, Kubernetes
- CNCF graduated project
- OCI-compliant

### Podman

- Daemonless container engine
- Docker-compatible CLI
- Rootless containers
- Red Hat sponsored

### CRI-O

- Lightweight container runtime
- Specifically for Kubernetes
- OCI-compliant
- Minimal feature set

---

## Container Orchestration

Managing containers at scale:

### Kubernetes

- Industry-standard orchestration
- Automated deployment and scaling
- Self-healing
- Service discovery and load balancing
- Rolling updates

### Docker Swarm

- Docker-native orchestration
- Simpler than Kubernetes
- Integrated with Docker
- Good for smaller deployments

### Apache Mesos / Marathon

- General-purpose cluster manager
- Supports containers and other workloads
- Large-scale deployments
- Mature technology

---

## Advantages

### Performance

- **Near-native performance**
- Minimal overhead (1-3%)
- No hypervisor layer
- Direct system calls to kernel
- Fast I/O operations

### Efficiency

- **Extremely lightweight**
- Megabytes vs. gigabytes for VMs
- Seconds startup vs. minutes for VMs
- High container density
- Efficient resource utilization

### Portability

- Consistent environments
- Infrastructure-agnostic
- Easy migration
- Standardized packaging

### Speed

- Rapid deployment
- Fast scaling
- Quick iteration
- CI/CD optimization

### DevOps Integration

- Infrastructure as Code
- Version-controlled configurations
- Reproducible builds
- Microservices architecture

---

## Disadvantages

### Security Concerns

- **Shared kernel attack surface**
- Container breakout risks
- Less isolation than VMs
- Kernel vulnerabilities affect all containers
- Requires careful security hardening

### OS Limitation

- **Same OS family required**
- Linux containers need Linux kernel
- Cannot run Windows containers on Linux (without VM)
- Cannot run different kernel versions

### Complexity for Stateful Applications

- Persistent data management challenges
- Volume mounting complications
- Database containerization complexities
- State management in distributed systems

### Learning Curve

- New paradigm for traditional sysadmins
- Orchestration complexity
- Networking concepts
- Storage management

---

## Isolation Mechanisms

Containers provide isolation through:

### Process Isolation

- Separate process trees per container
- PID namespaces
- No cross-container process visibility
- Resource limits via cgroups

### Filesystem Isolation

- Separate filesystem namespaces
- Read-only container images
- Writable container layers
- Volume mounts for persistence

### Network Isolation

- Virtual network interfaces
- Separate network namespaces
- Software-defined networking
- Port mapping

### Resource Isolation

- CPU share limits
- Memory limits
- Disk I/O limits
- Network bandwidth controls

---

## Not a Virtual Machine

Critical differences from [[Virtual Machine|VMs]]:

|Aspect|Containers|Virtual Machines|
|---|---|---|
|Kernel|Shared|Separate per VM|
|Isolation|Process-level|Hardware-level|
|Overhead|Minimal (1-3%)|Moderate (5-15%)|
|Startup|Milliseconds|Seconds to minutes|
|Size|Megabytes|Gigabytes|
|Density|100s-1000s|10s-100s|
|[[Hypervisor]]|**None**|Required|
|OS variety|Same family|Any OS|

---

## Security Best Practices

### Hardening Containers

- Run as non-root users
- Minimal base images
- Read-only root filesystems
- Capability dropping
- Seccomp profiles
- SELinux/AppArmor policies

### Image Security

- Scan for vulnerabilities
- Sign and verify images
- Use trusted registries
- Regular updates
- Minimal layers

### Runtime Security

- Network policies
- Resource quotas
- Pod security policies (Kubernetes)
- Runtime threat detection
- Audit logging

---

## Hybrid Approaches: Containers + VMs

Modern architectures often combine both:

### VM Isolation + Container Efficiency

- Containers run inside VMs
- Cloud providers use this model (AWS Fargate, Google Cloud Run)
- VM provides strong isolation boundary
- Containers provide efficient workload packaging

### Kata Containers / gVisor

- Lightweight VMs that look like containers
- Hardware virtualization for container isolation
- Container interface, VM security
- Best of both worlds

---

## Use Cases

### Ideal for Containers

- Microservices architectures
- Stateless applications
- CI/CD pipelines
- Development environments
- Scalable web applications
- Batch processing

### Better Suited for VMs

- Different OS requirements
- Strong isolation needs
- Legacy monolithic applications
- Stateful databases (often)
- Full OS control needed

---

## Relationship to VMM Properties

Containers do not satisfy [[Virtual Machine Monitor|VMM]] properties in the traditional sense:

### Not a VMM

- No [[Equivalence Property|equivalence]] - cannot run arbitrary OS
- Modified [[Efficiency Property|efficiency]] - even better (1-3% overhead)
- Different [[Resource Control Property|resource control]] - kernel-level, not hardware-level

Containers represent a different virtualization paradigm optimized for application isolation rather than full system virtualization.

---

## Evolution and Standards

### OCI (Open Container Initiative)

- **Image Specification**: Standard container image format
- **Runtime Specification**: Container runtime standard
- **Distribution Specification**: Image distribution protocol

### CNCF (Cloud Native Computing Foundation)

- Kubernetes (orchestration)
- containerd (runtime)
- Prometheus (monitoring)
- Envoy (service mesh)

---

## Related Concepts

- [[Virtualization]]: Containerization is one approach
- [[Virtual Machine]]: Alternative to containers
- [[Hypervisor]]: Not used in containerization
- [[Type-1 Hypervisor]]: May host containerized workloads
- [[Operating System-Level Virtualization]]: Formal term for containerization