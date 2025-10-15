# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...

## Tutorial steps

### [Step 1.1: Install Rails](https://www.theodinproject.com/lessons/ruby-on-rails-installing-rails)

- `gem install rails`
- `rails -v`

### [Step 1.2: Install Yarn](https://www.theodinproject.com/lessons/ruby-on-rails-installing-rails)

- `npm install --global yarn`
-`yarn --version`

### [Step 1.3: Create the application](https://www.theodinproject.com/lessons/ruby-on-rails-installing-rails#step-13-create-the-application)

```
cd ~/your_odin_project_directory
rails tutorial_app
```

```
cd tutorial_app
rails generate scaffold car make:string model:string year:integer
```

```
rails db:migrate
```

### [Step 1.4: Start up your app](https://www.theodinproject.com/lessons/ruby-on-rails-installing-rails#step-13-create-the-application)

`rails server`
`rails console --sandbox` for an irb

### [Step 2: Git groundwork](https://www.theodinproject.com/lessons/ruby-on-rails-installing-rails)

### [Step 2.2 Initialize on GitHub, add the remote, and push](https://www.theodinproject.com/lessons/ruby-on-rails-installing-rails)

### [Step 2.3 Confirm Git is working correctly](https://www.theodinproject.com/lessons/ruby-on-rails-installing-rails)

### add routes:

```
  # in config/routes.rb
  ...
  resources :posts
  ...
```
- with this `rails routes` in the CLI outputs all routes.
- or 
