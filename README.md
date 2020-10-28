Reverse proxy setup for local development using [Docker](https://www.docker.com/) and [Traefik](https://traefik.io/traefik/).

# Requirements
You should have a working Docker setup with docker-compose included. See https://docs.docker.com for details.

# Setup
```
docker network create traefik
docker-compose up -d
```

# Usage
To enable the local reverse proxy for your project on a custom domain, you have to add a few labels and the `traefik` network to the container you want to expose.

Here is a basic `docker-compose.yml` example on how to configure the reverse proxy on the domain `domain.example`:
```
version: '2.4'
services:
  example:
    image: nginx
    labels:
      - "traefik.docker.network=traefik"
      - "traefik.enable=true"
      - "traefik.http.routers.example.rule=Host(`domain.example`)"
    networks:
      - default
      - traefik
```

For further information on the labels, see the official Traefik documentation: https://doc.traefik.io/traefik/providers/docker/

Do not forget to add `domain.example` to your local DNS server or your `hosts` file to resolve to localhost.
