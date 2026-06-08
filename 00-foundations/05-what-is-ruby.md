# What Is Ruby? 

## Origin

Ruby was created by Yukihiro "Matz" Matsumoto in 1993 and released in 1995. Matz wanted a language that was more powerful than Perl and more object-oriented than Python. He built Ruby by combining ideas from several languages:

- **Perl** -- Practical text processing and scripting
- **Smalltalk** -- Pure object-oriented design
- **Lisp** -- Blocks and functional programming
- **Scheme** -- Closures

The result is a language optimized for developer productivity and readability.

## Design Philosophy

Matz designed Ruby around a single principle: **developer happiness**.

He has stated:

> "I hope to see Ruby help every programmer in the world to be productive, and to enjoy programming, and to be happy."

This is not marketing. It shapes every design decision in the language:

- **Readable syntax** -- Ruby code reads like English. `5.times { puts "hello" }` is valid Ruby.
- **Principle of least surprise** -- APIs behave the way you would expect. Methods are named clearly. Consistency matters.
- **Everything is an object** -- Numbers, strings, `true`, `false`, `nil` -- all are objects with methods. `5.class` returns `Integer`. `nil.class` returns `NilClass`.
- **Multiple ways to express the same thing** -- Ruby gives you freedom. `if` and `unless`. `do...end` and `{}`. Pick what reads best.

## Everything Is an Object

This is not a metaphor. In Ruby, every value is an object with methods.

> 🖼️ **[IMAGE_PLACEHOLDER]** — Ruby everything is an object method call on primitives

```ruby
5.even?        # => false
"hello".upcase # => "HELLO"
nil.nil?       # => true
[1, 2, 3].sum # => 6
```

You do not need to wrap primitives in utility classes. The methods are already there.

## Ruby's Identity: Blocks

Blocks are Ruby's signature feature. A block is an anonymous function you pass to a method.

```ruby
[1, 2, 3].each { |number| puts number }
```

The `{ |number| puts number }` part is a block. Every method in Ruby can accept one. This enables patterns like iteration, callbacks, and resource management without ceremony.

## Ruby in the Real World

- **GitHub** -- Built on Rails, one of the largest code hosting platforms
- **Shopify** -- Powers millions of e-commerce stores
- **Basecamp** -- The company that created Rails
- **Stripe** -- Payment processing (Ruby SDK, internal tools)
- **Airbnb** -- Core platform built on Rails

Move to `06-why-ruby-rails-why-not-x.md` for an honest comparison of Rails against alternatives.
