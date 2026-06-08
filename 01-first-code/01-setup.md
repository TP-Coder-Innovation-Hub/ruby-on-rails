# Setup 

## Install Ruby with rbenv

rbenv manages multiple Ruby versions. Use it instead of system Ruby to avoid permission issues and version conflicts.

### macOS

```bash
# Install Homebrew if you do not have it
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install rbenv and ruby-build
brew install rbenv ruby-build

# Add rbenv to your shell
echo 'eval "$(rbenv init -)"' >> ~/.zshrc
source ~/.zshrc

# Install the latest stable Ruby
rbenv install 3.3.4
rbenv global 3.3.4

# Verify
ruby -v
# => ruby 3.3.4 (or later)
```

### Linux (Ubuntu/Debian)

```bash
# Install dependencies
sudo apt update
sudo apt install -y git curl autoconf bison build-essential libssl-dev libyaml-dev libreadline-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm-dev

# Install rbenv
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build

# Add to shell
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc

# Install Ruby
rbenv install 3.3.4
rbenv global 3.3.4

# Verify
ruby -v
```

## Install Rails

```bash
gem install rails
rbenv rehash

# Verify
rails -v
# => Rails 8.0.0 (or later)
```

## Choose an Editor

VS Code with the following extensions works well:

- **Ruby LSP** -- Language server for autocompletion and navigation
- **Rails** -- Snippets and navigation for Rails projects

Other options: RubyMine (paid, full-featured), Neovim with LSP (terminal-only).

## Run Your First Program

Create a file called `hello.rb`:

```ruby
puts "Hello, world!"
puts "The current time is #{Time.now}"
puts "Ruby version: #{RUBY_VERSION}"
```

Run it:

```bash
ruby hello.rb
# => Hello, world!
# => The current time is 2026-06-08 12:00:00 +0000
# => Ruby version: 3.3.4
```

## Try the Interactive Console

IRB (Interactive Ruby) lets you run Ruby code line by line:

```bash
irb
```

```ruby
irb(main):001> 2 + 2
=> 4
irb(main):002> "hello".upcase
=> "HELLO"
irb(main):003> [1, 2, 3].sum
=> 6
irb(main):004> exit
```

Use IRB to experiment. It is the fastest way to learn what a method does.

Move to `02-variables-and-types.md` to start learning Ruby syntax.
