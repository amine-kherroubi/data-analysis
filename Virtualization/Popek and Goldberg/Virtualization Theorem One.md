# Virtualization Theorem One

For any conventional third generation computer, **a hypervisor may be constructed if the set of sensitive instructions for that computer is a subset of the set of privileged instructions**.

To build a hypervisor for a particular architecture, it is necessary that instructions that could compromise the proper functioning of this hypervisor (configuration-sensitive instructions) are trapped and that control is given to the hypervisor. This guarantees the resource control criterion. Unprivileged instructions must be executed without intervention from the hypervisor in order to guarantee the efficiency criterion.

---

## Particular Case

If configuration-sensitive instructions are not part of the privileged instructions, then, under the efficiency criterion, they will be executed without intervention from the hypervisor and therefore the resource control criterion is not met.