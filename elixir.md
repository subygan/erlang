---
emoji: 🩸
title: Elixir
description: Learning elixir as a non-functional noob
date: 2025-10-03
layout: base
tags: ["tech", "programming"]
---

## Tools

## hex

is the package manager. similar to pip.

### mix

tooling to build, run, test, and release Erlang applications. kinda like cargo for rust.

```bash
mix new my_app # creates a new mix project
```
 
### iex

the interactive elixir shell. kinda like repl for rust.



## Concepts/variables

## Immutability

This is the most interesting part to me. That all data is immutable. which means you don't mutate and return a variable you always create a new variable and return.

to me it looks like this is going to be massively inefficient because then I'm just constantly creating and copying new data. but it seems like since the base assumption is that no data can be modified. each new data built on top of the old data basically points to the old data with the changes. which means it is not quite a completely new copy but a chain of changes on the datastructure. it is sort of like git, in just, a new variable is created my adding the change to the variable.

Then the garbage collector removes all the unnecessary cruft.

## Atoms

Atoms are variables whose value is their own name, they're initialized with a semicolon in the from `:atom` has the value and variable atom

Atoms are not garbage collected 

## Transformations

since all data is immutable everything can follow the unix principle of piping content we can do this


```
request
|> parse
|> route
|> format_reponse
```


is the equivalent of 

```
def handle(request) do
    conv = parse(request)
    conv = route(conv)
    format_response(conv)
end


# or

format_response(route(parse(request)))
```

## Pattern matching

This is a core part of how elixir works. My biggest gripe with rust is that, it doesn't allow function overloading, and now I have function names like `parse_request_with_method(request: Request, method: Method)` when it could've been overloaded and simply be, `parse(request: Request, method: Method)`


Elixir does the opposite of this and allows you to overload functions with the variables based on pattern matching 🤯. Whyyy??


the base is this, the equal(=) operator in elixir matches patterns.


for example

```
iex> x = 1
1 
iex> 1 = x
1 # pattern '1' was matched successfully against 'x' which held the value 1
iex> 2 = x
** (MatchError) no match of right hand side value: 1
```

This can also be done with maps or tuples

```
iex> m = %{hello: "world", hello1: 42}
{hello: "world", hello1: 42}
iex(8)> %{hello:"world"} = m
%{hello: "world", hello1: 42}
```

so in an "overloaded" function you do this match operation, only the function that matches it properly will be sent.


also __functions are matched top to bottom__ so if you have a catchall at the top then everything is going to be rerouted there.

pattern matching is also how you handle errors from functions

## documentations

```
@moduledoc "example doc"
```
inside a `defmodule`

or 

```
@doc "example doc"
```

above a function in a module.


```
defmodule namer do
    @name "suriya"
    def her_name, do:@name

    @name "ruby"
    def his_name, do:@name
end
```

here `@name` is a module attribute way to set constants within the function and the most

## Structs

```
defmodule Servy.Conv do
    defstruct method:"", path: "", status: nil
end
```

Structs are fancy dicts with, only the defined number of keys and default values


There can only be one struct per module. and a struct cannot be associated outside a module.

 
you can't compare a map and a struct though.


## Sockets
