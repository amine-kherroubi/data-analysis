# Dispatcher

The **dispatcher** (also called **regulator**) is the top-level control module of a [[Hypervisor|hypervisor]] or [[Virtual Machine Monitor|virtual machine monitor]]. It serves as the entry point for all VMM interventions.

---

## Function

The dispatcher's primary responsibility is to receive control when the hardware traps and determine which VMM component should handle the trapped condition. It acts as a routing mechanism that:

1. Receives control from hardware trap mechanisms
2. Analyzes the cause of the trap
3. Invokes either the [[Allocator|allocator]] or appropriate [[Interpreter Routines|interpreter routine]]
4. Returns control to the [[Virtual Machine|virtual machine]] after handling completes

---

## Entry Points

The dispatcher's initial instruction is placed at the location to which hardware traps occur. In the Popek-Goldberg model, this is the address specified by the program counter in location 1 of memory.

Some architectures trap to different locations depending on trap type (privileged instruction trap, memory trap, etc.). In such systems, the dispatcher may have multiple entry points, each handling a specific trap category.

---

## Control Flow

When a [[Sensitive Instruction|sensitive instruction]] or other trappable condition occurs:

1. Hardware saves current state (PSW) to location 0
2. Hardware loads new state (PSW) from location 1, transferring control to dispatcher
3. Dispatcher examines saved state and trap cause
4. Dispatcher invokes appropriate VMM component
5. After handling, dispatcher restores virtual machine state and returns control

---

## Mode of Execution

The dispatcher typically runs in supervisor mode with full memory access (relocation register set to 0, bounds set to maximum). This allows it to:

- Access VMM code and data structures
- Access virtual machine state
- Manage memory allocation
- Simulate privileged operations

---

## Related Concepts

- [[Hypervisor]]: Contains the dispatcher as a component
- [[Allocator]]: Invoked by dispatcher for resource management
- [[Interpreter Routines]]: Invoked by dispatcher to simulate instructions
- [[Trap Mechanism]]: Hardware mechanism that transfers control to dispatcher
- [[Privileged Instruction]]: Instructions that cause traps to the dispatcher