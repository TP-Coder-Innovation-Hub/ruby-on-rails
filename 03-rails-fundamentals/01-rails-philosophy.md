# Rails Philosophy `[Mid]`

## Convention over Configuration

Rails' core principle: the framework makes decisions for you. If you follow the conventions, you write less code. If you fight the conventions, you write more code and lose the speed advantage.

What this means in practice:

- A model named `User` maps to a `users` table automatically
- A controller named `UsersController` handles routes starting with `/users`
- A file named `show.html.erb` is the template for the `show` action
- The primary key is `id`. Timestamps are `created_at` and `updated_at`

You do not configure these mappings. They happen because you followed the naming convention.

## "The Rails Way"

DHH coined the term "opinionated software." Rails has strong opinions about how web applications should be built:

- **Fat models, skinny controllers** -- Business logic belongs in the model (or service objects), not in controllers
- **RESTful by default** -- Resources map to standard HTTP verbs (GET, POST, PUT, DELETE)
- **Test-first culture** -- Rails ships with a testing framework. The community expects tests.
- **Monolith by default** -- Start with a single application. Extract services later if needed.
- **Server-rendered by default** -- Use Hotwire (Turbo + Stimulus) for interactivity, not a separate SPA framework

## Why This Enables Speed

A new Rails developer can build a functional CRUD application in minutes:

```bash
rails new blog
cd blog
bin/rails generate scaffold Post title:string body:text
bin/rails db:migrate
bin/rails server
```

Four commands. The application has:

- A database table (`posts`)
- A model with validations
- A controller with all CRUD actions
- RESTful routes
- HTML views for listing, creating, editing, and deleting posts
- A layout with navigation

The scaffold generated all of this because Rails knows the convention. No configuration files. No routing setup by hand. No ORM mapping declarations.

## When Convention Fights Back

Conventions break down when:

- Your application has non-CRUD workflows
- You need to integrate with a legacy database schema that does not follow Rails naming
- Your team has strong opinions that differ from Rails' defaults

In these cases, you can override conventions. Rails lets you configure almost anything. The cost is more code and more maintenance.

## The Extraction Pattern

Rails was extracted from Basecamp, a real product. This is not theoretical software. Every major feature in Rails was built to solve a real problem in a real application:

- **Active Record** -- Data access in Basecamp
- **Action Mailer** -- Email notifications in Basecamp
- **Action Cable** -- Real-time features in Basecamp
- **Solid Queue** -- Background processing without Redis
- **Kamal** -- Deployment of Basecamp's containerized services

This extraction-from-reality philosophy means Rails features are practical, not academic.

## Rails 8 Opinions (2026)

- Deployment: Kamal 2 (containers, zero-downtime)
- Frontend: Hotwire (Turbo + Stimulus, not React/Vue)
- Jobs: Solid Queue (database-backed, not Redis/Sidekiq by default)
- Assets: Propshaft (not Sprockets, not webpack)
- Auth: Built-in generator (or Devise for complex needs)
- Database: PostgreSQL (not MySQL, not SQLite in production)

Move to `02-mvc-in-rails.md` to understand the request lifecycle.
