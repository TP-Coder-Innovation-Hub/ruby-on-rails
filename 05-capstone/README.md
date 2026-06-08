# Capstone Project `[Senior]`

Build a complete Rails application from scratch. Choose one of two projects: a blog platform or an e-commerce store.

## Option A: Blog Platform

### Requirements

**Users and Authentication**
- Sign up, sign in, sign out (Devise)
- User profiles with name, email, bio
- Authors can create, edit, delete their own posts
- Visitors can read published posts without signing in

**Posts**
- Title (required, max 200 characters)
- Body (required, supports basic formatting)
- Draft/published status
- Published posts appear on the homepage in reverse chronological order
- Draft posts are only visible to the author
- Slug-based URLs (e.g., `/posts/my-first-post` instead of `/posts/1`)

**Comments**
- Authenticated users can comment on published posts
- Comment body (required)
- Authors can delete comments on their own posts
- Display comment count on each post

**Background Jobs**
- Send welcome email when a user signs up
- Send notification email to the author when a new comment is posted
- Daily job to count and log total posts and comments

**Testing**
- Model tests for Post and Comment validations
- Request tests for all CRUD actions
- System test for the "create a post" workflow
- At least 80% model coverage

**Deployment**
- Deploy with Kamal 2
- PostgreSQL in production
- SSL/HTTPS enabled

### Data Model

```
User
  name: string
  email: string
  bio: text

Post
  title: string
  body: text
  slug: string (unique)
  published: boolean (default: false)
  user_id: foreign key

Comment
  body: text
  post_id: foreign key
  user_id: foreign key
```

## Option B: E-Commerce Store

### Requirements

**Users and Authentication**
- Sign up, sign in, sign out (Devise)
- Admin role (can manage products and orders)
- Customer role (can browse and order)

**Products**
- Name, description, price, image
- In-stock quantity
- Admin CRUD interface
- Public product listing with search

**Cart and Orders**
- Session-based shopping cart
- Add/remove items, adjust quantities
- Checkout creates an order
- Order status: pending, paid, shipped, completed
- Order confirmation email (background job)

**Background Jobs**
- Order confirmation email
- Low-stock notification to admins
- Daily sales report

**Testing**
- Model tests for Order and Product validations
- Request tests for checkout flow
- System test for "add to cart and checkout" workflow
- At least 80% model coverage

**Deployment**
- Deploy with Kamal 2
- PostgreSQL in production
- SSL/HTTPS enabled

### Data Model

```
User
  name: string
  email: string
  role: string (admin/customer)

Product
  name: string
  description: text
  price: decimal
  stock_quantity: integer
  image_url: string

Order
  status: string
  total: decimal
  user_id: foreign key

OrderItem
  quantity: integer
  price: decimal
  product_id: foreign key
  order_id: foreign key
```

## Implementation Steps

1. `rails new app_name -d postgresql`
2. Set up Devise for authentication
3. Generate models and run migrations
4. Define associations and validations
5. Build controllers and routes (RESTful)
6. Build views with ERB and Hotwire (Turbo + Stimulus)
7. Add background jobs for email and reporting
8. Write tests (models first, then requests, then system)
9. Set up Kamal 2 and deploy
10. Verify production: SSL, background workers, database

## Acceptance Criteria

- All tests pass (`bundle exec rspec`)
- Application deploys successfully to production
- A new user can sign up, create content, and receive email
- Code follows Rails conventions (no fighting the framework)
- No scaffold-generated code without customization

## Resources

- Rails Guides: https://guides.rubyonrails.org
- Devise documentation: https://github.com/heartcombo/devise
- Kamal documentation: https://kamal-deploy.org
- Hotwire documentation: https://hotwired.dev
