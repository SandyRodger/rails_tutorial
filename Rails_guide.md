# [Rails Guide](https://guides.rubyonrails.org/getting_started.html)

## [Introduction](https://guides.rubyonrails.org/getting_started.html#introduction)
## [Rails Philosophy](https://guides.rubyonrails.org/getting_started.html#rails-philosophy)
## [Creating a New Rails App](https://guides.rubyonrails.org/getting_started.html#creating-a-new-rails-app)
## [Prerequisites]()
## [Creating Your First Rails App](https://guides.rubyonrails.org/getting_started.html#creating-a-database-model)
## [Directory Structure]()
## [Model-View-Controller Basics]()
## [Hello, Rails!]()
## [Autoloading in Development]()
## [Creating a Database Model]()
## [Database Migrations](https://guides.rubyonrails.org/getting_started.html#database-migrations)
## [Running Migrations](https://guides.rubyonrails.org/getting_started.html#running-migrations )
## [Rails Console]()
## [Active Record Model Basics](https://guides.rubyonrails.org/getting_started.html#active-record-model-basics)
## [Creating Records](https://guides.rubyonrails.org/getting_started.html#creating-records)
`product = Product.new(name: "T-Shirt")`
`product.save`

## [Querying Records](https://guides.rubyonrails.org/getting_started.html#querying-records)

## [Filtering & Ordering Records]()
## [Finding Records](https://guides.rubyonrails.org/getting_started.html#finding-records)



## [Updating Records](https://guides.rubyonrails.org/getting_started.html#updating-records)
## [Deleting Records]()
## [Validations]()
## [A Request's Journey Through Rails]()
## [Routes]()
## [Parts of a URL]()
## [HTTP Methods and Their Purpose]()
## [Rails Routes]()
## [Routes Command]()
## [Controllers & Actions]()
## [Making Requests]()
## [Instance Variables]()
## [CRUD Actions]()
## [Showing Individual Products]()
## [Creating Products]()
## [Editing Products]()
## [Deleting Products]()
## [Adding Authentication]()
## [Adding Log Out]()
## [Allowing Unauthenticated Access]()
## [Showing Links for Authenticated Users Only]()
## [Caching Products]()
## [Rich Text Fields with Action Text]()
## [File Uploads with Active Storage]()
## [Internationalization (I18n)]()
## [Action Mailer and Email Notifications]()
## [Basic Inventory Tracking]()
## [Adding Subscribers to Products]()
## [In Stock Email Notifications]()
## [Extracting a Concern]()
## [Unsubscribe Links]()
## [Adding CSS & JavaScript]()
## [Propshaft]()
## [Import Maps]()
## [Hotwire]()
## [Testing]()
## [Fixtures]()
## [Testing Emails]()
## [Consistently Formatted Code with RuboCop]()
## [Security]()
## [Continuous Integration with GitHub Actions]()
## [Deploying to Production](https://guides.rubyonrails.org/getting_started.html#deploying-to-production)

- Rails deployment tool is caled Kamal. Kamal uses Docker containers to run the app and deploy it.
- Rails by default makes a production ready Dockerfile, which Kamal will use to build the Docker image, creating a containerized version of your app with its dependencies and configurations.
- This Dockerfile uses THruster to compress and serve assest efficiently
- To deploy Kamal we need:
  - a server running Ubuntu LTS:
    - 1GB RAM <
    - wiht LTS (Long Term Support)
    - I will use Digital ocean
  - A Docker Hub account & access token. Dockerhub stores the image of the application so it can be downlaoded and run on the server.

Steps:
  - on DockerHub create a repo for your app image. Call this repo "store" -> TICK
  - Open `config/deploy.yml` and replace:
    -  `192.168.0.1` with your server's IP address (209.97.143.198) -> TICK
    -  `your-user` with your Docker Hub username -> TICK
    -  Create an access token with Read & Write permissions on Docker's website so Kamal can push the Docker image for your application.
      -  `docker login -u sandy851`
      -  password: [check KeePassXC]
    -  Then export the access token in the terminal so Kamal can find it with the following command:
      -  `export KAMAL_REGISTRY_PASSWORD=your-access-token`
      -  run the following command to set up your server and deploy your app for the first time: `bin/kamal setup`

ERROR: `  ERROR (Net::SSH::AuthenticationFailed): Exception while executing on host 209.97.143.198: Authentication failed for user root@209.97.143.198`

DEBUG:
- Can I SSH in manually with `ssh root@209.97.143.198` ?
  - no -> `root@209.97.143.198: Permission denied (publickey).`
- I need to ensure that my key is on the new droplet
  - I'll try SSH key 25519, becasue I have recorded the password.
  - I will copy the public key to the droplet thus:
    - `ssh-copy-id -i ~/.ssh/id_ed25519.pub root@209.97.143.198`
  - ERROR: `root@209.97.143.198: Permission denied (publickey).`
    - so the droplet only accepts keys it already knows.
  - I will try to add the key manually via DigitalOcean's web console

  export KAMAL_REGISTRY_USER="sandy851"
export KAMAL_REGISTRY_PASSWORD=[check KeePassXC]

Summary of debugging:
  1. SSH to the droplet was failing:
    - `Net::SSH::AuthenticationFailed`
    - Fix: use DO's browser console to manually paste the correct public key:
      - `/root/.ssh/authorized_keys`
  2. Docker Hub login failed (unauthorised)
     - `docker login ... unauthorized: incorrect username or password`
     - CAUSE: `KAMAL_REGISTRY_PASSWORD` was set to a placeholder instead of your real DOcker Hub access token.
     - FIX: Create a new token in Docker Hub/account/security
       - Test manually with `docker login -u sandy851`
       - export correct env vars.
  3. Git Head Error:
    - symptom: `fatal : ambiguous argument 'HEAD': unknown revision
     - Cause: your Rails had no git commits, so HEAD did not exist
     - Fix: `git add .` `git commit -m "Initial commit for Kamal deploy"
   4. Docker daemon on your Mac not running
      - Symptom: `Cannot connect to the Docker daemon at unix:///Users/sandyboy/.docker/run/docker.sock`
      - Cause Docker desktop wasn't running
      - Fix: open Docker desktop

      [ ... then an infinite black hole of ChatGPT solutons, each promising to be the last one. I'm going to try human message boards ... ]

- Google: `docker exit status: 32000`
  - export KAMAL_REGISTRY_PASSWORD="********************" <- check KeePassXC
    - source ~/.bashrc
  
## [Adding a User to Production]()
## [Background Jobs using Solid Queue]()
What's Next?
