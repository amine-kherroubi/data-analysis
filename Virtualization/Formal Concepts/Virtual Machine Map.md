# Virtual Machine Map

A **virtual machine map** is a mathematical mapping between states of the real machine and states of the virtual machine system. It provides a formal characterization of the [[Equivalence Property|equivalence property]] required for virtualization.

---

## Formal Definition

A virtual machine map $f: C_r \to C_v$ is a one-to-one homomorphism with respect to all instruction sequences in $I$.

where:

- $C_r$ = set of real machine states (without VMM)
- $C_v$ = set of virtual machine states (with VMM)
- $I$ = set of all finite instruction sequences
- $f$ maps each real state to a corresponding virtual state

---

## Homomorphism Property

For any state $S \in C_r$ and any instruction sequence $e_i \in I$:

There exists instruction sequence $e_i'$ such that:

$f(e_i(S)) = e_i'(f(S))$

This diagram commutes:

```
Real machine:     S  ----e_i---->  e_i(S)
                  |                  |
                  f                  f  
                  |                  |
                  v                  v
Virtual machine: f(S) ---e_i'---> f(e_i(S))
```

---

## Standard Virtual Machine Map

The Popek-Goldberg model defines a standard VM map. For VMM occupying locations 0 to $k-1$ and VM occupying $k$ to $k+w-1$:

$f(E, M, P, R) = (E', M', P', R')$

where:

### Memory Transformation

```
E'[i + k] = E[i]           for i = 0 to w-1  (VM memory)
E'[i] = VMM_code           for i = 2 to k-1   (VMM code)
E'[1] = (s, first_VMM_addr, (0, q-1))         (trap entry PSW)
E'[0] = (M, P, R)          (saved VM state)
```

### Mode Transformation

```
M' = u  (user mode - VM always runs in user mode)
```

### Program Counter Transformation

```
P' = P  (unchanged - logical address preserved)
```

### Relocation-Bounds Transformation

If $R = (l, b)$ then:

```
R' = (l + k, b)  (shifted by VMM size)
```

---

## Inverse Map

Since $f$ is one-to-one, it has a left inverse $g$:

$g(f(S)) = S$

The inverse map $g$ is used in the equivalence proof to verify that virtual machine state corresponds correctly to real machine state.

---

## Properties

### 1. One-to-One (Injective)

Each real state maps to a unique virtual state:

- No information loss
- Inverse mapping exists
- States can be uniquely identified

### 2. Preserves Instruction Effects

For each real instruction sequence $e_i$:

- Corresponding virtual sequence $e_i'$ exists
- Same logical state transformation
- [[Equivalence Property|Equivalence]] maintained

### 3. Natural Correspondence

The mapping reflects the actual VMM implementation:

- Memory layout matches actual system
- Mode transformations match VMM operation
- State representation is realistic

---

## Multiple Virtual Machines

For systems hosting multiple VMs, the map can be extended:

$f_1, f_2, ..., f_n$ for $n$ VMs

Each map $f_i$ allocates VM $i$ to a distinct memory region with appropriate relocation-bounds.

---

## State Space Partitioning

The complete state space $C$ is partitioned:

- $C_r$ = states without VMM (for comparison)
- $C_v$ = states with VMM present

The partition criterion:

- $S \in C_v$ if VMM code is in memory and PSW in location 1 points to VMM dispatcher
- $S \in C_r$ otherwise

---

## Usage in Equivalence

The VM map formalizes [[Equivalence Property|equivalence]]:

**Programs execute equivalently if:**

Starting from $S_1 \in C_r$ and $S_1' = f(S_1) \in C_v$:

- If real machine halts in $S_2$
- Then virtual machine halts in $S_2' = f(S_2)$

---

## Acceptable Differences

The VM map allows two types of differences:

### Resource Availability

$f$ may map to smaller memory:

- Real machine state: $R = (l, b)$ with large $b$
- Virtual machine state: $R' = (l+k, b')$ with $b' < b$
- Represents VMM consuming resources

### Timing

The map is state-based, not time-based:

- No timing information in states
- Execution speed differences permitted
- Trap overhead not reflected in mapping

---

## Complexity

The VM map can be arbitrarily complex:

- Standard map is simple linear transformation
- Could include complex memory mappings
- Could represent elaborate state transformations
- Must maintain homomorphism property

---

## Constructability

The VM map must be:

- **Mathematically well-defined** (exists as a function)
- **Constructible by the VMM** (can be computed/implemented)
- **Efficiently computable** (reasonable overhead)

The standard map satisfies all three requirements.

---

## Related Concepts

- [[Equivalence Property]]: Formalized using VM map
- [[Virtual Machine Monitor]]: Implements the VM map
- [[Virtual Machine]]: State space $C_v$ created by map
- [[Relocation-Bounds Register]]: Key component in state transformation
- [[Popek-Goldberg Virtualization Theorem]]: Proves existence of valid VM map