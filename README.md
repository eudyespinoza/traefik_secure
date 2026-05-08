# Traefik (SecureApprove + DistroMaxi)

Este directorio contiene un proxy Traefik minimo para publicar SecureApprove y
DistroMaxi desde proyectos Docker separados.

## Requisitos

- Docker + Docker Compose.
- Las apps publicadas deben estar en la misma red Docker externa que Traefik:
  `secureapprove_proxy`.
- Certificados TLS disponibles en `./certs/secureapprove`:
  - `secureapprove.com.crt`
  - `secureapprove.com.key`
  - `distromaxi.com.crt`
  - `distromaxi.com.key`
  - `distromaxi.com.ar.crt`
  - `distromaxi.com.ar.key`

Si en el server los certificados viven en otro directorio, monta ese directorio
en `/certs/secureapprove` dentro del contenedor Traefik.

## Uso

1. Crear la red externa una sola vez:

```bash
docker network create secureapprove_proxy
```

2. Levantar Traefik:

```bash
docker compose up -d
```

3. Levantar cada app con labels Traefik y conectada a `secureapprove_proxy`.

## Labels esperados

Los routers reales se crean desde los labels de cada proyecto.

Ejemplo para un frontend:

```yaml
labels:
  - traefik.enable=true
  - traefik.docker.network=secureapprove_proxy
  - traefik.http.routers.mi-app.entrypoints=websecure
  - traefik.http.routers.mi-app.rule=Host(`example.com`) || Host(`www.example.com`)
  - traefik.http.routers.mi-app.tls=true
  - traefik.http.routers.mi-app.middlewares=default-sec-headers@file,gzip-compress@file
  - traefik.http.services.mi-app.loadbalancer.server.port=80
```

Para APIs publicadas bajo el mismo dominio, crea un router adicional con
`PathPrefix("/api")`, mayor prioridad que el frontend y el puerto interno del
backend.
