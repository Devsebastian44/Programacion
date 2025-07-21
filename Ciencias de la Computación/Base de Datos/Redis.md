# Introducción a Redis

**Redis** (Remote Dictionary Server) es una base de datos en memoria, tipo **clave-valor**, de código abierto y con persistencia opcional. Es utilizada como base de datos, sistema de cache y broker de mensajes.

## Instalación de Redis

### En sistemas basados en Debian (Ubuntu)

```bash
sudo apt update
sudo apt install redis-server
```

Verificar instalación:

```bash
redis-cli ping
# Respuesta esperada: PONG
```

### En sistemas basados en Red Hat (Fedora, CentOS)

```bash
sudo dnf install redis
sudo systemctl enable --now redis
```

### En macOS (usando Homebrew)

```bash
brew install redis
brew services start redis
```

## Redis con Docker

### Usar imagen oficial

```bash
docker run --name redis -p 6379:6379 -d redis
```

### Redis con persistencia

```bash
docker run --name redis \
  -p 6379:6379 \
  -v redis-data:/data \
  -d redis redis-server --save 60 1 --loglevel warning
```

### Probar conexión

```bash
docker exec -it redis redis-cli
```

## Conceptos Básicos de Redis

|Concepto|Descripción|
|---|---|
|**Clave-Valor**|Redis almacena datos como pares clave/valor|
|**In-Memory**|Todos los datos residen en la memoria RAM (muy rápido)|
|**Persistencia**|Redis puede guardar los datos en disco (RDB o AOF)|
|**Tipos de datos**|Soporta múltiples estructuras de datos más allá de strings|
|**Monohilo**|Redis corre en un solo hilo (muy optimizado)|

## Tipos de Datos en Redis

|Tipo|Descripción|Ejemplo comando|
|---|---|---|
|`string`|Texto, número, binario|`SET clave valor` / `GET clave`|
|`list`|Lista enlazada de strings|`LPUSH`, `RPUSH`, `LRANGE`|
|`set`|Conjunto no ordenado de elementos únicos|`SADD`, `SMEMBERS`, `SREM`|
|`hash`|Diccionario de pares campo-valor|`HSET`, `HGET`, `HGETALL`|
|`zset`|Set ordenado con puntuación (score)|`ZADD`, `ZRANGE`, `ZREM`|
|`bitmap`|Secuencia de bits|`SETBIT`, `GETBIT`|
|`hyperloglog`|Conteo probabilístico de elementos|`PFADD`, `PFCOUNT`|

