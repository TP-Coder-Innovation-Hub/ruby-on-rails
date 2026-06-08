# Variables and Types `[Entry]`

## Variables

Ruby variables do not need type declarations. The type is determined by the value assigned.

```ruby
name = "Alice"
age = 30
active = true
```

Variable naming conventions:

- `snake_case` for local variables and method names
- `UPPER_CASE` for constants
- `@instance_var` for instance variables (inside objects)
- `@@class_var` for class variables (rare in practice)

## Dynamic Typing

A variable can hold any type and can change types at runtime:

```ruby
x = 5        # Integer
x = "hello"  # Now it is a String
x = [1, 2]   # Now it is an Array
```

Ruby does not enforce types at assignment. This is dynamic typing.

## Duck Typing

Ruby does not check what an object *is*. It checks what an object *can do*.

```ruby
def double(value)
  value * 2
end

double(5)         # => 10
double("ha")      # => "haha"
double([1, 2, 3]) # => [1, 2, 3, 1, 2, 3]
```

All three types respond to `*`. Ruby does not care that they are different types. It only cares that they support the method being called.

"If it walks like a duck and quacks like a duck, it is a duck."

## Everything Is an Object

In many languages, `5` is a primitive. In Ruby, `5` is an object with methods:

```ruby
5.class           # => Integer
5.even?           # => false
5.next            # => 6
5.times { puts "hi" }  # prints "hi" 5 times
```

Even `nil` (Ruby's null) is an object:

```ruby
nil.class         # => NilClass
nil.nil?          # => true
nil.to_s          # => ""
nil.to_i          # => 0
```

## Core Types

| Type | Example | Description |
|------|---------|-------------|
| Integer | `42` | Whole numbers |
| Float | `3.14` | Decimal numbers |
| String | `"hello"` | Text, double or single quotes |
| Symbol | `:name` | Immutable identifier, used as keys |
| Array | `[1, 2, 3]` | Ordered collection |
| Hash | `{ name: "Alice" }` | Key-value pairs |
| Boolean | `true`, `false` | Logic values |
| Nil | `nil` | Absence of value |
| Range | `1..10` | Sequence of values |

## Symbols vs Strings

Symbols are immutable and reused in memory. Use them for identifiers (hash keys, method names):

```ruby
# Symbol -- same object in memory every time
:name.object_id  # => same number each call

# String -- new object every time
"name".object_id # => different number each call
```

In Rails, symbols appear everywhere: hash keys, route names, status flags, association declarations.

Move to `03-control-flow.md` to learn how to make decisions and repeat actions.
