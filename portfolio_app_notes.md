# Steps

## 1

rails new portfolio --database=postgresql
cd portfolio

## 2 

- add these to gemfile:

```
gem "pagy"
gem "image_processing", "~> 1.2"
gem "aws-sdk-s3", require: false
gem "tailwindcss-rails"   # optional but recommended
```

- then run these commands

```
bundle install
bin/rails tailwindcss:install   # optional
bin/rails active_storage:install
bin/rails db:create db:migrate
git add . && git commit -m "init portfolio app"
```

## 3 generate project model

```
bin/rails generate model Project title:string slug:string short:text body:text tech:string year:integer repo_url:string demo_url:string featured:boolean
bin/rails db:migrate
```

## 4 Wire active model for screetshots

```app/models/project.rb
class Project < ApplicationRecord
  has_many_attached :images
  validates :title, :slug, presence: true
  validates :slug, uniqueness: true

  scope :featured, -> { where(featured: true) }
end
```

## 5 Routes, controller, views

```config/routes.rb
Rails.application.routes.draw do
  root "projects#index"
  resources :projects
end
```

- create controller:

```app/controllers/projects_controller.rb
class ProjectsController < ApplicationController
  include Pagy::Backend

  def index
    @pagy, @projects = pagy(Project.order(featured: :desc, year: :desc), items: 6)
  end

  def show
    @project = Project.find_by!(slug: params[:id])
  end

  def new
    @project = Project.new
  end

  def create
    @project = Project.new(project_params)
    if @project.save
      redirect_to @project, notice: "Project created."
    else
      render :new, status: :unprocessable_entity
    end
  end

  private

  def project_params
    params.require(:project).permit(:title, :slug, :short, :body, :tech, :year, :repo_url, :demo_url, :featured, images: [])
  end
end
```

- create the following:

```app/views/projects/index.html.erb
<h1>My Projects</h1>

<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
  <% @projects.each do |p| %>
    <div class="card">
      <% if p.images.attached? %>
        <%= link_to project_path(p.slug) do %>
          <%= image_tag p.images.first.variant(resize_to_limit: [600,400]), alt: p.title, class: "w-full h-40 object-cover" %>
        <% end %>
      <% end %>
      <h2><%= link_to p.title, project_path(p.slug) %></h2>
      <p><%= truncate(p.short, length: 110) %></p>
      <p><small><%= p.tech %> • <%= p.year %></small></p>
    </div>
  <% end %>
</div>

<div class="mt-6">
  <%= pagy_nav(@pagy) %>
</div>
```

- and the following:

```app/views/projects/show.html.erb
<article>
  <h1><%= @project.title %></h1>
  <p><small><%= @project.tech %> • <%= @project.year %></small></p>
  <div class="gallery">
    <% @project.images.each do |img| %>
      <%= image_tag img.variant(resize_to_limit: [1200,800]), alt: @project.title %>
    <% end %>
  </div>

  <section class="mt-4">
    <p><%= simple_format(@project.body) %></p>
  </section>

  <p class="mt-4">
    <% if @project.demo_url.present? %>
      <%= link_to "Live demo", @project.demo_url, target: "_blank", rel: "noopener" %>
    <% end %>
    <% if @project.repo_url.present? %>
      • <%= link_to "Source", @project.repo_url, target: "_blank", rel: "noopener" %>
    <% end %>
  </p>

  <%= link_to "Back to projects", root_path %>
</article>
```

## 6 Admin UI

- use the new/edit controller views above and protect them with HTTP basic auth in ApplicationController or a very simple password:

```app/controllers/application_controller.rb
class ApplicationController < ActionController::Base
  before_action :require_admin, if: -> { request.path.starts_with?("/projects/new") || request.path.match?("/projects/.*/edit") }

  private

  def require_admin
    authenticate_or_request_with_http_basic do |user, pass|
      user == ENV["ADMIN_USER"] && pass == ENV["ADMIN_PASS"]
    end
  end
end
```

## 7 Make slugs easy

```app/models/project.rb
before_validation :set_slug, on: :create

def set_slug
  self.slug ||= title.to_s.parameterize
end
```
