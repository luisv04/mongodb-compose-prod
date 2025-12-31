# Guía de Conexión a MongoDB

## Problema Común: Conexión desde otros contenedores Docker

Si estás intentando conectar desde otro contenedor (como n8n) y no funciona, verifica lo siguiente:

## URL Correcta

### ❌ Incorrecto (desde contenedor)
```
mongodb://admin:Jasu%$26@0.0.0.0:27018/admin?authSource=admin
```

**Problemas:**
- `0.0.0.0` no funciona desde dentro de Docker, usa el nombre del contenedor
- `27018` es el puerto del host, dentro del contenedor usa `27017`
- Los caracteres especiales no están codificados correctamente

### ✅ Correcto (desde contenedor en la misma red)
```
mongodb://admin:Jasu%25%2426@mongodb-prod:27017/admin?authSource=admin
```

**Explicación:**
- `mongodb-prod` = nombre del contenedor (resolución DNS en Docker)
- `27017` = puerto interno del contenedor
- `Jasu%25%2426` = contraseña codificada (`%` → `%25`, `$` → `%24`)

## Codificación de Caracteres Especiales en URLs

| Carácter | Código URL |
|----------|------------|
| `%` | `%25` |
| `$` | `%24` |
| `&` | `%26` |
| `#` | `%23` |
| `@` | `%40` |
| `:` | `%3A` |
| `/` | `%2F` |
| `?` | `%3F` |
| `=` | `%3D` |
| `+` | `%2B` |
| ` ` (espacio) | `%20` |

## Ejemplos de Contraseñas

**Contraseña original:** `Jasu%$26`
**URL codificada:** `Jasu%25%2426`

**Contraseña original:** `Pass&Word#123`
**URL codificada:** `Pass%26Word%23123`

## Verificar Conexión

### Desde el contenedor n8n:
```bash
docker exec -it n8n mongosh "mongodb://admin:Jasu%25%2426@mongodb-prod:27017/admin?authSource=admin"
```

### Desde el host:
```bash
mongosh "mongodb://admin:Jasu%25%2426@localhost:27018/admin?authSource=admin"
```

## Si n8n no está en la misma red

Si n8n está en otra red Docker, tienes dos opciones:

### Opción 1: Conectar n8n a la red de MongoDB
```bash
docker network connect mongodb-compose-prod_mongodb-network n8n
```

### Opción 2: Hacer la red externa (editar docker-compose.yml)
```yaml
networks:
  mongodb-network:
    external: true
    name: mongodb-network
```

Luego crear la red manualmente:
```bash
docker network create mongodb-network
```

