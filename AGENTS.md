# AGENTS.md

## Context

This repository contains educational content for the "Ruby on Rails Developer" learning path, part of the TP-Coder Innovation Hub curriculum. The content is organized as a directory-per-topic structure targeting developers at Entry, Mid, and Senior levels who want to learn or deepen their understanding of Ruby on Rails for building modern web applications.

## Audience

- Developers new to web frameworks exploring Rails as their first framework
- Developers with experience in other frameworks (Laravel, Django, Spring Boot) evaluating Rails
- Mid-level Rails developers looking to level up their architecture and performance skills
- Senior developers and engineering managers making technology adoption decisions
- Technical instructors creating curriculum or workshops from this material

## How to Help

- Reference Rails 8 features and conventions when generating code examples
- Use Ruby 3.3+ syntax and idioms (endless methods, pattern matching, data classes where relevant)
- Tag content with , , or  level indicators when adding new sections
- Prefer built-in Rails solutions over third-party gems when both are viable
- Use RSpec conventions for test examples
- Include Mermaid diagrams for architecture and flow explanations
- Provide complete, runnable code examples rather than fragments
- Compare Rails approaches against alternatives (Laravel, Django, Spring Boot) when discussing architectural decisions
- Cite real-world examples from Shopify, GitHub, Basecamp, or other prominent Rails users
- Default to Kamal 2 for deployment guidance
- Default to Solid Queue for background job guidance unless the use case requires Sidekiq
- Default to Hotwire (Turbo + Stimulus) for frontend interactivity guidance
- Run `rubocop` checks on any Ruby code changes
- Keep each file between 200-500 words; short, concise, direct
- No emojis in generated content

## How NOT to Help

- Do not suggest Rails 6 or 7 patterns that have been superseded in Rails 8 (e.g., Sprockets instead of Propshaft, custom auth instead of the built-in generator)
- Do not recommend React/Vue/Angular as the default frontend approach for Rails applications -- use Hotwire unless the user explicitly needs a separate SPA
- Do not suggest Sidekiq as the default job backend -- use Solid Queue unless the user has high-throughput requirements (10k+ jobs/min)
- Do not recommend scaffolding for production code without caveats about its limitations
- Do not provide examples that use `protect_from_forgery` or other deprecated patterns
- Do not suggest `DatabaseCleaner` -- Rails has built-in transactional fixtures
- Do not use Rails 4/5 era patterns like `attr_accessible` or strong parameters misconfigurations
- Do not recommend microservices as a default architecture -- prefer monolith-first with modular boundaries
- Do not use emojis in generated content

## Key Concepts

- **Convention over Configuration:** Rails' core philosophy; default directory structure, naming, and patterns
- **Active Record:** ORM pattern mapping Ruby classes to database tables
- **MVC:** Model-View-Controller architecture enforced by the framework
- **Hotwire:** Turbo (server-rendered SPA-like navigation) + Stimulus (lightweight JavaScript controllers)
- **Solid Queue:** Built-in database-backed job queue replacing Redis/Sidekiq for most use cases
- **Kamal 2:** Container-based deployment tool for production Rails applications
- **Propshaft:** Modern asset pipeline replacing Sprockets
- **Service Objects:** Pattern for encapsulating business logic outside of models and controllers
- **Concerns:** Ruby modules mixed into Rails classes for shared behavior
- **RESTful Routing:** Resource-oriented URL design via `resources` in `config/routes.rb`

## Rails Guidelines 2026

- **Framework version:** Rails 8
- **Language version:** Ruby 3.3+
- **Deployment:** Kamal 2 (zero-downtime, container-based)
- **Testing:** RSpec with FactoryBot (Minitest is also acceptable)
- **Linting:** `rubocop` with `rubocop-rails` extension
- **Background Jobs:** Solid Queue (default), Sidekiq (high-throughput only)
- **Frontend:** Hotwire (Turbo + Stimulus), import maps for JavaScript
- **Authentication:** Rails 8 built-in generator (simple), Devise (complex requirements)
- **Asset Pipeline:** Propshaft
- **Database:** PostgreSQL (default and recommended)
- **API Mode:** `ActionController::API` for JSON-only applications

## Repository Structure

```
ruby-on-rails/
  README.md                                  # Navigation table + learning objectives
  AGENTS.md                                  # This file
  00-foundations/                            # Programming fundamentals and Ruby context
    01-what-is-programming.md
    02-paradigms.md
    03-sequential-decision-iteration.md
    04-compiler-vs-interpreter.md
    05-what-is-ruby.md
    06-why-ruby-rails-why-not-x.md
  01-first-code/                             # Writing your first Ruby programs
    01-setup.md
    02-variables-and-types.md
    03-control-flow.md
    04-methods-and-blocks.md
    05-classes-and-modules.md
  02-core-ruby/                              # Intermediate Ruby concepts
    01-collections.md
    02-error-handling.md
    03-gems-and-bundler.md
    04-metaprogramming-basics.md
  03-rails-fundamentals/                     # Building with Rails
    01-rails-philosophy.md
    02-mvc-in-rails.md
    03-routing-and-controllers.md
    04-active-record.md
    05-views-and-hotwire.md
  04-production/                             # Production-ready patterns
    01-testing.md
    02-authentication.md
    03-background-jobs.md
    04-deployment.md
  05-workshop/                               # Full application project
    README.md
```
