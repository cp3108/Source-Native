# Why go native, or why isn't it already native?

Everyone knows about the wonders of `eval`, that takes in a string and runs it as Javascript code. But simply using it on _Source_ would not cut it. 

# Problem 1: Disallowed constructs

_Source_ bans 'bad' Javascript features, such as `==`, `!==`, `with`. It also disallows the more advanced features such as objects, rest parameters, classes, and generators. 

This is an already solved problem. Numerous Abstract Syntax Tree generators are available, and using them it's trivial to detect such constructs in code.

The current interpreter for _Source_ already does it, so that's one part of the problem solved.

# Problem 2: Strict types

`'' * ''` is valid Javascript code, but is not in _Source_. This is undetectable during parsing of code, as the expression on the left and right would have to be evaluated first to know their type.
