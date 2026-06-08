# Authentication `[Senior]`

## What Authentication Provides

Authentication answers: "Who is this user?" It involves:

- Registration (creating an account)
- Login (verifying identity with email/password)
- Session management (keeping the user logged in)
- Logout (ending the session)

## Devise

Devise is the most widely used authentication solution for Rails. It provides a complete, battle-tested authentication system.

### Installation

```ruby
# Gemfile
gem "devise"
```

```bash
bundle install
bin/rails generate devise:install
bin/rails generate devise User
bin/rails db:migrate
```

The generator creates a `User` model with email, encrypted_password, and other authentication fields. It also creates the routes, controllers, and views needed for sign-up, sign-in, sign-out, and password reset.

### Usage in Controllers

```ruby
class ApplicationController < ActionController::Base
  # Require authentication for all actions
  before_action :authenticate_user!

  # Skip authentication for specific actions
  # skip_before_action :authenticate_user!, only: [:index, :show]
end
```

### Usage in Views

```erb
<% if user_signed_in? %>
  <p>Hello, <%= current_user.name %></p>
  <%= button_to "Sign Out", destroy_user_session_path, method: :delete %>
<% else %>
  <%= link_to "Sign In", new_user_session_path %>
  <%= link_to "Sign Up", new_user_registration_path %>
<% end %>
```

### Protecting Resources

```ruby
class PostsController < ApplicationController
  before_action :authenticate_user!
  before_action :set_post, only: [:show, :edit, :update, :destroy]
  before_action :authorize_user, only: [:edit, :update, :destroy]

  def create
    @post = current_user.posts.build(post_params)
    if @post.save
      redirect_to @post, notice: "Post created"
    else
      render :new, status: :unprocessable_content
    end
  end

  private

  def authorize_user
    redirect_to root_path, alert: "Not authorized" unless @post.user == current_user
  end
end
```

`current_user` is available in all controllers and views when a user is signed in.

## Sessions

Devise manages sessions using cookies. The session stores a signed, encrypted user ID. On each request, Devise looks up the user from this ID.

Session configuration:

```ruby
# config/initializers/devise.rb
Devise.setup do |config|
  config.mailer_sender = "noreply@example.com"
  config.timeout_in = 30.minutes
  config.remember_for = 2.weeks
  config.sign_out_via = :delete
end
```

## Password Hashing

Devise uses `bcrypt` for password hashing. Passwords are never stored in plain text.

```ruby
# What Devise does internally:
# 1. User submits password: "mysecretpassword"
# 2. bcrypt generates a salt and hashes the password
# 3. The hash is stored: "$2a$12$R9h/cIPz0gi.URNNX3kh2OPST9et..."
# 4. On login, bcrypt compares the submitted password against the stored hash
```

Never roll your own password hashing. Use Devise (which uses bcrypt) or Rails 8's built-in `has_secure_password`:

```ruby
# Alternative to Devise for simple needs
class User < ApplicationRecord
  has_secure_password
end

# Usage
User.create(email: "alice@example.com", password: "secret")
user.authenticate("secret")  # => user
user.authenticate("wrong")   # => false
```

## Authorization vs Authentication

- **Authentication** -- Who are you? (login, sessions)
- **Authorization** -- What are you allowed to do? (roles, permissions)

Devise handles authentication. For authorization, use a gem like Pundit:

```ruby
# app/policies/post_policy.rb
class PostPolicy
  attr_reader :user, :post

  def initialize(user, post)
    @user = user
    @post = post
  end

  def update?
    user.admin? || post.user == user
  end

  def destroy?
    user.admin?
  end
end
```

```ruby
# app/controllers/posts_controller.rb
def update
  authorize @post
  # ...
end
```

## Security Checklist

- Use HTTPS in production (Kamal 2 handles this)
- Never log passwords or session tokens
- Use strong parameters (never pass `params` directly)
- Set secure, HttpOnly, SameSite flags on cookies
- Rate-limit login attempts
- Use Devise's `confirmable` module for email verification
- Keep Devise and Rails updated for security patches

Move to `03-background-jobs.md` for handling work outside the request cycle.
