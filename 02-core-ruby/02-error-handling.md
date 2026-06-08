# Error Handling `[Mid]`

## Basic Pattern: begin / rescue / ensure

```ruby
def divide(a, b)
  result = a / b
  puts "Result: #{result}"
rescue ZeroDivisionError
  puts "Cannot divide by zero"
ensure
  puts "Division attempted"
end

divide(10, 2)
# => Result: 5
# => Division attempted

divide(10, 0)
# => Cannot divide by zero
# => Division attempted
```

- `begin` (or the method body itself) -- the code that might fail
- `rescue` -- what to do when a specific error occurs
- `ensure` -- code that always runs, whether an error occurred or not

## How It Works

Ruby uses exception objects. When something goes wrong, Ruby raises an exception. If nothing catches it, the program crashes with a stack trace.

```ruby
10 / 0
# => ZeroDivisionError: divided by 0
```

`rescue` catches specific exception classes:

```ruby
begin
  # risky code
rescue ZeroDivisionError
  # handle division by zero
rescue TypeError
  # handle type mismatch
rescue => e
  # catch any other StandardError subclass
  puts e.class   # the exception class
  puts e.message # the error message
end
```

## Catching Specific Errors

Always rescue specific error classes. Catching all errors with a bare `rescue` hides bugs.

```ruby
# Bad -- catches everything including system errors
rescue => e

# Good -- catches only what you expect
rescue ActiveRecord::RecordNotFound
rescue KeyError
rescue JSON::ParserError
```

## Raising Exceptions

Use `raise` to signal that something went wrong:

```ruby
def withdraw(amount, balance)
  raise ArgumentError, "Amount must be positive" if amount <= 0
  raise InsufficientFunds, "Not enough balance" if amount > balance

  balance - amount
end
```

Two forms:

```ruby
raise "Something went wrong"              # raises RuntimeError with message
raise ArgumentError, "Invalid input"      # raises specific class with message
raise CustomError.new("details")          # raises a custom exception instance
```

## Custom Error Classes

Define your own exception classes for domain-specific errors. Inherit from `StandardError`.

```ruby
class InsufficientFunds < StandardError; end
class AccountFrozen < StandardError; end

def withdraw(amount, account)
  raise AccountFrozen, "Account #{account.id} is frozen" if account.frozen?
  raise InsufficientFunds, "Need #{amount}, have #{account.balance}" if amount > account.balance

  account.balance -= amount
end
```

Custom error classes let calling code handle different failure cases differently:

```ruby
begin
  withdraw(100, account)
rescue InsufficientFunds => e
  redirect_to account_path, alert: e.message
rescue AccountFrozen => e
  redirect_to support_path, alert: "Contact support"
end
```

## ensure

Use `ensure` for cleanup that must run regardless of success or failure:

```ruby
def process_file(path)
  file = File.open(path, "r")
  content = file.read
  process(content)
rescue Errno::ENOENT
  puts "File not found: #{path}"
ensure
  file&.close
end
```

The `&.` operator (safe navigation) calls `close` only if `file` is not `nil`.

## retry

`retry` re-runs the `begin` block from the start. Use sparingly.

```ruby
retries = 0

begin
  call_external_api
rescue Net::TimeoutError
  retries += 1
  retry if retries < 3
  raise
end
```

## Rails Context

In Rails, most error handling happens in controllers and background jobs:

- Controllers use `rescue_from` for shared error handling
- Background jobs use `retry_on` for transient failures
- Models use validations to prevent bad data (not exceptions)

Move to `03-gems-and-bundler.md` to learn how Ruby manages dependencies.
