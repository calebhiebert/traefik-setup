# Traefik Setup

### Getting Started
Clone the repo from GitHub

```
git clone https://github.com/calebhiebert/traefik-setup
```

### Step 1. Configue Docker Compose File
Edit the docker compose file to add your containers and [traefik rules](https://docs.traefik.io/configuration/backends/docker/#labels-overriding-default-behavior).


Backend + UI Example:
```yaml
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
1.  `Modify docker-compose.yml`
2.  Run `./start.sh`
3.  Check docker containers are running `docker ps`

**Restarting Application**
`docker-compose restart`

**Checking Logs**
`docker-compose logs -f`

### Issues - Lets Encrypt Certificates Problems
1. `rm acme.json`
2. Run `./start.sh`
3. Wait 5 Mintes and the Lets Encypt certs should be applied
