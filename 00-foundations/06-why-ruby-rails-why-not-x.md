# Why Ruby on Rails? Why Not X? 

## What Rails Is

Rails is a full-stack web application framework written in Ruby. It provides everything you need to build a database-backed web application: routing, controllers, models, views, background jobs, websockets, email, and deployment tooling. All in one framework. All with consistent conventions.

Created by David Heinemeier Hansson (DHH) at Basecamp in 2004. Extracted from a real application, not designed in a vacuum.

## Rails vs the Field

### Rails vs Django (Python)

- Both full-stack, both batteries-included
- Django is more explicit, Rails is more convention-driven
- Python dominates data science and ML; Ruby dominates developer-tooling startups
- Choose Rails if you value convention and speed-to-feature; choose Django if your team is stronger in Python

### Rails vs Spring Boot (Java)

- Spring Boot is enterprise-grade with massive ecosystem
- Rails is faster to prototype; Spring Boot scales more predictably for large engineering orgs
- Java's type system catches errors at compile time; Ruby's dynamism enables faster iteration
- Choose Rails for startups and mid-size products; choose Spring Boot when you need Java's enterprise ecosystem

### Rails vs Laravel (PHP)

- Nearly identical philosophy: convention over configuration, developer happiness
- Both ship with ORM, routing, migrations, queues, auth
- PHP runs on cheap shared hosting; Ruby needs a proper server
- Choose based on team skill. The frameworks are more alike than different.

### Rails vs Express (Node.js)

- Express is minimal -- you assemble your own stack
- Rails is opinionated -- the decisions are made for you
- Node.js excels at real-time, high-concurrency I/O; Rails excels at CRUD-heavy business logic
- Choose Express when you need fine-grained control; choose Rails when you want to ship features fast

## When to Choose Rails

- You are building a CRUD-heavy web application
- Your team values convention and consistency
- You want to ship an MVP in weeks, not months
- You are building a startup and need to move fast
- Your application is primarily server-rendered with some interactivity

## When Not to Choose Rails

- You need heavy real-time concurrent connections (consider Elixir/Phoenix)
- Your team has no Ruby expertise and no interest in learning it
- You are building a data pipeline or ML system (Python is the better ecosystem)
- You need microsecond-level response times (consider Go or Rust)
- Your application is primarily a single-page app with a thin API backend

## Honest Trade-offs

**Strengths:** Developer speed, convention, ecosystem maturity, community, full-stack out of the box.

**Weaknesses:** Runtime performance, memory usage, hiring pool (smaller than Python/Java/JS), global interpreter lock (GIL limits true parallelism in MRI Ruby).

**Bottom line:** Rails is the fastest path from idea to deployed web application for most CRUD applications. It is not the fastest-running framework. It is the fastest-developing framework. For most businesses, developer time is more expensive than server time.

Move to `01-first-code/01-setup.md` to install Ruby and write your first program.
