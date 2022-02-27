# Traefik config for docker-compose

This project hosts a simple config for Traefik intended to allow multiple services to be run off of a single host using docker-compose.

## Usage

To start the server during development:

```
docker-compose up
```

The dashboard is accessible at http://traefik.localhost/

The example service defined in `docker-compose.example.yml` is accessible at http://whoami.localhost/

See [production usage](#production-usage) below for additional setup needed for production.

## Configuring a project to use Traefik

To configure another project to be served by Traefik, you just need to attach it to the external `traefik` network and add a few labels to tell Traefik how to route trafic to the containers:

```
services:
  my-service:
    networks:
      - default
      - traefik
    labels:
      - traefik.enable=true
      - traefik.http.routers.my-service.rule=Host(`my-service.example.com`)

networks:
  traefik:
    name: traefik
    external: true
```

See [`docker-compose.example.yml`](docker-compose.example.yml) for a complete, working example with further documentation.

For detailed documentation, see the [Traefik documentation](https://doc.traefik.io/traefik/providers/docker/).

## Production usage

### Setting the dashboard password

In production, you must first set up an account which will be used to access the dashboard:

```
# Create file
touch config/dashboard_users.htpasswd
chmod 600 config/dashboard_users.htpasswd

# Install htpasswd
sudo apt install apache2-utils

# Update password for user "admin"
htpasswd -BC 7 config/dashboard_users.htpasswd admin
```

Note: Bcrypt cost function can be tuned with `time htpasswd -nbBC $cost admin testpass`. Keep in mind that with basic auth this cost is paid on every request, including requests for subresources.

When updating this file, you will need to reload Traefik for the changes to take effect.

### Running Traefik

```
docker-compose --env-file=prod.env up --detach
```
