# Virtual Machine Monitor

A **virtual machine monitor (VMM)**, also called a **hypervisor**, is a control program that creates and manages [[Virtual Machine|virtual machines]] by providing an environment for programs that is essentially identical to the original machine.

---

## Essential Characteristics

According to Popek and Goldberg, a VMM must satisfy three essential properties:

### 1. Equivalence (Fidelity)

Any program running under the VMM should exhibit behavior identical to that demonstrated if the program had been run on the original machine directly, with two acceptable exceptions:

- Differences caused by **resource availability** (e.g., less memory available)
- Differences caused by **timing dependencies** (due to VMM intervention)

This requirement distinguishes VMMs from traditional time-sharing operating systems.

### 2. Efficiency (Performance)

A statistically dominant subset of the virtual processor's instructions must be executed directly by the real processor with no software intervention by the VMM.

This requirement excludes:

- Traditional emulators
- Complete software interpreters (simulators)
- Systems where most instructions require interpretation

### 3. Resource Control (Safety)

The VMM must have complete control of system resources, meaning:

- Programs cannot access resources not explicitly allocated to them
- The VMM can regain control of allocated resources under certain circumstances

Resources include memory, peripherals, and other system components (though not necessarily processor time in the simplest form).

---

## Formal Model

A VMM creates a mapping between real machine states and virtual machine states. For state $S$ in the real machine:

$$f(S) = S'$$

where $S'$ is the corresponding state in the virtual machine system, and $f$ is a **virtual machine map** that preserves the effect of instruction sequences.

---

## Three-Component Architecture

A VMM consists of three functional components:

1. **[[Dispatcher]]**: The top-level control module that decides which component to invoke
2. **[[Allocator]]**: Manages resource allocation decisions
3. **[[Interpreter Routines]]**: One routine per [[Privileged Instruction|privileged instruction]] to simulate its effect

---

## Related Concepts

- [[Virtual Machine Monitor]]: Synonym for VMM
- [[Virtual Machine]]: The environment created by the VMM
- [[Equivalence Property]]: First essential characteristic
- [[Efficiency Property]]: Second essential characteristic
- [[Resource Control Property]]: Third essential characteristic
- [[Privileged Instruction]]: Instructions that require VMM intervention
- [[Sensitive Instruction]]: Instructions that affect resources or depend on configuration