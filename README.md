# Traefik (solo SecureApprove)

Este directorio contiene un proxy Traefik mínimo para publicar **SecureApprove**.

## Requisitos

- Docker + Docker Compose
- La app de SecureApprove (en otro proyecto) debe estar en la **misma red Docker** que Traefik.
- Certificados TLS disponibles como:
  - `./certs/secureapprove/secureapprove.com.crt`
  - `./certs/secureapprove/secureapprove.com.key`

## Uso

1) Crear la red externa (una sola vez):

```bash
docker network create secureapprove_proxy
```

2) Levantar Traefik:

```bash
docker compose up -d
```

## Labels mínimos esperados (en el proyecto SecureApprove)

Asegurate de:
- conectar los containers a la red `secureapprove_proxy`
- agregar labels Traefik (ejemplo):

```yaml
labels:
  - traefik.enable=true

  # Router HTTPS
  - traefik.http.routers.secureapprove.entrypoints=websecure
  - traefik.http.routers.secureapprove.rule=Host(`secureapprove.com`) || Host(`www.secureapprove.com`)
  - traefik.http.routers.secureapprove.tls=true

  # Servicio
  - traefik.http.services.secureapprove.loadbalancer.server.port=80

  # Middlewares (definidos en dynamic.yml de este proxy)
  - traefik.http.routers.secureapprove.middlewares=default-sec-headers@file,gzip-compress@file,cors-headers@file
```

Si tu API corre en otro contenedor/puerto (por ejemplo `api.secureapprove.com`), creá un segundo router/servicio con su `rule` y `server.port`.
