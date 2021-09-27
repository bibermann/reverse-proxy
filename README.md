# reverse-proxy

Docker Compose project for [GitHub » nginx-proxy / acme-companion](https://github.com/nginx-proxy/acme-companion).

## Setup

- Configuration
    ```bash
    # Persist data volumes in a specific directory
    PREFIX=./data/

    # Fallback email for missing LETSENCRYPT_EMAIL in proxied containers
    DEFAULT_EMAIL=name@example.com
    
    echo "PREFIX=$PREFIX" >> .env
    echo "DEFAULT_EMAIL=$DEFAULT_EMAIL" >> .env
    sudo mkdir -p ${PREFIX}{conf,vhost,html,certs,acme}
    ```
- Initialization/Update
    ```bash
    # Get latest nginx template
    curl https://raw.githubusercontent.com/nginx-proxy/nginx-proxy/main/nginx.tmpl > nginx.tmpl

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

## Use

Set the following environment variables in the proxied containers:
- `VIRTUAL_HOST`
- `VIRTUAL_PORT` (optional, defaults to `80`)
- `LETSENCRYPT_HOST` (must equal the value of `VIRTUAL_HOST`)
- `LETSENCRYPT_EMAIL` (optional)

## References

- https://github.com/nginx-proxy/acme-companion
