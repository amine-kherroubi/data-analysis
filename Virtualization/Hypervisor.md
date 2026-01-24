# Hypervisor

An **hypervisor** (or **virtual machine manager**) is a lightweight software layer (compared to an OS) which allows maximum physical resources to be allocated to [[Virtual Machine|virtual machines]].

---

## Components

An hypervisor has three components (in categories):

- A **dispatcher**, also known as regulator: it can be considered as the highest level control module of the hypervisor. Its role is to give control to one of the modules of the second or third category.
- An **allocator** whose role is to determine which resources should be allocated to virtualized applications. The allocator does not give the same resource simultaneously to two distinct virtual environments. The regulator will call the allocator each time a virtual environment attempts to execute a privileged instruction which would have the effect of modifying the resources allocated to this virtual environment.
- **Interpreters**: to each of the privileged instructions (with the exception of those which are supported by the allocator), we will associate an **interpretation routine (RI)**. The role of these routines is to simulate the result of privileged instructions that are trapped.