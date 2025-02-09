# Setting up our examples

Some basic setup code for our operator examples. We put this in a separate file setup.td and include it in our code samples as necessary.

```c
/// setup.td
 
#ifndef SETUP
#define SETUP
 
// Some simple classes and defs to be used in the operator examples.
class Op {
  string name;
}
  
def addOp : Op { let name = "add"; }
def mulOp : Op { let name = "mul"; }
def divOp : Op { let name = "div"; }
 
// defvar is to define a global variable
// We just use this as a standin for errors
// that would have been reported if our
// assertions failed. 
defvar errorStr = "error";
 
#endif // SETUP
```
