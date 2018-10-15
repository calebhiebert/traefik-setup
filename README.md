# Traefik Setup

### Getting Started
Clone the repo from GitHub

```
git clone https://github.com/calebhiebert/traefik-setup
```

### Step 1 - Configue Docker Compose File
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
1.  Modify `docker-compose.yml` to suit your needs
2.  Modify `traefik.toml` to suit your needs
3.  Run `./start.sh`
4.  Check docker containers are running `docker ps`

**Restarting Application**
`docker-compose restart`

**Checking Logs**
`docker-compose logs -f`

### Issues - Lets Encrypt Certificates Problems
1. `rm acme.json`
2. Run `./start.sh`
3. Wait 5 Minutes and the Lets Encypt certs should be applied

# Longer Example
```yaml
version: '3.6'
networks:
  default:
    external:
      name: proxy
services:
  traefik:
    image: traefik:alpine
    restart: always
    ports:
      - 80:80
      - 443:443
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./traefik.toml:/traefik.toml
      - ./acme.json:/acme.json
  backend:
    restart: always
    image: [Dockerhub Image]
    build: .
    labels:
      - 'traefik.backend=be'
      - 'traefik.docker.network=proxy'
      - 'traefik.enable=true'
      - 'traefik.port=3000'
      - 'traefik.frontend.rule=Host:example.domain.com'
      - 'traefik.default.protocol=http'
      - 'traefik.frontend.priority=20'
    environment:
      - NEVER=YOU
      - GONNA=HAVE
      - GIVE=BEEN
      - YA=RICK
      - UP=ROLLED
      - NEVER=HA
      - GONNA=HA
      - LET=HA
      - YOU=HA
      - DOWN=HA
      - REDIS=redis
  postgres:
    image: postgres:10-alpine
    restart: always
    ports:
      - 5432:5432
    labels:
      - 'traefik.postgres=postgres'
      - 'traefik.docker.network=proxy'
      - 'traefik.enable=true'
      - 'traefik.port=5432'
      - 'traefik.default.protocol=http'
    environment:
      - POSTGRES_USER=tm
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=tm
  redis:
    image: redis:3.2-alpine
    restart: always
    ports:
      - 6379:6379
    labels:
      - 'traefik.redis=redis'
      - 'traefik.docker.network=proxy'
      - 'traefik.enable=true'
      - 'traefik.port=6379'
      - 'traefik.default.protocol=http'
 ```



