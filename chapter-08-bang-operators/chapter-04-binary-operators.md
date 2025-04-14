# Binary operators

```tablegen
/// binaryops.td
 
#ifndef BINARYOPS
#define BINARYOPS
 
include "setup.td"
  
def BinaryOps {
  // !add(a, b, ...) // any number of operands
  int add_v = !add(10, 50, -59);  // 1
  assert !eq(add_v, 1), errorStr;
   
  // Binary arithmetic operators of the form
  // !op(a, b)
  int sub_v = !sub(100, 99); // 1
  assert !eq(sub_v, 1), errorStr;
  
  int mul_v = !mul(2, 3, 4); // 24
  assert !eq(mul_v, 24), errorStr;
  
  int div_v = !div(100, 100);  
  assert !eq(div_v, 1), errorStr;
   
  // Binary logical operators of the form
  // !op(a, b)
  bit eq_v = !eq(10, 10); // true, converted to 1
  assert eq_v, errorStr;
  
  bit ne_v = !ne(10, 11); // 1
  assert ne_v, errorStr;
  
  bit gt_v = !gt(11, 10); // 1
  assert gt_v, errorStr;
  
  bit ge_v = !ge(11, 10); // 1
  assert ge_v, errorStr;
  
  bit lt_v = !lt(10, 11); // 1
  assert lt_v, errorStr;
  
  bit le_v = !le(10, 11); // 1
  assert le_v, errorStr;
     
  // Logical operators of the form
  // !op(a, b, ...)
  bit and_v = !and(true, true, true); // 1
  assert and_v, errorStr;
  
  bit or_v = !or(false, false, false, true); // 1
  assert or_v, errorStr;
  
  bit xor_v = !xor(true, false, false, false, false); // 1
  assert xor_v, errorStr;
}
 
#endif // BINARYOPS
```
