rewatch "90 minute Scheme to C compiler presentation"

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

- An untyped language (without dynamic tagged)
  that is easy to do source-to-source transformation

# problems

How to compile with proper tail-call?

- Solution 1: CPS
- Solution 2: use jump instead of call when calling the last function

How to compile closure to a language without closure?

- Solution 1: lambda lifting.
- Solution 2: closure conversion
  - closure -> vector of closure (no free-variables) and arguments
  - call to closure -> call to vector's closure append vector's arguments

How to not use tagged value in the runtime?

- Just not use them, and define a lot of type specific primitives.
- Maybe we need a simple type system for this.

CPS (continuation passing style)

```scheme
(let ([square (lambda (x) (* x x))])
  (write (+ (square 10) 1)))
```

The continuation of

```scheme
(square 10)
```

is

```scheme
(lambda (r) (write (+ r 1)))
```

The whole examples in CPS:

```scheme
(let ([square (lambda (k x) (k (* x x)))])
  (square
    (lambda (r) (write (+ r 1)))
    10))
```

```scheme
;; If the example is:

(let ([mul (lambda (x y) (* x y))])
  (let ([square (lambda (x) (mul x x))])
    (write (+ (square 10) 1))))

;; CPS:

(let ([mul (lambda (k x y) (k (* x y)))])
  (let ([square (lambda (k x) (mul k x x))])
    (square
      (lambda (r) (write (+ r 1)))
      10)))
```

In postfix notation:

```ruby
def square (x) x x * end
1 10 square add write
```

In postfix notation (CPS):

```ruby
def square (x k) x x * k end
10
lambda (r) 1 r add write end
square
```

```ruby
# If the example is:

def mul (x y) x y * end
def square (x) x x mul end
1 10 square add write

# CPS:

def mul (x y k) x y * k end
def square (x k) x x k mul end
10
lambda (r) 1 r add write end
square
```
