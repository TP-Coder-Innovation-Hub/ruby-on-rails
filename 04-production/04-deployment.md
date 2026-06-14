# Deployment 

## Kamal 2: The Rails 8 Default

Kamal 2 is a container-based deployment tool created by the Rails team. It packages your application as a Docker image, deploys it to one or more servers, and manages zero-downtime rollouts.

Kamal replaces the need for platform-as-a-service (Heroku) or complex infrastructure-as-code (Terraform + Kubernetes) for most Rails applications.

## Setup

### 1. Install Kamal

```bash
gem install kamal
```

### 2. Initialize Configuration

```bash
cd your-rails-app
kamal init
```

This creates `config/deploy.yml`.

### 3. Configure deploy.yml

```yaml
# config/deploy.yml
service: myapp
image: your-registry/myapp

servers:
  web:
    - 192.168.1.100
  workers:
    - 192.168.1.101

registry:
  server: ghcr.io
  username:
    - KAMAL_REGISTRY_USERNAME
  password:
    - KAMAL_REGISTRY_PASSWORD

env:
  clear:
    RAILS_ENV: production
    RAILS_SERVE_STATIC_FILES: true
  secret:
    - RAILS_MASTER_KEY
    - DATABASE_URL

asset_path: /rails/public/assets

healthcheck:
  path: /up
  interval: 10
  timeout: 5

traefik:
  options:
    publish: "443:443"
    volume: "/etc/traefik/acme:/etc/traefik/acme"
  labels:
    traefik.http.routers.myapp.rule: "Host(`myapp.example.com`)"
    traefik.http.routers.myapp.tls: true
    traefik.http.routers.myapp.tls.certresolver: letsencrypt
```

### 4. Set Up Secrets

```bash
kamal env push RAILS_MASTER_KEY=your_master_key
kamal env push DATABASE_URL=postgres://user:pass@host/db
```

Secrets are stored as environment variables on the server. They are never committed to the repository.

### 5. Deploy

```bash
kamal setup    # First deploy: provisions server, deploys, runs migrations
kamal deploy   # Subsequent deploys: builds, pushes, deploys
```

## What Happens During a Deploy

1. **Build** -- Docker builds an image from your `Dockerfile`
2. **Push** -- The image is pushed to your container registry
3. **Pull** -- Servers pull the new image
4. **Health check** -- The new container starts and responds to `/up`
5. **Switch** -- Traefik (reverse proxy) routes traffic to the new container
6. **Stop** -- The old container is stopped

The switch is atomic. Users experience zero downtime.

## Dockerfile

Rails 8 generates a production-ready `Dockerfile`:

```dockerfile
FROM ruby:3.3

WORKDIR /rails

# Install dependencies
RUN apt-get update -qq && \
    apt-get install -y libpq-dev && \
    rm -rf /var/lib/apt/lists/*

# Install gems
COPY Gemfile Gemfile.lock ./
RUN bundle install

# Copy application
COPY . .

# Precompile assets
RUN bin/rails assets:precompile

EXPOSE 3000

CMD ["bin/rails", "server", "-b", "0.0.0.0"]
```

## Database Migrations

Kamal runs migrations automatically during deploy. If migrations fail, the deploy stops before traffic switches to the new version.

```yaml
# config/deploy.yml
deploy:
  migration:
    command: bin/rails db:migrate
```

## Rollback

If something goes wrong after deploy:

```bash
kamal rollback [version]
```

This redeploys the previous Docker image. It is instant because the image already exists on the server.

## Monitoring and Logs

```bash
# View logs from all app containers
kamal app logs

# View logs from a specific container
kamal app logs --grep "error"

# SSH into the server
kamal app exec bash

# Check app status
kamal app details
```

## Multiple Servers

Scale horizontally by adding more servers:

```yaml
servers:
  web:
    - 192.168.1.100
    - 192.168.1.101
    - 192.168.1.102
```

Kamal deploys to all servers simultaneously. Traefik load-balances between them.

## Production Checklist

- Set `RAILS_ENV=production`
- Set a strong `RAILS_MASTER_KEY`
- Use PostgreSQL (not SQLite) in production
- Enable HTTPS (Kamal + Traefik + Let's Encrypt handles this)
- Set up monitoring (logs, error tracking, uptime checks)
- Configure background workers (`workers:` in deploy.yml)
- Run database migrations during deploy
- Set up backup for your database
- Use environment variables for all secrets
- Enable rate limiting on public endpoints

## Alternatives to Kamal

| Tool | When to Use |
|------|------------|
| Kamal 2 | Default. Single server or small cluster. Docker-based. |
| Heroku | Smallest teams. Zero ops. Higher cost at scale. |
| AWS (ECS/Fargate) | Need AWS ecosystem integration. Auto-scaling. |
| Kubernetes | Large engineering orgs. Complex infrastructure needs. |
| Render/Fly.io | Platform-as-a-service. Simpler than raw servers. |

For most Rails applications, Kamal 2 is the right choice. It is the default for a reason.

Move to `05-workshop/README.md` for the final project.
