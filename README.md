# SonarQube local

Configuración para ejecutar SonarQube y PostgreSQL localmente con Docker Compose.

## Prerrequisitos

- Docker Desktop (macOS o Windows) o Docker Engine (Linux), en ejecución.
- Docker Compose v2, disponible como el comando `docker compose`.
- Puerto `9000` disponible en la máquina.
- Conexión a Internet la primera vez, para descargar las imágenes de Docker.

## Configuración

El archivo `.env` incluido contiene la configuración para ejecutar este
proyecto localmente. No es necesario modificarlo para seguir las instrucciones
de esta guía.

> Si planeas crear tu propio servidor de SonarQube en la nube o en un VPS,
> configura allí las variables `POSTGRES_USER`, `POSTGRES_PASSWORD`,
> `POSTGRES_DB` y `SONARQUBE_VERSION` según tu entorno. Usa una contraseña
> segura y no publiques ni compartas el archivo `.env` con credenciales.

En el despliegue local, `SONARQUBE_VERSION` determina la etiqueta de la imagen
`sonarqube`; por ejemplo, `community` selecciona la edición Community disponible
con esa etiqueta.

## Iniciar

Desde la raíz del proyecto:

```bash
docker compose up -d
```

PostgreSQL inicia primero. SonarQube espera a que la comprobación de salud de la
base de datos sea satisfactoria.

Consulta el estado de los servicios:

```bash
docker compose ps
```

Para ver los registros de SonarQube durante el arranque:

```bash
docker compose logs -f sonarqube
```

Cuando termine el arranque, abre [http://localhost:9000](http://localhost:9000)
en el navegador. En una instalación nueva, las credenciales iniciales de
SonarQube suelen ser `admin` / `admin`; cambia la contraseña cuando la
aplicación lo solicite.

## Detener y conservar los datos

Para detener los servicios sin borrar la base de datos ni los datos de
SonarQube:

```bash
docker compose down
```

Los datos se guardan en volúmenes de Docker y se conservan al detener o recrear
los contenedores.

## Borrar los datos

Si necesitas eliminar también la base de datos, los proyectos y la
configuración persistida, ejecuta:

```bash
docker compose down --volumes
```

**Este comando borra permanentemente los datos almacenados en los volúmenes.**

## Servicios y persistencia

- `sonarqube`: interfaz web en el puerto `9000`.
- `postgres`: base de datos PostgreSQL 17, accesible dentro de la red de Docker.
- Los volúmenes `sonarqube_postgres_data`, `sonarqube_data`,
  `sonarqube_extensions` y `sonarqube_logs` persisten la base de datos, los
  datos de SonarQube, las extensiones y los registros, respectivamente.
