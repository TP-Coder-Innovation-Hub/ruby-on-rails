# Sequential, Decision, Iteration `[Entry]`

## The Three Building Blocks

Every program, regardless of language or paradigm, is built from three constructs:

1. **Sequence** -- Execute instructions in order
2. **Decision** -- Choose between paths based on a condition
3. **Iteration** -- Repeat instructions until a condition is met

That is it. Every algorithm, every application, every framework reduces to these three.

## Sequence

Code runs top to bottom, one line at a time.

```ruby
name = "Alice"
greeting = "Hello, #{name}"
puts greeting
```

Line 1 runs. Then line 2. Then line 3. The order matters. If you swap lines 1 and 2, the program breaks because `name` is undefined when `greeting` is constructed.

Most of your Rails code is sequential. A controller action executes statements in sequence: find a record, modify it, save it, render a response.

## Decision

The program chooses a path based on a condition.

```ruby
age = 20

if age >= 18
  puts "Adult"
else
  puts "Minor"
end
```

Ruby also provides `unless` (the opposite of `if`) and `case/when` (multi-way branching):

```ruby
status = :active

case status
when :active  then puts "Account is active"
when :suspended then puts "Account is suspended"
else puts "Unknown status"
end
```

In Rails, decisions appear everywhere: "if the user is logged in, show their dashboard; otherwise, show the login page."

## Iteration

The program repeats a block of code.

```ruby
names = ["Alice", "Bob", "Carol"]

names.each do |name|
  puts "Hello, #{name}"
end
```

Ruby prefers iterators (`each`, `map`, `select`) over traditional `for` loops. Iterators are more readable and less error-prone.

In Rails, iteration happens when you render a list of records, process a batch of jobs, or validate a collection of inputs.

## Why These Three Matter

Sequence, decision, and iteration are not Ruby concepts. They are universal. If you understand them, you can read code in any language. The syntax changes; the logic does not.

Rails abstracts much of this away. A single line like `User.where(active: true)` handles sequence (build the query), decision (filter by condition), and iteration (traverse results). But under the hood, it is still these three building blocks.

Move to `04-compiler-vs-interpreter.md` to understand how Ruby executes these instructions.
