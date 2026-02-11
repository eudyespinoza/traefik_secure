Colocá acá los certificados TLS:
- secureapprove.com.crt
- secureapprove.com.key

Si en tu server ya existen en otro path (ej: /etc/ssl/secureapprove), editá el volumen en docker-compose.yml para montar ese directorio en /certs/secureapprove.
