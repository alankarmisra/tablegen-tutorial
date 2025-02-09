# The paste operator

The paste operator is useful but has a few quirks. First let's start with the useful usecases (see what I did there?).

```c
defvar errorStr = "error";

class cls<string opName> {
  // The paste operator can combine two lists
  list<int> lst = [1, 2] # [3, 4];
  // and concatenate two strings 
  string name = "my" # opName; // "myadd"
}

// You can also use verbatim strings
// (strings without the quotes)
// to create a new class name
// though I can't imagine where
// I would use this syntax
def add # op : cls<"add">{ }

// But it makes more sense 
// with one verbatim string
// and one variable.
foreach i = [1, 2] in {
  def rec # i { } // rec is a naked name, i is a variable. Output is below.
}
```

which outputs:

```c
------------- Classes -----------------
class cls<string cls:opName = ?> {
  list<int> lst = [1, 2, 3, 4];
  string name = !strconcat("my", cls:opName);
}
------------- Defs -----------------
def addop {     // cls
  list<int> lst = [1, 2, 3, 4];
  string name = "myadd";
}
def rec1 { 
}
def rec2 {
}
```

### Paste: The weirdness

```c
defvar suffix = "_string";

def PasteExample {
  string s = suffix # suffix; // Bet you can't guess what this is gonna do.
}
```

In the example above, in the expression `suffix # suffix`, the first suffix is evaluated as a variable, but the second is evaluated as a verbatim string!

So you get the output:

```c
------------- Classes -----------------
------------- Defs -----------------
def PasteExample {
  string s = "_stringsuffix";
}
```

What??? Not a dealbreaker but be wary of this behaviour.
