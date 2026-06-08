# Control Flow 

## Conditionals

### if / elsif / else

```ruby
score = 85

if score >= 90
  puts "A"
elsif score >= 80
  puts "B"
elsif score >= 70
  puts "C"
else
  puts "F"
end
```

### unless

`unless` is the opposite of `if`. Use it when you want to express "if not":

```ruby
user = nil

unless user
  puts "No user found"
end
```

### Ternary

For simple either/or decisions:

```ruby
age = 20
status = age >= 18 ? "adult" : "minor"
```

### case / when

Multi-way branching. Uses `===` for comparison (not `==`):

```ruby
role = :admin

case role
when :admin  then puts "Full access"
when :editor then puts "Edit access"
when :viewer then puts "Read access"
else puts "No access"
end
```

`case` also works with ranges and classes:

```ruby
case 42
when 0..10   then puts "Small"
when 11..100 then puts "Medium"
end

case "hello"
when String then puts "It is a string"
when Integer then puts "It is a number"
end
```

## Iterators (Preferred Over for Loops)

Ruby idiomatic code uses iterators, not `for` loops. Iterators are more readable and scope the loop variable correctly.

### each -- do something with every item

```ruby
names = ["Alice", "Bob", "Carol"]

names.each do |name|
  puts "Hello, #{name}"
end
```

### map -- transform every item, return new array

```ruby
prices = [10, 20, 30]
with_tax = prices.map { |price| price * 1.2 }
# => [12.0, 24.0, 36.0]
```

### select -- filter items by condition

```ruby
numbers = [1, 2, 3, 4, 5, 6]
evens = numbers.select { |n| n.even? }
# => [2, 4, 6]
```

### reject -- inverse of select

```ruby
numbers = [1, 2, 3, 4, 5, 6]
odds = numbers.reject { |n| n.even? }
# => [1, 3, 5]
```

### times -- repeat N times

```ruby
3.times { puts "hello" }
```

## Why Not for Loops?

```ruby
# Avoid this in Ruby
for name in names
  puts name
end
```

The `for` loop leaks the loop variable into the surrounding scope. `each` does not. In practice, `each` is what Ruby developers write. You will rarely see `for` in production Rails code.

## Short Forms

Single-expression blocks can use `{}` on one line:

```ruby
names.each { |name| puts name }
doubled = numbers.map { |n| n * 2 }
```

Move to `04-methods-and-blocks.md` to learn about methods and Ruby's signature feature: blocks.
