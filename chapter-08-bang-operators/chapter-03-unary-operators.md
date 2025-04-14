# Unary operators

```tablegen
#ifndef UNARYOPS
#define UNARYOPS
 
// Notice we don't have the # sign in front of the include like we do in C++
include "setup.td"
 
def UnaryOps {
  // !repr(value)
  // Represents value as a string. 
  // String format for the value might change over TableGen versions,
  // so don't rely on it being exact.
  // Intended for debugging purposes only.
  string addOp_s = !repr(addOp); 
  // "addOp {       // Op
  //  string name = "add";
  // }
  //";
  
  // Base 2 log 
  int logtwo_v = !logtwo(2); // 1
  assert !eq(logtwo_v, 1), errorStr;
    
  // logical shift performed on a 64-bit integer
  // value undefined if count &#x3C; 0 or count > 63
  // The shift function naming convention is a bit odd.
  // SHift Left logical -> shl
  int shl_v = !shl(1, 3); // 8
  assert !eq(shl_v, 8), errorStr;
 
  // Shift Right Logical -> srl
  int srl_v = !srl(8, 3); // 1
  assert !eq(srl_v, 1), errorStr; 
 
  // Shift Right Arithmetic -> sra
  int sra_v = !sra(-8, 3); // -1
  assert !eq(sra_v, -1), errorStr; 
    
  // !not(a)
  bit not_t = !not(true); // 0
  assert !eq(not_t, 0), errorStr;
 
  bit not_f = !not(false); // 1
  assert !eq(not_f, 1), errorStr;
 
  // works on 0 and 1 like it would on true/false
  bit not_1 = !not(1); // 0
  assert !eq(not_1, 0), errorStr;
 
  // any non-zero number (positive or negative) is true. 
  // so !not(non_zero_number) = 0 ie false.
  bit not_2 = !not(-2); 
  assert !eq(not_2, 0), errorStr;
  
  // !empty(a)
  // This operator produces 1 if the string, list, or DAG a is empty; 0 otherwise. 
  // A dag is empty if it has no arguments; the operator does not count.
  bit empty_string = !empty("");  
  assert empty_string, errorStr; 
  
  // size(a)
  int size_hello = !size("Hello, World!"); // 13
  assert !eq(size_hello, 13), errorStr;
 
  int size_list = !size([1, 2, 3]); // 3
  assert !eq(size_list, 3), errorStr;
}
 
#endif // UNARYOPS
```
