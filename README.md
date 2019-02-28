# Source-Native
An ongoing documentation about the many failed attempts at writing an open-source transpiler for the Source programming language into Javascript, written in TypeScript (the transpiler, not the documentation).

An explanation of original ideas [here](ideas.md).

An explanation of final work [here](doc.md).

# Progress:
- [x] Native!
- [x] Builtins work! 
- [x] Can store constants and letiables (repl works)
- [x] Proper tail calls (Iterative processes are iterative)
- [x] Enforcing of strict types (`'' * ''` still works)
- [x] Error handling (just throws a bunch of unhelpful JS errors)
- [x] stringifying functions (shows the transpiled code currently) 

# Project Timeline


| Week | Task |
| --- | --|
| 3    | Planning |
| 4 | Get builtins working and incorporate into frontend |
| 5 | Get globals working |
| 6 | Get tail calls working |
| 7 | Get strict types working |
| 8 | Testing of frontend |
| 9 | Figure out how to stop long running code (especicially infinite loops) |
| 10 | Filter out the stack trace to produce sensible error messages |
| 11 | Checking for potential security holes |
| 12 | Buffer |
| 13 | Buffer |
