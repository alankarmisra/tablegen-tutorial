# Multi-classes

This is where we depart from the OOP-ness of TableGen classes. Whereas a class allows you to generate a def, a multi-class allows you to generate multiple defs. That’s pretty much it. Why do we need these? I’ve only ever seen them being useful for low-level code where you want to generate defs for different architectures. Let’s see some simplified examples:

```c
// For now we use an empty instruction.
class Instruction{}
 
// Multiclass will take a single instruction
// and then create multiple records out of it.
multiclass InstructionM {
  // We want to generate two different variations of each instruction.
  def _amd : Instruction{}
  def _intel : Instruction{}
}
 
// You can "invoke" a multiclass using defm
defm ADD : InstructionM; 
// If you see the output, you will see some defm weirdness.
// So let's go through the generation process step by step.
//
// defm ADD : InstructionM invokes the multiclass.
//
// TableGen then generates new records as expected. 
// But the names of the records are prepended with the variable name.
// So instead of generating records _amd, and _intel,
// it will generate 
// ADD_amd, and 
// ADD_intel
 
defm MUL : InstructionM;
// Similarly the above will generate 
// MUL_amd, and 
// MUL_intel
```

which outputs:

```c
------------- Classes -----------------
class Instruction {
}
------------- Defs -----------------
def ADD_amd {   // Instruction
}
def ADD_intel { // Instruction
}
def MUL_amd {   // Instruction
}
def MUL_intel { // Instruction
}
```

Here’s a more involved example.

```c
// Define a class for instructions with a 4-bit opcode 
// and an assembly name.
class InstructionWithOpcode {
   bits<4> opcode;
   string asm;
}
 
// Define a multiclass that generates two variations 
// of each instruction with the specified opcode and assembly name.
multiclass InstructionWithOpcodeM<bits<4> OpcodeA, bits<4> OpcodeB, string Asm> {
  def _amd : InstructionWithOpcode {
    bits<4> opcode = OpcodeA;
    string asm = Asm;
  }
  def _intel : InstructionWithOpcode {
    bits<4> opcode = OpcodeB;
    string asm = Asm;
  }
}
 
// Invoke the multiclass to generate the records with 
// different opcodes and assembly names.
defm ADD : InstructionWithOpcodeM<0b0000, 0b0001, "add">;
// This will generate records:
// ADD_amd with opcode 0b0000 and asm "add"
// ADD_intel with opcode 0b0001 and asm "add"
 
defm SUB : InstructionWithOpcodeM<0b0010, 0b0011, "sub">;
// This will generate records:
// SUB_amd with opcode 0b0010 and asm "sub"
// SUB_intel with opcode 0b0011 and asm "sub"
```

And here's the output:

```c
------------- Classes -----------------
class InstructionWithOpcode {
  bits<4> opcode = { ?, ?, ?, ? };
  string asm = ?;
}
------------- Defs -----------------
def ADD_amd {   // InstructionWithOpcode
  bits<4> opcode = { 0, 0, 0, 0 };
  string asm = "add";
}
def ADD_intel { // InstructionWithOpcode
  bits<4> opcode = { 0, 0, 0, 1 };
  string asm = "add";
}
def SUB_amd {   // InstructionWithOpcode
  bits<4> opcode = { 0, 0, 1, 0 };
  string asm = "sub";
}
def SUB_intel { // InstructionWithOpcode
  bits<4> opcode = { 0, 0, 1, 1 };
  string asm = "sub";
}
```
