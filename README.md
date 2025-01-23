# DOCKER COMPOSE POSTGRESQL + PGADMIN

Este proyecto proporciona una implementación sencilla para uso personal utilizando **Docker** para crear un entorno con **PostgreSQL** y **pgAdmin** listo para usar en menos de 3 minutos. Es ideal para practicar y tener un entorno aislado para gestionar bases de datos.

## Estructura de los Servicios

Este proyecto utiliza **Docker Compose** para orquestar dos servicios:

1. **PostgreSQL:** Base de datos relacional para almacenar y gestionar datos.
2. **pgAdmin:** Herramienta web de administración de bases de datos PostgreSQL.

## Requisitos

- Docker y Docker Compose instalados en tu máquina.

## Configuración

### 1. Clona el repositorio

Primero, clona este repositorio en tu máquina:

```bash
git clone https://github.com/zer0-sh/DBPostgresDocker
cd DBPostgresDocker
```

### 2. Edita la configuración según sea necesario

Puedes personalizar los valores de los servicios, como el nombre de la base de datos, el usuario y la contraseña en el archivo `docker-compose.yml`:

```bash
services:
  postgres:
    image: postgres:latest
    container_name: postgres
    environment:
      POSTGRES_USER: admin # Cambia esto al nombre de usuario deseado
      POSTGRES_PASSWORD: admin123 # Cambia esto a la contraseña deseada
      POSTGRES_DB: postgres # Cambia esto al nombre de la base de datos deseada
    ports:
      - "5432:5432" # Cambia esto al puerto deseado local:contenedor
    volumes:
      - postgres_data:/var/lib/postgresql/data

  pgadmin:
    image: dpage/pgadmin4:latest
    container_name: pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: admin
      PGADMIN_DEFAULT_PASSWORD: admin123
    ports:
      - "8080:80" # Cambia esto al puerto deseado local:contenedor
    depends_on:
      - postgres
    volumes:
      - pgadmin_data:/var/lib/pgadmin
      - ~/DatosBD/:/var/lib/pgadmin/storage/admin_admin.com # ~/DatosBD/ es el path al directorio de backups
      - ./servers.json:/pgadmin4/servers.json

volumes:
  postgres_data:
  pgadmin_data:

```

"""
This function connects to a database and retrieves data based on the provided query.

#### Note:
- En Linux, el directorio puede ser `~/DatosBD` para `/home/PepitoPerez/DatosBD`.
- En Windows, el directorio es `/mnt/c/Users/PepitoPerez/Documents/` para `C:\Users\PepitoPerez\Documents`.

### 3. Inicia los servicios

Una vez configurado el archivo, inicia los servicios con Docker Compose

```bash
docker-compose up -d
```
Esto descargará las imágenes necesarias, creará y levantará los contenedores de PostgreSQL y pgAdmin.

### 4. Accede a pgAdmin

Una vez que los contenedores estén corriendo, puedes acceder a pgAdmin a través de tu navegador en:

```bash
http://localhost:8080
```

Las credenciales predeterminadas son:

    Email: admin
    Contraseña: admin123

## Nota Importante sobre el Directorio de Backups en Linux

Al usar el directorio ~/DatosBD/ para almacenar los backups en Linux, existen dos caminos principales viables, aunque uno de ellos es menos seguro que el otro:

### 1. Establecer los permisos al directorio: Puedes cambiar la propiedad del directorio a root mediante el siguiente comando:

```bash
sudo chown -R root:root ~/DatosBD/
```

Sin embargo, para modificar este directorio, necesitarás realizar cambios como `root`. Aunque esta es una opción más segura, requiere privilegios elevados.

### 2. Dar permisos 777: Puedes darle permisos de escritura global con el siguiente comando:

```bash
sudo chmod -R 777 ~/DatosBD/
```

Este método es riesgoso, ya que permitirá que cualquier usuario del sistema tenga acceso completo al directorio, lo cual puede generar problemas de seguridad.

### 3. Eliminar la linea 26 y usar el directorio por defecto: Como alternativa, puedes usar el directorio por defecto de Docker para almacenar los backups:

#### En Linux

```bash
/var/lib/docker/volumes/dbpostgresdocker_pgadmin_data/_data
```

#### En Windows

```bash
\\wsl$\docker-desktop-data\version-pack-data\community\docker\volumes\dbpostgresdocker_pgadmin_data\_data
```

Este directorio también requiere que realices cambios como `root` o `administrador` dependiendo del SO utilizado.

### Nota final
Honestamente no lo he probado en Windows, en teoría debería funcinar, espero en el futuro hacerlo.

## Checklist

- [ ] Solucionar permisos para carpeta `~/DatosBD/` en Linux.
- [ ] Realizar pruebas en Windows.


