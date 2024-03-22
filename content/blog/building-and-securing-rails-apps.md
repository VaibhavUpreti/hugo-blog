---
title: "Building and Securing Rails apps: Deploying with Docker on EC2, SSL with Cloudflare, and Domain Integration"
date: 2023-06-04
tags: ["rails"]
series: ["rails"]
featured: false
---


Create a new rails app

```ruby
rails new blog_app -d postgresql --skip-test
```

Remove windows platform from Gemfile gems.

`vim config/environments/production.rb`

set 
force_ssl to false
and assume_ssl to true


Create the Dockerfile.production


```Dockerfile
# Use a multi stage build for the builder
FROM --platform=$BUILDPLATFORM ruby:3.1.2 as builder

# Set production environment
ARG BUILDPLATFORM
ENV NODE_ENV=production
ENV RAILS_ENV=production

RUN curl -fsSL https://deb.nodesource.com/setup_16.x | bash -
RUN apt-get update -qq && apt-get install -y --no-install-recommends --auto-remove nodejs imagemagick libvips

RUN npm install --global yarn

WORKDIR /usr/src/app

RUN yarn install --frozen-lockfile --network-timeout 1000000 && yarn cache clean

COPY Gemfile* ./
ENV BUNDLE_DEPLOYMENT=true
# ENV BUNDLE_JOBS=4
ENV BUNDLE_WITHOUT=development:test

RUN bundle install \
   && rm -rf vendor/bundle/ruby/3.1.2/cache/*

COPY . .

RUN RAILS_ENV=production \
    SECRET_KEY_BASE=1 \
    DATABASE_URL=postgres://nulldb \
	DISABLE_FLIPPER=true \
    bundle exec rails assets:precompile && \
	yarn cache clean

RUN rm -rf node_modules spec

# Precompile the Bootsnap cache for the specified directories to optimize boot time in production.
RUN bundle exec bootsnap precompile --gemfile app/ lib/

RUN mkdir -p /public/

WORKDIR /app

FROM --platform=$BUILDPLATFORM ruby:3.1.2-slim as app

# Copy compiled assets and application from builder
COPY --from=builder /usr/src/app /usr/src/app

ENV RAILS_ENV=production
ENV NODE_ENV=production
ENV BUNDLE_PATH='vendor/bundle'
ENV RAILS_LOG_TO_STDOUT=true
ENV BUNDLE_DEPLOYMENT=true
ENV BUNDLE_WITHOUT="development:test"
ENV RAILS_SERVE_STATIC_FILES="true"

ARG REDIS_URL="redis://localhost:6379"

RUN apt-get update -qq && apt upgrade -y && apt-get install -y --no-install-recommends --auto-remove curl  \
   && curl -fsSL https://deb.nodesource.com/setup_16.x | bash - \
   && apt-get update -qq && apt-get install -y --no-install-recommends --auto-remove nodejs imagemagick libpq5 libcurl4 libjemalloc2  \
   && apt-get clean \
   && rm -rf /var/cache/apt/archives/* \
   && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
   && truncate -s 0 /var/log/*log

ENV LD_PRELOAD="libjemalloc.so.2"
ENV MALLOC_CONF="dirty_decay_ms:1000,narenas:2,background_thread:true"

WORKDIR /usr/src/app

ENTRYPOINT ["/usr/src/app/bin/docker-entrypoint"]

EXPOSE 3000
```

Github Actions

```md
name: Docker publish

on:
  schedule:
    - cron: '26 5 * * *'
  push:
    branches: [ main ]
  pull_request:
    branches: [ master ]
  workflow_call:

env:
  # Use docker.io for Docker Hub if empty
  REGISTRY: ghcr.io
  # github.repository as <account>/<repo>
  IMAGE_NAME: ${{ github.repository }}

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
      # This is used to complete the identity challenge
      # with sigstore/fulcio when running outside of PRs.
      id-token: write

    steps:
      - name: Checkout repository with submodules
        uses: actions/checkout@v3
        with:
          submodules: recursive

      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'

            # - name: Copy sample database configuration
            #  run: cp config/database.example.yml config/database.yml

      # Install the cosign tool except on PR
      # https://github.com/sigstore/cosign-installer
      - name: Install cosign
        if: github.event_name != 'pull_request'
        uses: sigstore/cosign-installer@main
        with:
          cosign-release: 'v1.7.1'

      - name: Set up QEMU
        uses: docker/setup-qemu-action@v2
        with:
          platforms: linux/amd64,linux/arm64

      - name: Setup Docker buildx
        uses: docker/setup-buildx-action@v2

      # Login against a Docker registry except on PR
      # https://github.com/docker/login-action
      - name: Log into registry ${{ env.REGISTRY }}
        if: github.event_name != 'pull_request'
        uses: docker/login-action@v2
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      # Extract metadata (tags, labels) for Docker
      # https://github.com/docker/metadata-action
      - name: Extract Docker metadata
        id: meta
        uses: docker/metadata-action@v4
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}

      - name: LowerCase repository name
        run: echo IMAGE_REPOSITORY=$(echo ${{ github.repository }} | tr '[:upper:]' '[:lower:]') >> $GITHUB_ENV

      # Build and push Docker image with Buildx (don't push on PR)
      # https://github.com/docker/build-push-action
      - name: Build and push Docker image
        id: build-and-push
        uses: docker/build-push-action@v3
        with:
          file: Dockerfile.production
          context: .
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ env.REGISTRY }}/${{ env.IMAGE_REPOSITORY }}:${{ github.sha }},${{ env.REGISTRY }}/${{ env.IMAGE_REPOSITORY }}:latest
          labels: ${{ steps.meta.outputs.labels }}
          platforms: linux/amd64 #, linux/arm64
          cache-from: type=gha
          cache-to: type=gha,mode=max

```

Push the changes




Generating Scaffolds

```ruby
rails g scaffold Article title:string content:text
rails g scaffold Category name:string
rails g scaffold Categorization article:references category:references
```

Create index route
```ruby
rails g controller home index
```

config/routes.rb

```ruby
root 'home#index'
```

Declare a variable like and it prints on the controller

Explain models and controllers


add relations to models

app/models/article.rb


```bash
has_many :categorizations
has_many :categories, through: :categorizations, dependent: :destroy
```

app/models/category.rb

```bash
has_many :categorizations
has_many :articles, through: :categorizations, dependent: :destroy
```

rails console show relations between objects after creating.


app/views/home/index.html.erb

```ruby

<%= link_to "Articles", articles_path, class: "inline-block"%>
<%= link_to "Categories", categories_path, class: "inline-block"%>
```

Making it look neat! Customise Views

Explore models/app/controllers

## Authentication

Install devise

```ruby
bundle add devise
rails generate devise:install
rails g devise User
```





## Attachments in Rails


## Some super duper cool feature build it




## Deployment


### Bootstrap server

1. Spin up a new t2.micro EC2 instance

Bootstrap script
```bash
curl -fsSL https://gist.githubusercontent.com/VaibhavUpreti/9a5ea6dce660d6775163c8a6f7ccaa30/raw/2560907d38d409170ae58e00418c13abd3ded942/bootstrap-rails-docker-ec2.sh | bash

```

2. Run bash script to install

- docker
- postgresql@15
- postgres lib tools
- Caddy : reverse proxy



Install docker
```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

sudo apt install docker-ce

sudo usermod -aG docker ${USER}
```

Install Postgresql@15

```bash
sudo apt install wget ca-certificates
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'

sudo apt update
sudo apt install postgresql postgresql-contrib

```

Install Caddy
```bash
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy

```



Exit the terminal and ssh again to reload changes.
### updating postgresql configs

1. Update default role
Update postgres role

sudo -u postgres psql

ALTER USER postgres WITH PASSWORD 'postgres';
ALTER USER postgres WITH SUPERUSER;



2. 
`sudo vim /etc/postgresql/*/main/postgresql.conf`

```bash
# prefer this
listen_addresses = '*' 
# or
listen_addresses = 'localhost, 172.17.0.1' 

```


`sudo vim /etc/postgresql/*/main/pg_hba.conf`

Allow all hosts to login within the postgres cluster
```bash
host    all             all              0.0.0.0/0                       scram-sha-256
host    all             all              ::/0                            scram-sha-256

```

`sudo systemctl restart postgresql`
Test configuration

`sudo -i -u postgres`
`psql -h 172.17.0.1`


Docker Login

docker login ghcr.io

enter: username, ghcr access token(password option now deprecated)-find under developer settings in github profile settings tab.






### Run the application


```bash
docker run -d -p 3000:3000 --network host \
  -e RAILS_ENV=production \
  -e SECRET_KEY_BASE="vionreinvroiv" \
  -e DATABASE_URL="postgresql://postgres:postgres@172.17.0.1:5432/blog_app_production" \
  image_id \
  ./bin/rails server


```


### Setup Caddy

`cd /etc/caddy`

`sudo vim Caddyfile`

```bash
blog.vaibhavupreti.me:443 {
	  file_server
	  reverse_proxy localhost:3000
}

```
`sudo caddy reload`
`sudo systemctl restart caddy`


Add ip to A record in cloudflare.


## Security

