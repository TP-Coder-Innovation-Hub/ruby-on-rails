# AGENTS.md

## Context

This repository contains educational content for the "Ruby on Rails Developer" learning path, part of the TP-Coder Innovation Hub curriculum. The content targets developers at Entry, Mid, and Senior levels who want to learn or deepen their understanding of Ruby on Rails for building modern web applications.

## Audience

- Developers new to web frameworks exploring Rails as their first framework
- Developers with experience in other frameworks (Laravel, Django, Spring Boot) evaluating Rails
- Mid-level Rails developers looking to level up their architecture and performance skills
- Senior developers and engineering managers making technology adoption decisions
- Technical instructors creating curriculum or workshops from this material

## How to Help

- Reference Rails 8 features and conventions when generating code examples
- Use Ruby 3.3+ syntax and idioms (endless methods, pattern matching, data classes where relevant)
- Tag content with `[Entry]`, `[Mid]`, or `[Senior]` level indicators when adding new sections
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
  README.md        # Primary educational content (this learning path's core document)
  AGENTS.md        # Agent instructions and project context
```

As the learning path grows, the expected structure is:

```
ruby-on-rails/
  README.md                    # Fundamentals guide
  AGENTS.md                    # This file
  exercises/                   # Hands-on coding exercises
    01-ruby-basics/
    02-first-rails-app/
    03-active-record/
    04-hotwire-frontend/
    05-background-jobs/
    06-deployment/
  projects/                    # Full application projects
    blog-app/
    marketplace/
    saas-starter/
  reference/                   # Quick reference materials
    rails-cheatsheet.md
    ruby-cheatsheet.md
    deployment-guide.md
```
