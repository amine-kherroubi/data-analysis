# Type-1 (System Implementation)

**Type-1 hypervisors** (also called bare-metal) consist of a base software designed to provide virtualization at the OS level, as do **VMware ESXi**, **Nutanix AHV** and **Citrix XenServer**. They run directly on a hardware platform.

Type-1 [[Hypervisor|hypervisors]] use [[Hardware Virtualization|hardware virtualization]] and [[Para-Virtualization|para-virtualization]].

A particular case is that of a general purpose OS providing type-1 [[Virtual Machine|virtual machine]] management functions. For instance, **Linux KVM**, which turns a complete Linux kernel into a hypervisor, is also considered a type-1 hypervisor. Microsoft **Hyper-V** is a role above the Windows kernel, but has type-1 functionality.