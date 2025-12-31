# MongoDB Docker Compose - Producción

Configuración de MongoDB para producción con Docker Compose.

## Configuración

1. Copia el archivo de entorno:

   ```bash
   cp .env.example .env
   ```

2. Edita `.env` y cambia las credenciales por defecto.

## Uso

```bash
# Iniciar
docker compose up -d

# Ver logs
docker compose logs -f

# Detener
docker compose down

# Estado
docker compose ps
```

## Conexión

**Desde el host (localhost):**

```
mongodb://admin:tu_contraseña@localhost:27017/admin?authSource=admin
```

**Desde otro contenedor Docker (misma red):**

```
mongodb://admin:tu_contraseña@mongodb-prod:27017/admin?authSource=admin
```

**Nota:** Si la contraseña tiene caracteres especiales, codifícala en la URL (ej: `%` → `%25`, `$` → `%24`, `&` → `%26`)

**MongoDB Shell:**

```bash
docker compose exec mongodb mongosh -u admin -p tu_contraseña --authenticationDatabase admin
```

## Solución de Problemas

**Puerto ocupado:** Cambia `MONGO_PORT` en `.env`

**Ver logs:** `docker compose logs mongodb`
