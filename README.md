# Magrittr.jl

It's Lazy.jl but only the pipe macro `@>`, `@>>` and `@as`.

```julia
Pkg.add("Magrittr")
```
### Macros

The threading macros will pipe values through functions, a bit like the `|>` operator but far more flexible. They can make code a *lot* cleaner by putting function calls in the order they are applied. The best way to understand them is by example:

```julia
# Just like x |> f etc.
@> x f == f(x)
@> x g f == f(g(x))
@> x a b c d e == e(d(c(b(a(x)))))

# Unlike |>, functions can have arguments - the value
# preceding a function will be treated as its first argument
@> x g(y, z) f == f(g(x, y, z))

@> x g f(y, z) == f(g(x), y, z)

# @>> does the exact same thing, but with value treated as the *last* argument.

@>> x g(y, z) f == f(g(y, z, x))

@>> x g f(y, z) == f(y, z, g(x))

# @as lets you name the threaded argument
@as _ x f(_, y) g(z, _) == g(z, f(x, y))

# All threading macros work over begin blocks

@as x 2 begin
 x^2
 x+2
end == 6
```
