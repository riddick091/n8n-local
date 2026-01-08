# Iniciar n8n Local con Docker

Esta guía describe cómo ejecutar n8n localmente utilizando Docker.

## Prerrequisitos

- Docker instalado en tu sistema
- Docker Compose (opcional, para configuraciones avanzadas)

## Método 1: Ejecutar con Docker (Simple)

### Instalación básica

Ejecuta n8n con persistencia de datos:

docker volume create n8n_data

docker run -it --rm `
 --name n8n `
 -p 5678:5678 `
 -e GENERIC_TIMEZONE="America/Argentina/Buenos_Aires" `
 -e TZ="America/Argentina/Buenos_Aires" `
 -e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true `
 -e N8N_RUNNERS_ENABLED=true `
 -v n8n_data:/home/node/.n8n `
 docker.n8n.io/n8nio/n8n

### Acceder a n8n

Una vez iniciado, accede a n8n en tu navegador:

```
http://localhost:5678
```

## Método 2: Docker Compose (Recomendado)

### Crear archivo docker-compose.yml

Crea un archivo `docker-compose.yml` con el siguiente contenido:

```yaml
version: '3.8'

services:
  n8n:
    image: n8nio/n8n
    container_name: n8n
    restart: unless-stopped
    ports:
      - "5678:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=admin
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
      - NODE_ENV=production
      - WEBHOOK_URL=http://localhost:5678/
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  n8n_data:
```

### Iniciar n8n con Docker Compose

```bash
docker-compose up -d
```

### Ver logs

```bash
docker-compose logs -f n8n
```

### Detener n8n

```bash
docker-compose down
```

## Método 3: Con Base de Datos PostgreSQL

Para producción, es recomendable usar PostgreSQL:

### Archivo docker-compose.yml con PostgreSQL

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:14
    container_name: n8n-postgres
    restart: unless-stopped
    environment:
      - POSTGRES_USER=n8n
      - POSTGRES_PASSWORD=n8n
      - POSTGRES_DB=n8n
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -h localhost -U n8n -d n8n']
      interval: 5s
      timeout: 5s
      retries: 10

  n8n:
    image: n8nio/n8n
    container_name: n8n
    restart: unless-stopped
    ports:
      - "5678:5678"
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=n8n
      - DB_POSTGRESDB_PASSWORD=n8n
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=admin
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
      - NODE_ENV=production
      - WEBHOOK_URL=http://localhost:5678/
    volumes:
      - n8n_data:/home/node/.n8n
    depends_on:
      postgres:
        condition: service_healthy

volumes:
  postgres_data:
  n8n_data:
```

### Iniciar con PostgreSQL

```bash
docker-compose up -d
```

## Variables de Entorno Importantes

| Variable | Descripción | Valor por defecto |
|----------|-------------|-------------------|
| `N8N_BASIC_AUTH_ACTIVE` | Activar autenticación básica | `false` |
| `N8N_BASIC_AUTH_USER` | Usuario para autenticación | - |
| `N8N_BASIC_AUTH_PASSWORD` | Contraseña para autenticación | - |
| `N8N_HOST` | Host donde corre n8n | `localhost` |
| `N8N_PORT` | Puerto de n8n | `5678` |
| `N8N_PROTOCOL` | Protocolo (http/https) | `http` |
| `WEBHOOK_URL` | URL base para webhooks | - |
| `DB_TYPE` | Tipo de base de datos | `sqlite` |
| `GENERIC_TIMEZONE` | Zona horaria | `America/New_York` |
| `TZ` | Zona horaria del contenedor | `America/New_York` |

## Comandos Útiles

### Ver contenedores en ejecución

```bash
docker ps
```

### Acceder a los logs de n8n

```bash
docker logs -f n8n
```

### Detener n8n

```bash
docker stop n8n
```

### Reiniciar n8n

```bash
docker restart n8n
```

### Eliminar contenedor

```bash
docker rm -f n8n
```

### Ver volúmenes

```bash
docker volume ls
```

### Hacer backup de los datos

```bash
docker run --rm -v n8n_data:/data -v $(pwd):/backup ubuntu tar czf /backup/n8n-backup.tar.gz -C /data .
```

### Restaurar backup

```bash
docker run --rm -v n8n_data:/data -v $(pwd):/backup ubuntu tar xzf /backup/n8n-backup.tar.gz -C /data
```

## Actualizar n8n

### Método 1: Docker run

```bash
docker stop n8n
docker rm n8n
docker pull n8nio/n8n
# Volver a ejecutar el comando docker run
```

### Método 2: Docker Compose

```bash
docker-compose down
docker-compose pull
docker-compose up -d
```

## Solución de Problemas

### El puerto 5678 ya está en uso

Cambia el puerto en el comando o docker-compose.yml:

```yaml
ports:
  - "5679:5678"  # Usa el puerto 5679 en tu host
```

### Problemas de permisos con volúmenes

Si tienes problemas de permisos, puedes ejecutar:

```bash
sudo chown -R 1000:1000 ~/.n8n
```

### Ver información detallada del contenedor

```bash
docker inspect n8n
```

## Recursos Adicionales

- [Documentación oficial de n8n](https://docs.n8n.io/)
- [n8n en Docker Hub](https://hub.docker.com/r/n8nio/n8n)
- [Repositorio de GitHub](https://github.com/n8n-io/n8n)

## Notas de Seguridad

- **Siempre** cambia las credenciales por defecto en producción
- Considera usar HTTPS con un proxy reverso (nginx, Traefik)
- Limita el acceso a la red si es posible
- Mantén n8n actualizado regularmente
- Realiza backups periódicos de tus datos
