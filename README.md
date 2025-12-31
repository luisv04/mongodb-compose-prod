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
Usa el puerto mapeado del host (ej: `27018` si está configurado así):

```
mongodb://admin:tu_contraseña@localhost:27018/admin?authSource=admin
```

**Desde otro contenedor Docker (misma red):**
Usa el **puerto interno del contenedor** (`27017`) y el nombre del contenedor:

```
mongodb://admin:tu_contraseña@mongodb-prod:27017/admin?authSource=admin
```

**⚠️ Importante:**

- Desde contenedores en la misma red: usa el puerto **interno** (27017), NO el puerto mapeado del host
- Si la contraseña tiene caracteres especiales, codifícala (ej: `%` → `%25`, `$` → `%24`, `&` → `%26`)

**MongoDB Shell:**

```bash
docker compose exec mongodb mongosh -u admin -p tu_contraseña --authenticationDatabase admin
```

## Solución de Problemas

**Puerto ocupado:** Cambia `MONGO_PORT` en `.env`

**Ver logs:** `docker compose logs mongodb`
