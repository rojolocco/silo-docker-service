# Silo Docker Service

Servicio Docker para desplegar **Silo**, un almacenamiento de objetos compatible con S3, con configuración reproducible mediante Docker Compose.

## Descripción

Este repositorio contiene la configuración para levantar Silo en local o en un entorno de desarrollo. Silo expone una API compatible con S3 (puerto 9000) y una consola web de administración (puerto 9001).

## Estructura del repositorio

| Ruta | Descripción |
| ---- | ----------- |
| `docker-compose.yaml` | Stack principal: servicio `silo` con volumen persistente, healthcheck y red interna. |
| `docker-compose.dev.yaml` | Variante de desarrollo que usa `.env.dev` en lugar de `.env`. |
| `.env.example` | Plantilla con las variables requeridas. |
| `test_python/` | Proyecto Python de prueba de humo contra el endpoint S3 (ver [test_python/README.md](test_python/README.md)). |
| `LICENSE` | Licencia del proyecto. |

## Servicio Silo

- **Contenedor**: `silo`
- **Imagen**: `docker.io/pgsty/silo` (versión fijada por fecha de release en el compose)
- **Puertos**:
  - `9000`: API S3
  - `9001`: consola web
- **Comando**: `server /data --console-address ":9001"`
- **Datos**: persistidos en el volumen `silo-data`, montado en `/data`
- **Variables de entorno**: cargadas desde `.env`
- **Reinicio**: `unless-stopped`
- **Healthcheck**: `silo healthcheck ready`, cada 30 s
- **Red**: `internal_network` (bridge)

## Requisitos

- Docker y Docker Compose instalados.

## Configuración

1. Copia la plantilla de variables y completa los valores:

   ```bash
   cp .env.example .env
   ```

2. Define en `.env`:

   ```env
   MINIO_ROOT_USER=<usuario-administrador>
   MINIO_ROOT_PASSWORD=<contraseña-administrador>
   ```

   > `.env` y `.env.*` están excluidos de Git. No subas credenciales al repositorio.

## Uso

Desde la raíz del proyecto:

```bash
docker compose up -d
```

Para el entorno de desarrollo (requiere `.env.dev`):

```bash
docker compose -f docker-compose.dev.yaml up -d
```

Para detener el servicio:

```bash
docker compose down
```

Accesos:

- API S3: [http://localhost:9000](http://localhost:9000)
- Consola web: [http://localhost:9001](http://localhost:9001)

## Prueba rápida

El proyecto `test_python/` verifica que el endpoint S3 responda correctamente: crea el bucket `mybucket`, sube un archivo, lo lista y comprueba que la descarga coincida con el original.

```bash
cd test_python
uv sync
uv run test-python
```

## Licencia

Este proyecto se distribuye bajo los términos del archivo [LICENSE](LICENSE).
