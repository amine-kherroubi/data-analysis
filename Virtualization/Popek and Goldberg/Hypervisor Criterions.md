# Hypervisor Criterions

## Equivalence

Any program run under the [[Hypervisor|hypervisor]] should exhibit an effect identical with that demonstrated if the program had been run on the original machine directly, with the possible exception of differences caused by the availability of system resources and differences caused by timing dependencies.

---

## Effectiveness

A statistically dominant subset of the virtual processor’s instructions can be executed directly by the real processor, with no software intervention by the hypervisor. [[Hardware Virtualization|Hardware virtualization]] makes it possible to better respond to this constraint. This criterion excludes purely software systems as simulation and emulation.

---

## Resource Control

The hypervisor must have complete control of the resources. It is not possible for a program running under it to access any resource not explicitly allocated and it is possible under certain circumstances for the hypervisor to regain control of resources already allocated.