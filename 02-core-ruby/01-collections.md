# Collections `[Mid]`

## Why Collections Matter

Working with collections of data is the most common operation in any application. Ruby's `Enumerable` module makes this the language's strongest feature. If you master collections, you master most of day-to-day Ruby programming.

## Array

Ordered, indexed collection. Holds any type.

```ruby
fruits = ["apple", "banana", "cherry"]

fruits[0]          # => "apple"
fruits.first       # => "apple"
fruits.last        # => "cherry"
fruits.count       # => 3
fruits.push("date") # or fruits << "date"
fruits.include?("banana")  # => true
```

## Hash

Key-value pairs. Use symbols as keys by convention.

```ruby
user = { name: "Alice", age: 30, role: :admin }

user[:name]        # => "Alice"
user[:email]       # => nil
user.fetch(:email, "none")  # => "none"
user.keys          # => [:name, :age, :role]
user.values        # => ["Alice", 30, :admin]
user[:active] = true  # add new key
```

## Enumerable Methods

Any class that includes `Enumerable` (Array, Hash, Range) gets these methods. Learn them. Use them constantly.

### map -- transform

Returns a new array with the result of running the block on each element.

```ruby
prices = [10, 20, 30]
with_tax = prices.map { |p| p * 1.2 }
# => [12.0, 24.0, 36.0]

names = ["alice", "bob"]
capitalized = names.map(&:capitalize)
# => ["Alice", "Bob"]
```

`&:method_name` is shorthand for `{ |item| item.method_name }`.

### select -- filter

Returns elements where the block is truthy.

```ruby
numbers = [1, 2, 3, 4, 5, 6]
evens = numbers.select(&:even?)
# => [2, 4, 6]
```

### reject -- inverse filter

```ruby
numbers = [1, 2, 3, 4, 5, 6]
odds = numbers.reject(&:even?)
# => [1, 3, 5]
```

### find -- first match

```ruby
numbers = [1, 2, 3, 4, 5]
numbers.find { |n| n > 3 }  # => 4
```

### each_with_object -- build something while iterating

```ruby
words = ["apple", "banana", "avocado", "blueberry"]
grouped = words.each_with_object(Hash.new { |h, k| h[k] = [] }) do |word, hash|
  hash[word[0]] << word
end
# => {"a"=>["apple", "avocado"], "b"=>["banana", "blueberry"]}
```

### group_by -- group by a key

```ruby
words = ["apple", "banana", "avocado", "blueberry"]
words.group_by { |w| w[0] }
# => {"a"=>["apple", "avocado"], "b"=>["banana", "blueberry"]}
```

### flat_map -- map + flatten

```ruby
sentences = ["hello world", "foo bar"]
words = sentences.flat_map { |s| s.split(" ") }
# => ["hello", "world", "foo", "bar"]
```

### reduce / inject -- accumulate

```ruby
prices = [10, 20, 30]
total = prices.reduce(0) { |sum, price| sum + price }
# => 60
```

### any? / all? / none? / one?

```ruby
numbers = [2, 4, 6, 8]
numbers.all?(&:even?)   # => true
numbers.any?(&:odd?)    # => false
numbers.none?(&:odd?)   # => true
numbers.one?(&:even?)   # => false
```

## Chaining

Enumerable methods return new collections, so you can chain them:

```ruby
orders = [
  { product: "Book", price: 20, paid: true },
  { product: "Pen", price: 5, paid: false },
  { product: "Laptop", price: 999, paid: true },
  { product: "Notebook", price: 10, paid: true },
]

total_paid = orders
  .select { |o| o[:paid] }
  .map { |o| o[:price] }
  .sum
# => 1029
```

This is idiomatic Ruby. Read it top to bottom: filter, transform, aggregate.

Move to `02-error-handling.md` to learn how to handle when things go wrong.
