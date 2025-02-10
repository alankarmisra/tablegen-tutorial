# Preprocessing

If you're coming from a C++ background, these preprocessing directives should look familar. We've been using them all along so there's no surprises below.

```tablegen
/// preprocessor.td

#ifndef PREPROCESSOR_TD
#define PREPROCESSOR_TD
 
// This will only work if your include paths are set correctly. 
include "mlir/IR/BuiltinAttributes.td"
 
class C {}

// the #else clause is also available in the preprocessor.
// but we don't use it here.
 
#endif // PREPROCESSOR_TD
```
