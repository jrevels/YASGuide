# YASGuide - Yet Another Style Guide For Julia

This guide is currently a work in progress. In the future, it should feature more examples and justification/explanations for various decisions.

It is more strict than the [Julia Manual's Style Guide](https://docs.julialang.org/en/v1.0.0/manual/style-guide/), and diverges in some places. For stylistic concerns not addressed in this guide, the manual's guide is a good fallback.

This guide is inspired by the [JuMP Style Guide](http://www.juliaopt.org/JuMP.jl/latest/style.html) and adopts a few of its guidelines.

## Naming Guidelines

- All type names should be `CamelCase`.
- All struct names should be `CamelCase`.
- All module names should be `CamelCase`.
- All function names should be `snake_case` (all lowercase).
- All variable names should be `snake_case` (all lowercase).
- All constant names should be `SNAKE_CASE` (all uppercase).
- All abstract type names should begin with `Abstract`.
- All type variable names should be a single capital letter, preferably related to the value being typed.
- Whole words are usually better than abbreviations or single letters.
- Single letters can be okay when naming a mathematical entity, i.e. an entity whose purpose or non-mathematical "meaning" is likely only known by downstream callers. For example, `a` and `b` would be appropriate names when implementing `*(a::AbstractMatrix, b::AbstractMatrix)`, since the "meaning" of those arguments (beyond their mathematical meaning as matrices, which is already described by the type) is only known by the caller.

## Comment Guidelines

- `TODO` to mark todo comments and `XXX` to mark comments about currently broken code
- Quote code in comments using backticks (e.g. `` `variable_name` ``).
- Use section headers to split code files into logical chunks. Section header formatting is as follows:

```
#####
##### Section Name
#####
```

## Line/Alignment/Spacing Guidelines

Note that lines containing only comments are not considered empty lines.

- All files should have newline(https://en.wikipedia.org/wiki/Newline) character at the end of the file.
- Use spaces between binary operators.
- Use a single space after commas and semicolons.
- Do not use extra spaces for unary operators, parentheses, or brackets.
- Use 4 spaces for indentation; no tabs, ever.
- Never follow an empty line with another empty line.
- For single-line method definitions, use the form `f(args...) = ...`.
- For method definitions spanning multiple lines, use the `function ... end` definition form.
- The first and last lines of an indentation block should never be empty.
- Top-level assignments or definitions should generally be separated by a single empty line.
- If defining more than one single-line method for the same function, they may be grouped together by removing the empty line.
- Function definitions using the `function` form should always be preceded by an empty line and followed by an empty line.
- Try to limit line length to 80 characters per line. This rule should not be enforced arbitrarily, but is rather a soft guideline.
- Definitions of types/structs without fields and functions without implementations ("stubs") should written in single-line form (e.g. `struct Foo <: AbstractFoo end`, `function foo end`).
- If an argument list extends to multiple lines, align new lines to the parenthesis that started the argument list.

## Other Syntax Guidelines

- Do not explicitly name type variables in method signatures when unary `<:` or `>:` would suffice.
- Type variable bindings should always be enclosed within `{}` brackets when using `where` syntax, e.g. `Vector{Vector{T} where T}` is good, `Vector{Vector{T}} where {T}` is good, `Vector{Vector{T}} where T` is bad.
- A decimal should never be the last character in numeric literals (e.g. `1.` is bad, `1.0` is good).
- Use `in` instead of `=` when specifying `for` statement iteration variables (e.g. `for i = 1:5` is bad, `for i in 1:5` is good).
- Where type annotations would normally default to `Any`, explicitly annotate as `Any`. This rule need not apply to argument types in method signatures.
- In argument lists, always separate positional arguments from keyword arguments using a semicolon (`;`).

## Programming Guidelines

- Dispatch type constraints should be used chiefly for *dispatch*, not for artificially restricting method signatures to "known" types, or merely for documentation.
- Dispatching on type parameters should be avoided unless necessary, especially if the method author does not "own" the method and/or the type being parameterized (in which case, this kind of dispatch could be an instance of "type piracy"). This is because naive dispatch on type parameters often does not (and cannot) describe values the way the programmer intends, unless the programmer explicitly accounts for invariance. For example, `AbstractArray{<:MyType}` does not describe "any value of type `<:AbstractArray` containing elements of type `<:MyType`"; it only describes a subset of such values. Likewise, `AbstractArray{>:MyType}` describes "any value of type `<:AbstractArray` that *could* contain elements of type `<:MyType`", which is likely a superset of what the programmer intended to describe (e.g. `AbstractArray{Any} <: AbstractArray{>:Number}` is `true`). This guideline can generally be relaxed when the method author "owns" both the method and the type being parameterized.
- Always use explicit `return` statements in `function ... end` definitions.
- If a function does not have a clearly appropriate return value, then explicitly return `nothing`.
- If a function definition is not at top-level, then it should be an anonymous function rather than a named function. This lessens confusion about whether the function is properly overloadable.
