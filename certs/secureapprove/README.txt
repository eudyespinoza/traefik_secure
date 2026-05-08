Coloca aca los certificados TLS usados por Traefik:

- secureapprove.com.crt
- secureapprove.com.key
- distromaxi.com.crt
- distromaxi.com.key
- distromaxi.com.ar.crt
- distromaxi.com.ar.key

El docker-compose monta este directorio como /certs/secureapprove.
Si en tu server existen en otro path, monta ese directorio en
/certs/secureapprove.
