# SPWASM
Attempting to run WASM/WAST/WAT instructions in SPWN to run C/C++/Rust code in SPWN

# WASM instructions implemented
- `i32.add`
- `i32.mul`
- `i32.const`
- `i32.store`
- `i32.load`
- `drop`
- `if`
- `get_local`

# Custom instructions exclusive to SPWASM
- `clear` (resets the stack)
- `clearmem` (resets memory)
- `drawpix` (draws pixels on display) (arguments: [...RGB values here...], [X position of pixel, Y position of pixel])

# Files
## Compile-time
[spwasm.spwn](spwasm.spwn) The compile-time version of SPWASM, which does not run in GD

## Runtime
[runtime/runtime_spwasm_countersonly.spwn](runtime/runtime_spwasm_countersonly.spwn) - The runtime version of SPWASM, made for very simple programs that do not require memory or a display. This is usually the least group-expensive version of SPWN, but is way more limited, and has no memory or support for input.

[runtime/runtime_spwasm_display.spwn](runtime/runtime_spwasm_display.spwn) - The runtime version of SPWASM, which includes a 10x10 display for drawing images and memory which can be used for storing integers outside of stack. This is the middle-sized version, which can be quite a bit more group-expensive, but it is in exchange for a display and memory.

[runtime/runtime_spwasm_input.spwn]([runtime/runtime_spwasm_input.spwn) - The full runtime version of SPWASM, which includes the display, memory, and a keypad for inputting integers into the program before executing it. Biggest & most group expensive version.

# Usage
Compile-time:
```rs
vm = @stack_vm::create()
vm.setMemory(1) // set pages of memory, 1 page = 64kb

result = vm.run([
  { opcode: "local.get", args: ["x"]}
  { opcode: "i32.const", args: [5]},
  { opcode: "i32.add", args: []}
], { x: 15 }); // runs a function that takes X as a parameter and adds five

$.print(result) // 20
```

Runtime: 
```rs
vm = @stack_vm::create()
// no ".setMemory" because it is already set to 11 bytes, which seems really small but is enough for the runtime VM
vm.setSpeed(0.1); // set execution speed (in seconds)
vm.run([
    { opcode: "drawpix", args: [[255, 255, 255], [5, 5]]},
    { opcode: "get_local", args: [0]} // instead of adding in a dictionary to `vm.run` for parameters, you can use `get_local` to push a parameter to the stack. 
    { opcode: "i32.const", args: [3]}, // input is usually entered by user before-hand, unless you are not using the SPWASM script with inputs
    { opcode: "i32.add" },
]);
```
