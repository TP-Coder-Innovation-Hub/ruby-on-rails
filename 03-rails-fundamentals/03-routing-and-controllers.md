# Routing and Controllers `[Mid]`

## RESTful Routes

Rails routes HTTP requests to controller actions. The `resources` macro generates standard REST routes:

```ruby
# config/routes.rb
Rails.application.routes.draw do
  resources :posts
end
```

This single line creates seven routes:

| HTTP Verb | Path | Action | Purpose |
|-----------|------|--------|---------|
| GET | /posts | index | List all posts |
| GET | /posts/new | new | Show new post form |
| POST | /posts | create | Create a new post |
| GET | /posts/:id | show | Show a single post |
| GET | /posts/:id/edit | edit | Show edit form |
| PATCH | /posts/:id | update | Update a post |
| DELETE | /posts/:id | destroy | Delete a post |

Verify your routes at any time:

```bash
bin/rails routes
```

## Nested Resources

When resources belong to other resources:

```ruby
resources :posts do
  resources :comments, only: [:create, :destroy]
end
```

This generates:

- `POST /posts/:post_id/comments` -> `comments#create`
- `DELETE /posts/:post_id/comments/:id` -> `comments#destroy`

`only:` and `except:` restrict which routes are generated. Use them to avoid unused routes.

## Custom Routes

Not everything fits REST. Add individual routes for non-CRUD actions:

```ruby
Rails.application.routes.draw do
  resources :posts do
    member do
      post :publish     # POST /posts/:id/publish
    end
    collection do
      get :search       # GET /posts/search
    end
  end

  get "about", to: "pages#about"
  root "posts#index"
end
```

- `member` -- acts on a specific resource (includes `:id`)
- `collection` -- acts on the collection (no `:id`)
- `root` -- the homepage

## Controller Actions

A controller action receives the request, interacts with the model, and returns a response.

```ruby
class PostsController < ApplicationController
  before_action :authenticate_user!, except: [:index, :show]
  before_action :set_post, only: [:show, :edit, :update, :destroy]

  def index
    @posts = Post.published.order(created_at: :desc)
    respond_to do |format|
      format.html
      format.json { render json: @posts }
    end
  end

  def show
  end

  def new
    @post = Post.new
  end

  def create
    @post = Post.new(post_params)
    if @post.save
      redirect_to @post, notice: "Post created successfully"
    else
      render :new, status: :unprocessable_content
    end
  end

  def edit
  end

  def update
    if @post.update(post_params)
      redirect_to @post, notice: "Post updated"
    else
      render :edit, status: :unprocessable_content
    end
  end

  def destroy
    @post.destroy
    redirect_to posts_path, notice: "Post deleted"
  end

  private

  def set_post
    @post = Post.find(params[:id])
  end

  def post_params
    params.require(:post).permit(:title, :body, :published)
  end
end
```

## Strong Parameters

`post_params` is the strong parameters pattern. It whitelists which parameters are allowed through to the model. This prevents mass-assignment vulnerabilities.

```ruby
# Only these fields are permitted
params.require(:post).permit(:title, :body, :published)

# Nested parameters
params.require(:user).permit(:name, :email, profile_attributes: [:bio, :avatar])
```

Never pass `params` directly to model constructors.

## Before Actions

`before_action` runs code before specified actions. Common uses:

- Authentication: `before_action :authenticate_user!`
- Authorization: `before_action :verify_admin, only: [:destroy]`
- Loading resources: `before_action :set_post, only: [:show, :edit, :update, :destroy]`

## Rendering vs Redirecting

- **render** -- Sends a response directly (HTML template, JSON, file). The browser URL does not change.
- **redirect_to** -- Sends a 302 redirect. The browser makes a new request to the target URL.

```ruby
# Render the new template (validation failed, show form again)
render :new, status: :unprocessable_content

# Redirect to the post show page (successful create)
redirect_to @post
```

Rule: render after a failed save, redirect after a successful save. This prevents double-submission on refresh.

Move to `04-active-record.md` for Rails' ORM layer.
