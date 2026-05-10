# Introduction

## What is TableGen?

TableGen is a data definition language and allows one to specify what it calls records. A backend can parse the records and use as needed. Here’s a quick example so we aren’t running blind.

```tablegen
class Compiler {
  string version = "1.0";
  string target = "arm64-apple-macosx"; 
}
```

It’s ok not to understand everything. And it’s ok to think this looks suspiciously like C++. Because it does. But it is not C++ as will be apparent in later examples.

## How does MLIR use it?

In the context of MLIR, TableGen files you write will describe different components of the compiler allowing an MLIR backend to generate C++ classes and helper functions for your compiler. This approach is quicker, cleaner, and more efficient than writing the C++ classes yourself. It also encourages adherence to certain guidelines, as we’ll explore in later chapters, leading to a more consistent development process.

!!!note
    You can even write your own custom backend, though this won’t be covered or necessary in our case.

## Why do I need to learn a new language?

Why not just use JSON, YAML, or any one of the several available options? While we could, TableGen provides additional features like templates, class hierarchies, macros, and preprocessing, allowing us to minimize repetition and reduce the complexity of specifying information. However, the question of the usefulness of TableGen has been discussed within the community as well. For now this is their tool of choice, so we run with it.
