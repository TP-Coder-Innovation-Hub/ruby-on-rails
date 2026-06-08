# Classes and Modules 

## Classes

A class is a blueprint for creating objects. Objects hold data (instance variables) and behavior (methods).

```ruby
class Dog
  def initialize(name, breed)
    @name = name
    @breed = breed
  end

  def bark
    puts "#{@name} says woof!"
  end

  def to_s
    "#{@name} (#{@breed})"
  end
end

rex = Dog.new("Rex", "German Shepherd")
rex.bark    # => Rex says woof!
puts rex    # => Rex (German Shepherd)
```

`initialize` is the constructor. It runs when you call `Dog.new`. Instance variables (`@name`, `@breed`) are private by default.

### Accessor Methods

Use `attr_accessor`, `attr_reader`, and `attr_writer` to generate getters and setters:

```ruby
class User
  attr_accessor :name
  attr_reader :email

  def initialize(name, email)
    @name = name
    @email = email
  end
end

user = User.new("Alice", "alice@example.com")
user.name          # => "Alice"
user.name = "Bob"  # setter via attr_accessor
user.email         # => "alice@example.com"
# user.email = "x" # NoMethodError -- attr_reader only provides getter
```

## Inheritance

Ruby supports single inheritance. A subclass inherits from one superclass:

```ruby
class Animal
  def initialize(name)
    @name = name
  end

  def speak
    "..."
  end
end

class Dog < Animal
  def speak
    "Woof!"
  end
end

class Cat < Animal
  def speak
    "Meow!"
  end
end

Dog.new("Rex").speak  # => "Woof!"
Cat.new("Whiskers").speak  # => "Meow!"
```

Use `super` to call the parent's version of the current method:

```ruby
class GuideDog < Dog
  def speak
    "#{super} I am a guide dog."
  end
end
```

## Modules

Modules group related methods and constants. They cannot be instantiated.

### Mixins (include)

Use `include` to add instance methods from a module:

```ruby
module Greetable
  def greet
    "Hello, I am #{name}"
  end
end

class Person
  include Greetable
  attr_reader :name

  def initialize(name)
    @name = name
  end
end

Person.new("Alice").greet  # => "Hello, I am Alice"
```

### Class Methods (extend)

Use `extend` to add class-level methods:

```ruby
module Findable
  def find(id)
    "Found record #{id}"
  end
end

class User
  extend Findable
end

User.find(1)  # => "Found record 1"
```

### include vs extend

| Keyword | Adds methods as | Example call |
|---------|----------------|--------------|
| `include` | Instance methods | `user.greet` |
| `extend` | Class methods | `User.find(1)` |

## Inheritance vs Composition

- **Inheritance:** "is a" relationship. A Dog *is an* Animal.
- **Composition:** "has a" or "behaves like" relationship. A Person *is Greetable* via a module.

Ruby favors composition through modules (mixins) over deep inheritance hierarchies. Rails uses this pattern extensively: controllers include modules for authentication, models include modules for associations and validations.

## Rails Connection

In Rails:

- Models are classes that inherit from `ApplicationRecord`
- Controllers are classes that inherit from `ApplicationController`
- Concerns are modules mixed into models and controllers
- Helpers are modules included in views

Understanding classes and modules is prerequisite to understanding any Rails codebase.

Move to `02-core-ruby/01-collections.md` for Ruby's most powerful feature.
