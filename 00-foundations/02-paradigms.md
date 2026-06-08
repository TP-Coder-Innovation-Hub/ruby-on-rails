# Programming Paradigms `[Entry]`

## What Is a Paradigm?

A paradigm is a style of organizing code. It is not a language feature -- it is a philosophy about how to think about problems.

Most languages support multiple paradigms. Ruby supports three primary ones.

## Imperative

You tell the computer exactly what to do, step by step.

```ruby
total = 0
prices = [10, 20, 30]

i = 0
while i < prices.length
  total = total + prices[i]
  i = i + 1
end

puts total
```

This works. It is clear. It is also verbose. Imperative code focuses on **how** to solve the problem.

## Object-Oriented Programming (OOP)

You model your program as objects that communicate with each other. Each object encapsulates data and behavior.

```ruby
class ShoppingCart
  def initialize
    @items = []
  end

  def add(price)
    @items << price
  end

  def total
    @items.sum
  end
end

cart = ShoppingCart.new
cart.add(10)
cart.add(20)
cart.add(30)
puts cart.total
```

Objects own their data. Objects expose methods. Other objects call those methods without knowing the internals.

Rails uses OOP extensively. A `User` model is an object. A `PostsController` is an object. The request itself is an object.

## Functional

You describe **what** you want, not **how** to compute it. Functions transform data without mutating state.

```ruby
prices = [10, 20, 30]
total = prices.sum
puts total
```

Ruby borrows heavily from functional programming. Methods like `map`, `select`, `reduce`, and `each` are functional patterns. They take a block (a function) and apply it to a collection.

## Where Ruby Lands

Ruby is primarily object-oriented but embraces functional idioms. Yukihiro Matsumoto ("Matz"), Ruby's creator, designed it for developer happiness. He took ideas from Perl, Smalltalk, Lisp, and Scheme, then made them readable.

In practice, a Rails application is OOP at the structural level (models, controllers, services) and functional at the data-transformation level (enumerables, blocks).

## Why This Matters

You will encounter all three styles in Rails code. Understanding the distinction helps you read existing code and make intentional choices when writing new code.

Move to `03-sequential-decision-iteration.md` for the three building blocks that underpin all three paradigms.
