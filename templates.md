# Templates

Not unlike C++, in TableGen, templates allow us to pass initialization values to classes. Let’s dive into an example:

```clike
// Base class for all Traits (shared behaviour among classes/instances)
class Trait {}
 
// Operator class takes two template variables mnemonicString and traitItem.
// These variables are stored in the class.
// Initializing variables to '?' means they currently hold an undefined value.
class Operator<string mnemonicString = ?, Trait traitItem = ?> {
  string mnemonic = mnemonicString;
  Trait trait = traitItem;
}
 
// A concrete record
def ArithmeticOperatorTrait : Trait {}
 
// A derived class
class ArithmeticOperator<string mnemonic, Trait trait = ArithmeticOperatorTrait> 
  : Operator<mnemonic, trait>;
 
 
// Concrete records
def AddOp : ArithmeticOperator<"add">; 
def MulOp : ArithmeticOperator<"mul">;
```

When you run this file through TableGen, it generates the following records. Notice that classes and defs are listed in alphabetical order, not hierarchical. Also, TableGen flattens and merges all the fields from parent classes and templates into the final def. This way, each def stands alone without referencing its parent, except in the comments.

```clike
------------- Classes -----------------
class ArithmeticOperator<string ArithmeticOperator:mnemonic = ?, 
 Trait ArithmeticOperator:trait = ArithmeticOperatorTrait> {    // Operator
  string mnemonic = ArithmeticOperator:mnemonic;
  Trait trait = ArithmeticOperator:trait;
}
class Operator<string Operator:mnemonicString = ?, 
 Trait Operator:traitItem = ?> {
  string mnemonic = Operator:mnemonicString;
  Trait trait = Operator:traitItem;
}
class Trait {
}
------------- Defs -----------------
def AddOp {     // Operator ArithmeticOperator
  string mnemonic = "add";
  Trait trait = ArithmeticOperatorTrait;
}
def ArithmeticOperatorTrait {   // Trait
}
def MulOp {     // Operator ArithmeticOperator
  string mnemonic = "mul";
  Trait trait = ArithmeticOperatorTrait;
}
```
