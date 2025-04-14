# !subst with records

I haven't seen many use-cases for `!subst` with `def`s instead of strings, but let's look at an example because it confused me a little when I first read the description:

```tablegen
// Define three test records, R1, R2, and R3
class C {
}

def R1 : C {
}

def R2 : C {
}

def R3 : C {
}

// !subst(target, repl, value)
// If the value is a record name, the operator checks if the value record name
// matches the target record name.
// - If the record names match, the operator returns the repl record.
// - If the record names do not match, the operator simply returns the value record.
def R4 : C {
  // Now use !subst to substitute one record with another
  // If value is R1, it will replace it with R2.
  C result = !subst(R1, R2, R1); // == replace R1 in R1 with R2 -> R2

  // If value is R3, no substitution occurs because R3 != R1.
  C no_substitution = !subst(R1, R2, R3); // == replace R1 in R3 with R2 -> R3, 
 // because R1 cannot be found in R3
}
```
