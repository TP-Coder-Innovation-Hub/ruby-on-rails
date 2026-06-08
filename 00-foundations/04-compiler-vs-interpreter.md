# Compiler vs Interpreter 

## Two Ways to Run Code

Programming languages need to be translated into machine code (the 1s and 0s a CPU understands). There are two strategies:

- **Compiled** -- Translate the entire program before running it
- **Interpreted** -- Translate and execute line by line at runtime

Ruby is interpreted.

## How a Compiler Works

A compiler reads your entire source file, translates it to machine code, and produces an executable binary. You run the binary directly.

```
Source Code --> [Compiler] --> Binary --> [Execute]
```

**Advantages:**
- Fast execution (the translation is done)
- Catches many errors before the program runs

**Examples:** C, C++, Go, Rust

**Trade-off:** Every change requires recompilation. The edit-run cycle is slower.

## How an Interpreter Works

An interpreter reads your source code and executes it directly, line by line, at runtime.

```
Source Code --> [Interpreter] --> Execute immediately
```

**Advantages:**
- Fast feedback loop -- write code, run it immediately
- Dynamic features -- code can modify itself at runtime (metaprogramming)
- Platform independence -- the same code runs anywhere the interpreter exists

**Examples:** Ruby, Python, JavaScript

**Trade-off:** Slower execution than compiled languages because translation happens at runtime.

## Ruby's Reality: Both

Ruby is technically a compiled-interpreted hybrid. Since Ruby 1.9, the interpreter (YARV/MRI) compiles source code to bytecode, then executes the bytecode. This is faster than pure interpretation but slower than ahead-of-time compilation to machine code.

For practical purposes, Ruby behaves like an interpreted language. You write code, save the file, and run it with `ruby my_program.rb`. No separate compilation step.

## What This Means for Rails Development

The interpreted nature of Ruby is central to the Rails development experience:

- **Fast iteration** -- Change a line, refresh the browser, see the result
- **REPL (IRB/Pry)** -- Run Ruby code interactively in a console for immediate feedback
- **Metaprogramming** -- Classes can define methods at runtime based on database schema (this is how Active Record works)
- **Dynamic typing** -- Variables are not bound to a type at compile time

The trade-off is runtime performance. For most web applications, this trade-off is acceptable. Shopify handles billions of requests on Ruby. GitHub runs on Rails. The bottleneck is almost always the database, not the language.

Move to `05-what-is-ruby.md` for the full picture of what Ruby is and why it exists.
