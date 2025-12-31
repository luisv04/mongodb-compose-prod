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

**String de conexión:**

```
mongodb://admin:tu_contraseña@localhost:27017/admin?authSource=admin
```

**MongoDB Shell:**

```bash
docker compose exec mongodb mongosh -u admin -p tu_contraseña --authenticationDatabase admin
```

## Solución de Problemas

**Puerto ocupado:** Cambia `MONGO_PORT` en `.env`

**Ver logs:** `docker compose logs mongodb`
