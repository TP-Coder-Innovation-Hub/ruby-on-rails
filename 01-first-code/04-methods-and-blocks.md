# Methods and Blocks 

## Methods

Methods are reusable pieces of code. Define them with `def`:

```ruby
def greet(name)
  "Hello, #{name}"
end

puts greet("Alice")
# => Hello, Alice
```

Ruby methods return the last evaluated expression. No explicit `return` needed in most cases:

```ruby
def add(a, b)
  a + b
end

add(2, 3) # => 5
```

### Default Parameters

```ruby
def greet(name = "World")
  "Hello, #{name}"
end

greet         # => "Hello, World"
greet("Bob")  # => "Hello, Bob"
```

### Keyword Arguments

```ruby
def create_user(name:, email:, role: "viewer")
  "#{name} (#{email}) - #{role}"
end

create_user(name: "Alice", email: "alice@example.com")
# => "Alice (alice@example.com) - viewer"

create_user(name: "Bob", email: "bob@example.com", role: "admin")
# => "Bob (bob@example.com) - admin"
```

Keyword arguments make method calls self-documenting. Rails uses them extensively.

## Blocks

Blocks are Ruby's identity feature. A block is an anonymous function you attach to a method call.

```ruby
[1, 2, 3].each do |number|
  puts number * 10
end
```

The code between `do` and `end` is the block. The method (`each`) executes the block once for each item.

### Two Syntaxes

```ruby
# Multi-line: do...end
[1, 2, 3].each do |n|
  puts n
end

# Single-line: {}
[1, 2, 3].each { |n| puts n }
```

Convention: use `do...end` for multi-line blocks, `{}` for single-line blocks.

### Blocks Are Not Objects

Blocks cannot be assigned to variables directly:

```ruby
my_block = { puts "hello" }  # SyntaxError
```

To store a block as an object, use a Proc or Lambda.

## Procs

A Proc is a block stored as an object:

```ruby
square = Proc.new { |x| x ** 2 }

square.call(5)  # => 25
[1, 2, 3].map(&square)  # => [1, 4, 9]
```

The `&` operator converts a Proc to a block (and vice versa).

## Lambdas

Lambdas are Procs with stricter behavior:

```ruby
greet = ->(name) { "Hello, #{name}" }

greet.call("Alice")  # => "Hello, Alice"
```

### Proc vs Lambda: Two Differences

1. **Return behavior** -- `return` in a Proc returns from the enclosing method. `return` in a Lambda returns from the Lambda only.
2. **Argument checking** -- Lambdas enforce argument count. Procs do not.

```ruby
# Proc: missing argument becomes nil
p = Proc.new { |x| x.inspect }
p.call  # => "nil"

# Lambda: raises ArgumentError
l = ->(x) { x }
l.call  # ArgumentError
```

## Methods That Accept Blocks

Use `yield` to call the block from inside a method:

```ruby
def run_twice
  yield
  yield
end

run_twice { puts "hello" }
# => hello
# => hello
```

Or capture the block explicitly:

```ruby
def run_twice(&block)
  block.call
  block.call
end
```

## Why This Matters for Rails

Rails uses blocks everywhere: migrations, routes, callbacks, validations, views. Understanding blocks is not optional. It is fundamental to reading and writing Rails code.

Move to `05-classes-and-modules.md` to learn object-oriented programming in Ruby.
