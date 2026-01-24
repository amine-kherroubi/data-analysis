**Full virtualization (FV)** consists of running any OS as a guest in a virtual machine. For the end user, it is the simplest virtualization to set up and the most practical. However, it only allows you to virtualize OSes of the same architecture as that of the host system.

The full virtualization strategy consists of creating virtual environments/[[Virtual Machine|machines]] which are a copy of a physical machine (memory, disks, etc.). This method is based on two principles:

- the binary translation of instructions that the virtualized system kernel wishes to execute (e.g., translating a privileged instruction into an unprivileged instruction);
- the direct execution of instructions relating to user applications (non-privileged).

FV is supported by [[Type-2 (Application Implementation)|type-2 hypervisors]].