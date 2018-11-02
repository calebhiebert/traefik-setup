# Traefik Setup

### Getting Started
Clone the repo from GitHub

```
git clone https://github.com/calebhiebert/traefik-setup
```

### Step 1 - Configue Docker Compose File
Edit your docker compose file to add your containers and [traefik rules](https://docs.traefik.io/configuration/backends/docker/#labels-overriding-default-behavior). **Make Sure you include the external proxy network in your own dockerfile**


### Backend + UI Example:
```yaml
networks:
  default:
    external:
      name: proxy
services:
  backend:
    restart: always
    build: .
    labels:
      - "traefik.backend=be"
      - "traefik.docker.network=proxy"
      - "traefik.enable=true"
      - "traefik.port=8090"
      - "traefik.frontend.rule=PathPrefixStrip:/api/"
      - "traefik.default.protocol=http"
      - "traefik.frontend.priority=20"
  ui:
    restart: always
    build: ../telegram-bot
    labels:
      - "traefik.backend=ui"
      - "traefik.docker.network=proxy"
      - "traefik.frontend.rule=Host:example.domain.com"
      - "traefik.enable=true"
      - "traefik.port=80"
      - "traefik.default.protocol=http"
      - "traefik.frontend.priority=10"
```

### Step 2 - Running the Application
1.  Modify your `docker-compose.yml` to add traefik labels
2.  Modify `traefik.toml`, setting the domain and letsencrypt email fields
3.  Run `./start.sh`
4.  Check docker containers are running `docker ps`


