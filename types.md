# Types

So far, we’ve been working mainly with string and custom classes in our examples. However, TableGen offers a few other types and operators. This is already an improvement over JSON because in our case TableGen type-checks everything for us. Let’s quickly go through the available types. There aren’t many, and they’re fairly straightforward.

```c
// Used in TypeExample
class BinaryOp {}
def op : BinaryOp {}
  
// We don't really need multiple instances of TypeExample
// so we just put everything in a definition instead of
// having a class, and then instantiating it with def.
def TypeExample {
  // Bit type.
  bit bitV = 1; 
  
  // There’s no dedicated boolean type in TableGen.
  // Use `bit` for boolean values. However, TableGen
  // provides `true` and `false` keywords, which are
  // automatically converted to 1 and 0.
  bit falseV = false; // Converted to 0.
  
  // We will explain `assert` and `!eq` (which is bang-equal and not not-equal) 
  // later in the tutorial. For now, just know that they do exactly what you'd expect.
  assert !eq(falseV, 0), "Expected falseV to be 0";
  
  bit trueV = true; // Converted to 1.
  assert !eq(trueV, 1), "Expected trueV to be 1";
  
  bit alsoTrueV = 1; // Cast into a bit.
  assert !eq(alsoTrueV, true), "Expected alsoTrueV to be true";
  assert !eq(alsoTrueV, 1), "Expected alsoTrueV to be 1";
  
  // Integer type.
  int intV = -1;
  
  // String type.
  string stringV = "stringVal";
  
  // `bits` type: a sequence of bits.
  bits<4> bitsV1 = {0, 1, 0, 1}; // Set individual bits.
  bits<4> bitsV2 = 5; // Or set them all at once.
  assert !eq(bitsV1, bitsV2), "Expected bitsV1 and bitsV2 to be equal"; 
  
  // Lists of any type, like in C++.
  list<int> listV = [10, 20, 30, 40];
  list<string> stringList = ["a", "b", "c", "d"];
  
  // We’ll cover `dag` later.
  dag dagV = !dag(op, [10, 20], ["op1", "op2"]);
  
  // Uninitialized values can be specified using a question mark (?).
  // The following two definitions are equivalent.
  string uName1;
  string uName2 = ?;
    
  // ! (bang) operators won't work on uninitialized data. We will learn them in a bit.
  // The following line won't work:  
  // assert !eq(uName1, uName2), "Expected uName1 and uName2 to be uninitialized";
  // Uncommenting will throw the following error:
  // error: assert condition must be of type bit, bits, or int.
}
```

{% hint style="info" %}
TableGen only supports decimal types i.e. floating point numbers won’t even parse. While this might seem like a gross omission, I haven’t seen any instances within the compiler construction framework where I wished I had floating point support. Having said that, if you absolutely MUST use floating point numbers in your code generation, you could represent them as strings and allow the backend to parse said strings.
{% endhint %}
