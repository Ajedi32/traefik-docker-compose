# Traefik config

This project hosts a simple config for Traefik intended to allow multiple services to be run off of a single host using docker-compose.

## Usage

```
docker-compose up
```

The dashboard is accessible at http://traefik.localhost/

The example service defined in `docker-compose.example.yml` is accessible at http://whoami.localhost/

### Production usage

```
docker-compose up --env-file=prod.env --detach
```
