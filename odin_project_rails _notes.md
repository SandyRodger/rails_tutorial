## A Railsy Web Refresher

### The basics of HTTP.
### The 4 most commonly used HTTP verbs.
### The 7 RESTful routes of Rails.
### The different components of a URL.
### The basics of MVC.
### What an API is.
### What “cookies” and “sessions” are.
### The difference between “authentication” and “authorization”.
  - [devise gem](https://github.com/heartcombo/devise)

## [Routing](https://www.theodinproject.com/lessons/ruby-on-rails-routing)

`rails routes` to get a list of the routes
- in your config/routes.rb include the following to set this as your landing page:
  - `root to: "kittens#index"  #kittens controller, index action (method)`.

### Configuring a root route.
- 7 main REST actions:
  - index all posts
  - show a post
  - new post page
  - create new post (post the above)
  - summon editing page
  - update -> so post the above
  - destroy

```
# in config/routes.rb
...
resources :posts
...
```

### Configuring RESTful routes for a resource.
### Configuring customized routes for a resource.
### The 7 RESTful controller actions.
### How to obtain a list of all possible routes for your current Rails application.
### Helper methods to create a navigation link on your webpage.

- `link_to "Edit this post", edit_post_path(3) # don't hardcode 3!`

#### Routes go to controller actions!

```
 # in app/controllers/posts_controller.rb
  class PostsController < ApplicationController

    def index
      # code to grab all posts so they can be
      # displayed in the Index view (index.html.erb)
    end

    def show
      # code to grab the proper Post so it can be
      # displayed in the Show view (show.html.erb)
    end

    def new
      # code to create an empty post and send the user
      # to the New view for it (new.html.erb), which will have a
      # form for creating the post
    end

    def create
      # code to create a new post based on the parameters that
      # were submitted with the form (and are now available in the
      # params hash)
    end

    def edit
      # code to find the post we want and send the
      # user to the Edit view for it (edit.html.erb), which has a
      # form for editing the post
    end

    def update
      # code to figure out which post we're trying to update, then
      # actually update the attributes of that post. Once that's
      # done, redirect us to somewhere like the Show page for that
      # post
    end

    def destroy
      # code to find the post we're referring to and
      # destroy it.  Once that's done, redirect us to somewhere fun.
    end
  end
```

- specify which you want with `only` and don't want using `except`

```
  resources :posts, only: [:index, :show]
  resources :users, except: [:index]
```
