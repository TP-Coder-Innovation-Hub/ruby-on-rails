# Views and Hotwire `[Mid]`

## ERB Basics

ERB (Embedded Ruby) is Rails' default templating language. Ruby code embedded in HTML.

```erb
<!-- app/views/posts/index.html.erb -->
<h1>All Posts</h1>

<% if @posts.any? %>
  <ul>
    <% @posts.each do |post| %>
      <li>
        <%= link_to post.title, post %>
        <span><%= time_ago_in_words(post.created_at) %> ago</span>
      </li>
    <% end %>
  </ul>
<% else %>
  <p>No posts yet.</p>
<% end %>

<%= link_to "New Post", new_post_path %>
```

Tags:

- `<%= expression %>` -- Output the result (HTML-escaped by default)
- `<% code %>` -- Execute code without output
- `<%= raw html %>` -- Output unescaped HTML (use carefully)

## Layouts

Every view renders inside a layout. The default layout is `app/views/layouts/application.html.erb`:

```erb
<!DOCTYPE html>
<html>
  <head>
    <title>Blog</title>
    <%= csrf_meta_tags %>
    <%= stylesheet_link_tag "application" %>
    <%= javascript_importmap_tags %>
  </head>
  <body>
    <nav>
      <%= link_to "Home", root_path %>
      <%= link_to "Posts", posts_path %>
    </nav>

    <main>
      <%= yield %>
    </main>
  </body>
</html>
```

`<%= yield %>` is where the action-specific view content is inserted.

## Partials

Partials are reusable view fragments. They reduce duplication.

```erb
<!-- app/views/posts/_post.html.erb -->
<article>
  <h2><%= link_to post.title, post %></h2>
  <p><%= truncate(post.body, length: 100) %></p>
  <small>By <%= post.user.name %></small>
</article>
```

Render a partial:

```erb
<!-- In any view -->
<%= render @post %>
<!-- Renders app/views/posts/_post.html.erb with post as local variable -->

<%= render partial: "posts/post", collection: @posts %>
<!-- Renders the partial once per item in @posts -->
```

## Helpers

Rails provides built-in helpers for common view logic:

```erb
<%= pluralize(@post.comments.count, "comment") %>
<!-- "1 comment" or "5 comments" -->

<%= number_to_currency(19.99) %>
<!-- "$19.99" -->

<%= time_ago_in_words(@post.created_at) %>
<!-- "2 hours ago" -->

<%= truncate(post.body, length: 100) %>
<!-- First 100 characters..." -->
```

Custom helpers go in `app/helpers/`:

```ruby
# app/helpers/application_helper.rb
module ApplicationHelper
  def page_title(title)
    content_tag(:title, "#{title} | Blog")
  end
end
```

## Hotwire: Modern Rails Frontend

Hotwire is Rails' answer to complex JavaScript. It gives you SPA-like interactivity without writing a separate frontend application.

Hotwire has two parts: **Turbo** and **Stimulus**.

### Turbo

Turbo intercepts link clicks and form submissions, sending requests via fetch instead of full page reloads.

**Turbo Drive** -- Enabled by default. All link clicks and form submissions become AJAX requests. The server sends back HTML, and Turbo swaps the `<body>` content. No JavaScript required.

**Turbo Frames** -- Update parts of the page independently:

```erb
<!-- app/views/posts/show.html.erb -->
<%= turbo_frame_tag "comments" do %>
  <h2>Comments</h2>
  <% @post.comments.each do |comment| %>
    <p><%= comment.body %></p>
  <% end %>

  <%= link_to "Add Comment", new_post_comment_path(@post) %>
<% end %>
```

When a link inside this frame is clicked, only the frame content updates. The rest of the page stays in place.

**Turbo Streams** -- Server-initiated DOM updates via WebSocket or in response to form submissions:

```ruby
# app/controllers/comments_controller.rb
def create
  @comment = @post.comments.create!(comment_params)

  respond_to do |format|
    format.turbo_stream
    format.html { redirect_to @post }
  end
end
```

```erb
<!-- app/views/comments/create.turbo_stream.erb -->
<%= turbo_stream.append "comments" do %>
  <%= render @comment %>
<% end %>
```

The new comment appears in the list without a page reload. No JavaScript written.

### Stimulus

Stimulus adds lightweight JavaScript behavior to HTML. Use it for client-side interactions that Turbo cannot handle: toggles, auto-complete, date pickers.

```html
<!-- app/views/posts/_form.html.erb -->
<div data-controller="preview">
  <textarea data-preview-target="input"
            data-action="input->preview#update"></textarea>
  <div data-preview-target="output"></div>
</div>
```

```javascript
// app/javascript/controllers/preview_controller.js
import { Controller } from "@hotwired/stimulus"

export default class extends Controller {
  static targets = ["input", "output"]

  update() {
    this.outputTarget.innerHTML = this.inputTarget.value
  }
}
```

Stimulus controllers are small, focused, and attached to HTML via `data-` attributes. No build step. No React. No Vue.

## The Hotwire Philosophy

- The server renders HTML, not JSON
- The browser updates DOM fragments, not the full page
- JavaScript is for behavior that cannot be done server-side
- The result feels like an SPA but is simpler to build and maintain

Move to `04-production/01-testing.md` for production-ready patterns.
