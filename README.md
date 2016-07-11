# Monolang

## Introduction

This is a monomorphically typed language which
is emphatically inspired by the Curry-Howard
isomorphism. In this spirit, there are minimal
constructs, but I believe that the few provided
will be effective for the construction of
programs which will be extremely optimizable.

The basic constructions allowed for can define
types, terms, and type synonyms. As a first
draft, I will not add any built in types, though
this will obviously make the initial prototype
lack certain efficiency. It is an experimental
project mainly for myself, so I do not care much.

## Syntax

There are purposefully few constructs in Monolang,
and those which are there are those which I find
I cannot do without.

Inspired by Haskell, I have decided to allow types
and data constructors only to begin with uppercase
letters, and variable names only to begin with
lowercase. No characters other than lowercase
and uppercase letters are allowed in identifiers.

The two productions following are the basis for
the above.

> Name ::= [A-Z]{A-Za-z}  
> name ::= [a-z]{A-Za-z}  

As well, there shall be two operators which will
be used for a great variety of things, so I will
define their productions as well.

> SmallStepRight ::= "->"  
> SmallStepLeft  ::= "<-"  
> TypeOr ::= "|"  
> Proves ::= ":"  

The type system is quite simple, it simply consists
of unions of constructors.

> Type ::= CDef {TypeOr CDef}  
> CDef ::= Name {Type}  

Unlike in Haskell and many other languages with
algebraic data types, each construction can
belong to arbitrarily many types, so it must
be treated as an enumeration in a different,
more global way. This can be optimized away,
surely, but does not to me seem to present major
performance problems.

> TypeDef ::= Name SmallStepRight Type  
> SynDef ::= Name SmallStepLeft Name  

Term construction is slightly different, as you
can state which type a term is a construction,
or a proof of, and you should also be able to
state what reduction should take place on a small
step, or what the definition of the function is.

The syntax for this is going to look different
than expected, but it will expand the things you
are able to write, not reduce it. 

> Assertion ::= Term Proves Type  
> FunDef ::= name SmallStepRight Term  

With each of these things in place, we can now show
the syntax for each of the top level declarations.

> Statement ::= Assertion | FunDef | TypeDef | SynDef  

In selecting which terms are allowed in the language,
I had a few things in mind. One, I would like this
language to have as little sugar as possible, not for
any reason of parsing, but in the spirit of mathematics.

I must of course allow the lambda, and I must as well
allow for type assertions next to all terms, as this
is could be helpful in enforcing invariants for the user
which may not be checked automatically by the type system.

I must allow for applications of functions, as well as for
destruction of constructions, the syntax of which will be
parallel to the syntax for their definition.

> Term ::= Application | Destruction | Abstraction | Construction | Annotated  
> Application ::= Term Term  
> CPattern ::= name | Name {CPattern}  
> Destruction ::= Term SmallStepRight [(CPattern SmallStepRight Term) {TypeOr (CPattern SmallStepRight Term}]  
> Abstraction ::= '?' name SmallStepRight Term
> Construction ::= Name {Term}
> Annotated ::= Assertion

## Type System

The type system should work in such a way that it
encourages totality. I think that, while it is
practical that each of your programs terminate,
it is often more important for real life to make
them well defined over all of the possible inputs.
