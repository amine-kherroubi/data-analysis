**Para-virtualization (PV)** is a technique in which the guest OS is modified to work in cooperation with the hypervisor to optimize performance. It increases performance and stability but requires kernel adaptation of guest systems and is limited to free systems (e.g. Linux).

The para-virtualization method involves modifying the kernel of the virtualized OS in order to replace non-virtualizable instructions with hypercalls that will communicate directly with the virtual layer of the hypervisor. 

PV is supported by [[Type-1 (System Implementation)|types-1]] and [[Type-2 (Application Implementation)|type-2]] hypervisors.