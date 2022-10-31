rewatch "90 minute Scheme to C compiler presentation"

- https://www.youtube.com/watch?v=Bp89aBm9tGU
- https://www.youtube.com/watch?v=M4dwcdK5bxE

reread the paper

reread the tutorial

# aims

> A compiler target for languages with powerful type systems.

Low level programming like in Forth and C,
is about bits and bytes in memory (stack or heap).

Features:

- can do side-effect
- with GC
- with closure
- with proper-tail call
- with first-class continuation

# syntax

> Forth-like language, with `def` and `end` style syntax instead of `:` and `;`

Concepts like tail-call and continuation will be easy to explian.

- tail-call is the last call in a sequence
- continuation is current value-stack + return-stack
  - the interface will be like `call/cc`
    - the lambda that is been called will not be part of the continuation
  - we can use label to do delimited continuation

Composition will make it easy to do optimization.

# problems

How to compile with proper tail-call?

How to compile closure to a language without closure?

How to not use tagged value in the runtime?
