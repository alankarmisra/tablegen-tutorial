# Terminology

TableGen specs primarily contain two types of records: abstract records called classes, and concrete records, confusingly called records in the official documentation (I’ll explain the abstract/concrete bifurcation in a minute). To complicate things further, when you look at the TableGen output, it refers to concrete records as “Defs” since we define a concrete record using the def keyword 🙄 But the intended meaning should usually be clear from the context in the documentation. To keep things simple, I’ll refer to concrete records as defs and use records to refer to both abstract and concrete records..

The abstract/concrete distinction is similar to the difference between classes and instances in object-oriented programming. A quick example will make this clearer:

```c
/// sample.td
// class (defined using the `class` keyword)
class ArithmeticOperator {
  string mnemonic; // field defined with its type
}
 
// record (defined using the `def` keyword : creates an instance of ArithmeticOperator)
def AddOp : ArithmeticOperator {
  let mnemonic = "add"; // field initialized using `let`
}
 
def MulOp : ArithmeticOperator {
  let mnemonic = "mul"; // field initialized using `let`
}
```

In this example, ArithmeticOperator is the abstract definition (the class), while AddOp and MulOp are concrete instances (the defs).

We run this through the llvm-tblgen command-line utility (I’m using the homebrew version), without specifying any backend like so:

```bash
/opt/homebrew/opt/llvm/bin/llvm-tblgen sample.td
```

we get the following output on the console:

```c
------------- Classes -----------------
class ArithmeticOperator {
  string mnemonic = ?;
}
------------- Defs -----------------
def AddOp {     // ArithmeticOperator
  string mnemonic = "add";
}
def MulOp {     // ArithmeticOperator
  string mnemonic = "mul";
}
```
