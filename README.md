# Hackett [![Build Status](https://travis-ci.org/lexi-lambda/hackett.svg?branch=travis-ci)](https://travis-ci.org/lexi-lambda/hackett)

Hackett is an attempt to implement a Haskell-like language with support for Racket’s macro system, built using the techniques described in the paper [*Type Systems as Macros*][types-as-macros]. It is currently *extremely* work-in-progress.

Here are some of the features that Hackett supports **right now**:

  - Parametric polymorphism
  - Algebraic datatypes (ADTs)
  - Hindley-Milner type inference
  - Typeclasses
  - Type-aware/type-directed macros

Here are some of the features that still need to be implemented for a minimal release:

  - Kindchecking
  - Exhaustiveness checking
  - Type inference for recursive definitions
  - Type expanders (of which type aliases are a subset)

And finally, here is a (non-exhaustive) collection of features I would like to eventually support:

  - Multi-parameter typeclasses
  - Functional dependencies
  - Syntax for infix operators
  - GADTs
  - Higher-rank polymorphism
  - Row types
  - Type families

Due to the way Hackett is implemented, many things that are language features in Haskell can be derived concepts in Hackett. In fact, Hackett’s ADTs are not primitives, they are actually implemented as a library via the `data` and `case` macros in `hackett/private/adt`. Other things, like newtype deriving and generics, should be possible to implement as derived concepts as well.

Here’s what some Hackett code *might* eventually look like:

```
#lang hackett

(data (Maybe a)
  nothing
  (just a))

(def x : Int
  (let ([y 3]
        [z 7])
    {y + z}))

(class (Show a)
  [show : {Int -> a -> String}])

(instance (forall [a] (Show a) => (Show (Maybe a)))
  (def+ show
    [nothing  -> "nothing"]
    [(just x) -> {"just " <> (show x)}]))
```

Much of the above syntax is already implemented, but some things are not.

## Trying Hackett

To reiterate: **Hackett is extremely experimental right now.** Things are not guaranteed to work correctly (or work at all), and things are likely to change dramatically. If you really want to install Hackett to play around with it, though, you can.

You will need to have Racket installed to use Hackett. Using `raco`, you can install Hackett as a package:

```
$ raco pkg install hackett
```

Now you can use Hackett by writing `#lang hackett` at the top of a file. Documentation is currently sparse, but you can take a look at what some Hackett code looks like by looking at the source of the `hackett/prelude` module.

[types-as-macros]: http://www.ccs.neu.edu/home/stchang/pubs/ckg-popl2017.pdf
