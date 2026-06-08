# Testing 

## Why Rails Teams Test Extensively

Rails applications run in production for years. Without tests, every change risks breaking existing functionality. Tests are not overhead. They are how you ship with confidence.

The Rails community has a strong testing culture. Most Rails applications have more test code than application code.

## RSpec Basics

RSpec is the most popular testing framework in the Rails ecosystem.

```ruby
# spec/models/post_spec.rb
RSpec.describe Post, type: :model do
  describe "validations" do
    it "requires a title" do
      post = Post.new(body: "Some content")
      expect(post).not_to be_valid
      expect(post.errors[:title]).to include("can't be blank")
    end

    it "requires a body" do
      post = Post.new(title: "Hello")
      expect(post).not_to be_valid
    end

    it "is valid with title and body" do
      post = Post.new(title: "Hello", body: "World")
      expect(post).to be_valid
    end
  end

  describe "scopes" do
    it "returns only published posts" do
      published = Post.create!(title: "P", body: "B", published: true)
      _draft = Post.create!(title: "D", body: "B", published: false)

      expect(Post.published).to eq([published])
    end
  end
end
```

Key matchers:

```ruby
expect(value).to eq(expected)
expect(value).to be_truthy
expect(value).to be_falsy
expect(value).to be_nil
expect(list).to include(item)
expect(list).to be_empty
expect { action }.to change(Post, :count).by(1)
expect { action }.to raise_error(ActiveRecord::RecordNotFound)
```

## FactoryBot

FactoryBot creates test data. It replaces manual object creation in tests.

```ruby
# spec/factories/posts.rb
FactoryBot.define do
  factory :post do
    title { "Test Post" }
    body { "This is the body of the test post." }
    published { true }
    user

    trait :draft do
      published { false }
    end

    trait :with_comments do
      after(:create) do |post|
        create_list(:comment, 3, post: post)
      end
    end
  end
end

# spec/factories/users.rb
FactoryBot.define do
  factory :user do
    name { "Test User" }
    email { "test@example.com" }
  end
end
```

Usage in tests:

```ruby
# Create and save to database
post = create(:post)
draft = create(:post, :draft)
post_with_comments = create(:post, :with_comments)

# Build without saving (faster, use when DB not needed)
post = build(:post)

# Create multiple
posts = create_list(:post, 5)

# Override attributes
post = create(:post, title: "Custom Title")
```

## Controller Tests (Request Specs)

Test the full request cycle: routing, controller, response.

```ruby
# spec/requests/posts_spec.rb
RSpec.describe "Posts", type: :request do
  let(:user) { create(:user) }

  before { sign_in user }

  describe "GET /posts" do
    it "returns a successful response" do
      create(:post, user: user)

      get posts_path

      expect(response).to have_http_status(:ok)
    end
  end

  describe "POST /posts" do
    it "creates a new post" do
      expect {
        post posts_path, params: { post: { title: "New", body: "Content" } }
      }.to change(Post, :count).by(1)

      expect(response).to redirect_to(Post.last)
    end

    it "renders new template with unprocessable content on failure" do
      post posts_path, params: { post: { title: "", body: "" } }

      expect(response).to have_http_status(:unprocessable_content)
    end
  end
end
```

## System Tests

System tests exercise the full application including JavaScript, in a real browser.

```ruby
# spec/system/posts_spec.rb
RSpec.describe "Posts", type: :system do
  it "creates a post and displays it" do
    user = create(:user)
    sign_in user

    visit posts_path
    click_link "New Post"

    fill_in "Title", with: "My First Post"
    fill_in "Body", with: "This is my first blog post."
    click_button "Create Post"

    expect(page).to have_text "Post created successfully"
    expect(page).to have_text "My First Post"
  end
end
```

## Running Tests

```bash
# Run all tests
bundle exec rspec

# Run a specific file
bundle exec rspec spec/models/post_spec.rb

# Run a specific test by line number
bundle exec rspec spec/models/post_spec.rb:12

# Run only model tests
bundle exec rspec spec/models/

# Run with format documentation
bundle exec rspec --format documentation
```

## Testing Strategy

| Layer | What to test | Type |
|-------|-------------|------|
| Model | Validations, scopes, associations, business methods | Model spec |
| Controller | Request/response, redirects, params | Request spec |
| System | User workflows, JavaScript interactions | System spec |
| Background jobs | Job execution, retries | Job spec |
| Services | Business logic encapsulation | Plain Ruby spec |

Prioritize model tests. They are fast and catch most bugs. Add request and system tests for critical user paths.

Move to `02-authentication.md` for securing your application.
