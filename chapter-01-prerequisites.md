# Prerequisites

You can either build l`lvm/mlir` locally or use a pre-built binary as outlined in [the documentation](https://llvm.org/docs/GettingStarted.html). I'm using a `homebrew` installation for the examples below.

!!!note
    Because TableGen is used only in the context of MLIR/LLVM, the `llvm-tblgen` binary is not copied into the usr/bin directory. Make sure the binary is in your path.
