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


