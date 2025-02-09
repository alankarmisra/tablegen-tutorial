# String operators

```c
/// stringops.td
#ifndef STRINGOPS
#define STRINGOPS
 
include "setup.td"
 
def StringOps {
  // strconcat(str1, str2, ...)
  string str_c = !strconcat("Hello, ", "World!"); // "Hello, World!"
  assert !eq(str_c, "Hello, World!"), errorStr;
  
  // !subst(target, repl, value)
  // replace target with repl in value
  string str_s = !subst("World", "Universe", "Hello World World!"); // "Hello Universe Universe!"
  assert !eq(str_s, "Hello Universe Universe!"), errorStr;
  
  // !tolower(a)
  string str_t = !tolower("Hello World"); // "hello world"
  assert !eq(str_t, "hello world"), errorStr;
  
  // !toupper(a)
  string str_u = !toupper("Hello World"); // "HELLO WORLD"
  assert !eq(str_u, "HELLO WORLD"), errorStr;
  
  // !find(string1, string2[, start])
  int index1 = !find("Hello World", "World"); // 6
  assert !eq(index1, 6), errorStr;
 
  int index2 = !find("Hello World", "World", 6); // 6
  assert !eq(index2, 6), errorStr;
 
  int index3 = !find("Hello World", "World", 7); // -1
  assert !eq(index3, -1), errorStr;
}
 
#endif // STRINGOPS
```
