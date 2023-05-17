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

# Custom instructions exclusive to SPWASM
- `clear` (resets the stack)
- `clearmem` (resets memory)
- `drawpix` (draws pixels on display) (arguments: [...RGB values here...], [X position of pixel, Y position of pixel])

# Files
## Compile-time
[spwasm.spwn](spwasm.spwn) The compile-time version of SPWASM, which does not run in GD

## Runtime
[runtime/runtime_spwasm_countersonly.spwn](runtime/runtime_spwasm_countersonly.spwn) - The runtime version of SPWASM, made for very simple programs that do not require memory or a display

[runtime/runtime_spwasm_display.spwn](runtime/runtime_spwasm_display.spwn) - The full runtime version of SPWASM, which includes a 10x10 display for drawing images and memory which can be used for storing integers outside of stack

# Usage
Compile-time:
```rs
vm = @stack_vm::create()
vm.setMemory(1) // set pages of memory, 1 page = 64kb

result = vm.run([
  { instruction: "local.get", args: ["x"]}
  { instruction: "i32.const", args: [5]},
  { instruction: "i32.add", args: []}
], { x: 15 }); // runs a function that takes X as a parameter and adds five

$.print(result) // 20
```

Runtime: 
```rs
let vm = @stack_vm::create();

let instructions = [
    { opcode: "drawpix", args: [[255, 255, 255], [5, 5]]},
    { opcode: "i32.const", args: [5]},
    { opcode: "i32.const", args: [3]},
    { opcode: "i32.add" },
];

vm.run(newinstr);
```
