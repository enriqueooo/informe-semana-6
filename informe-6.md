# Práctica: Contenedores con WordPress, PostgreSQL y pgAdmin usando Docker Compose

## 1. Título  
**Creación de servicios PostgreSQL, pgAdmin y WordPress en contenedores usando Docker Compose**

## 2. Tiempo de duración  
**90 minutos**

## 3. Fundamentos  

En esta práctica se aprenderá a levantar múltiples servicios (PostgreSQL, pgAdmin y WordPress) utilizando Docker Compose. Aunque WordPress no está diseñado para trabajar con PostgreSQL por defecto, esta práctica tiene el propósito de construir una orquestación básica de contenedores para familiarizarse con redes, volúmenes y dependencias entre servicios.

Se demostrará cómo definir servicios en un archivo `docker-compose.yml`, crear una red personalizada y volúmenes para persistencia de datos.

## 4. Conocimientos previos

- Fundamentos de Docker y Docker Compose.
- Conocimiento básico sobre PostgreSQL y pgAdmin.
- Familiaridad con YAML.
- Comprensión general del funcionamiento de WordPress.

## 5. Objetivos a alcanzar

- Definir una red en Docker Compose.
- Crear un volumen persistente para la base de datos PostgreSQL.
- Levantar los servicios: PostgreSQL, pgAdmin y WordPress.
- Acceder a las interfaces web de pgAdmin y WordPress.

## 6. Equipo necesario

- Docker y Docker Compose instalados.
- Conexión a internet.
- Navegador web.

## 7. Material de apoyo

- Docker Compose Docs: https://docs.docker.com/compose/
- PostgreSQL Docs: https://www.postgresql.org/docs/
- pgAdmin Docs: https://www.pgadmin.org/docs/
- WordPress: https://wordpress.org/

---

## 8. Procedimiento

### Paso 1: Crear el archivo `docker-compose.yml`

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:13
    container_name: postgres-container
    restart: always
    environment:
      POSTGRES_USER: wpuser
      POSTGRES_PASSWORD: wppassword
      POSTGRES_DB: wpdatabase
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - wp-network

  pgadmin:
    image: dpage/pgadmin4
    container_name: pgadmin-container
    restart: always
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@example.com
      PGADMIN_DEFAULT_PASSWORD: admin
    ports:
      - "5050:80"
    depends_on:
      - postgres
    networks:
      - wp-network

  wordpress:
    image: wordpress:latest
    container_name: wordpress-container
    restart: always
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: postgres:5432
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppassword
      WORDPRESS_DB_NAME: wpdatabase
    networks:
      - wp-network

volumes:
  pgdata:

networks:
  wp-network:
```
![](1.jpg)
![](2.jpg)
![](3.jpg)

## Paso 2: Ejecutar los servicios

```bash
docker-compose up -d
```
## Paso 3: Acceder a los servicios

- **pgAdmin**: [http://localhost:5050](http://localhost:5050)  
  - **Usuario**: `admin@example.com`  
  - **Contraseña**: `admin`

- **WordPress**: [http://localhost:8080](http://localhost:8080)  
  > Se mostrará un error de conexión a la base de datos, como se espera.
![](4.jpg)
---

## 9. Resultados esperados

- Los contenedores deben ejecutarse correctamente en una red común.
- pgAdmin debe conectarse a PostgreSQL exitosamente.
- WordPress debe levantar pero no podrá conectarse a PostgreSQL (limitación conocida).
- Se demostrará la capacidad de Docker Compose para orquestar múltiples servicios.

---

## 10. Bibliografía

- [Docker Docs](https://docs.docker.com/)
- [PostgreSQL Docs](https://www.postgresql.org/docs/)
- [pgAdmin Docs](https://www.pgadmin.org/docs/)
- [WordPress Docs](https://wordpress.org/support/)

---

## 11. Enlace del audio

> (Agrega aquí el enlace al archivo o plataforma de audio correspondiente)



