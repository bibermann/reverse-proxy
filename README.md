# reverse-proxy

Docker Compose project for [GitHub » nginx-proxy / acme-companion](https://github.com/nginx-proxy/acme-companion).

## Setup

- Configuration
    ```bash
    # Persist data volumes in a specific directory
    PREFIX=./data/

    # Fallback email for missing LETSENCRYPT_EMAIL in proxied containers
    DEFAULT_EMAIL=name@example.com
    
    # Shared network for this and all proxied containers
    NETWORK=reverse-proxy
    
    echo "PREFIX=$PREFIX" >> .env
    echo "DEFAULT_EMAIL=$DEFAULT_EMAIL" >> .env
    echo "NETWORK=$NETWORK" >> .env
    sudo mkdir -p ${PREFIX}{conf,vhost,html,certs,acme}
    sudo docker network create $NETWORK
    ```
- Initialization/Update
    ```bash
    # Get latest nginx template
    curl https://raw.githubusercontent.com/nginx-proxy/nginx-proxy/main/nginx.tmpl > nginx.tmpl

    # Get latest docker images
    sudo docker compose pull
    ```

## Run

```bash
sudo docker compose up -d
```

## Stop

```bash
sudo docker compose down
```

## Status

```bash
sudo docker compose exec acme-companion /app/cert_status
```

## Setup proxy in a container

- Network: Add the proxied container to the network stored in your `.env` file.
    Helper to copy necessary docker compose lines to X selection:
    ```bash
    xclip <<EOF
    networks:
      default:
        external: true
        name: $(source .env; echo $NETWORK)
    EOF
    ```
- Environment:
    Set the following environment variables in the proxied containers:
    - `VIRTUAL_HOST`
    - `VIRTUAL_PORT` (optional, defaults to `80`)
    - `LETSENCRYPT_HOST` (must equal the value of `VIRTUAL_HOST`)
    - `LETSENCRYPT_EMAIL` (optional)

## References

- https://github.com/nginx-proxy/acme-companion
