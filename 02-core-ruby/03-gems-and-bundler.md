# Gems and Bundler 

## What Is a Gem?

A gem is a packaged Ruby library or application. It contains Ruby code, metadata, and sometimes native extensions.

Install a gem:

```bash
gem install devise
```

List installed gems:

```bash
gem list
```

## The Problem Bundler Solves

Installing gems globally causes problems:

- Project A needs Devise 4.9. Project B needs Devise 4.8. They conflict.
- "Works on my machine" -- different gem versions on different developer machines.
- Production has different versions than development.

Bundler solves this by locking exact gem versions per project.

## Gemfile

A `Gemfile` lives in your project root. It declares what gems your project needs:

```ruby
# Gemfile
source "https://rubygems.org"

gem "rails", "~> 8.0"
gem "pg"
gem "puma", ">= 6.0"
gem "devise", "~> 4.9"
gem "rspec-rails", group: :test
gem "factory_bot_rails", group: :test
gem "rubocop", group: [:development, :test]
```

### Version Specifiers

| Specifier | Meaning |
|-----------|---------|
| `"4.9.0"` | Exact version |
| `">= 4.9"` | 4.9 or higher |
| `"~> 4.9"` | 4.9.x (patch updates only) |
| `"~> 4.9.0"` | 4.9.0 to 4.9.x (more restrictive) |

`~>` (twiddle-wakka) is the most common specifier. It allows safe updates without breaking changes.

## bundle install

```bash
bundle install
```

This command:

1. Resolves all gem dependencies (including transitive dependencies)
2. Downloads and installs the gems
3. Creates `Gemfile.lock` with exact versions

The `Gemfile.lock` file is critical. Commit it to version control. It ensures every developer and every deployment uses identical gem versions.

## Gemfile.lock

```
GEM
  remote: https://rubygems.org/
  specs:
    devise (4.9.3)
    rails (8.0.0)
    ...

PLATFORMS
  ruby

DEPENDENCIES
  devise (~> 4.9)
  rails (~> 8.0)
```

This file is auto-generated. Do not edit it by hand. To update a gem:

```bash
bundle update devise       # update devise to latest compatible version
bundle update              # update all gems within Gemfile constraints
```

## Groups

Group gems by environment to avoid loading test gems in production:

```ruby
gem "rails"

group :development, :test do
  gem "rspec-rails"
  gem "factory_bot_rails"
  gem "rubocop"
end

group :production do
  gem "kamal"
end
```

Install without certain groups:

```bash
bundle install --without production
```

## bundle exec

Run commands in the context of your bundled gems:

```bash
bundle exec rails server
bundle exec rspec
bundle exec rubocop
```

This ensures the command uses the gem versions from your `Gemfile.lock`, not globally installed gems.

## Adding a Gem to Your Project

1. Add the gem to your `Gemfile`
2. Run `bundle install`
3. Read the gem's README for configuration steps
4. Commit both `Gemfile` and `Gemfile.lock`

## Rails Default Gems

A new Rails 8 application ships with a `Gemfile` that includes:

- `rails` -- the framework
- `pg` -- PostgreSQL adapter
- `puma` -- web server
- `propshaft` -- asset pipeline
- `solid_queue` -- background jobs
- `kamal` -- deployment
- `rspec-rails` -- testing (if you chose RSpec at install time)

Move to `04-metaprogramming-basics.md` to understand how Rails uses Ruby's dynamic nature.
