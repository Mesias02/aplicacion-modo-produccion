# TAS9 - Aplicación en modo producción
## 1. Título
Aplicación en modo producción

## 2. Tiempo de duración
El tiempo estimado fue de 210 minutos.

## 3. Fundamentos
Dockerfile (Node.js + Nginx)
Para contenerizar aplicaciones modernas, es recomendable dividir la creación de la imagen en etapas. La primera etapa usa Node.js para construir los archivos del frontend, y la segunda etapa emplea Nginx para servir estos archivos en producción.
----
![image](https://github.com/user-attachments/assets/aef823ea-52c0-4b1a-a284-0a98de6ec8f3)
----
El archivo Dockerfile para el proceso de build usa una imagen de Node.js. Este archivo contiene instrucciones para instalar dependencias y generar el build de producción. Posteriormente, en otro Dockerfile, Nginx se encarga de servir los archivos estáticos construidos. Esta práctica permite imágenes más livianas y seguras (Docker Docs, n.d.).

Además, se aplicó la técnica de multi-stage build, que permite ejecutar cada etapa en entornos separados, optimizando el tamaño final de la imagen al copiar sólo los archivos necesarios a la imagen de producción (IONOS, n.d.).

Docker Compose
El archivo docker-compose.yml permite orquestar múltiples contenedores. En esta práctica se utilizaron contenedores para:

Frontend (servido con Nginx)

Backend (Spring Boot)

Base de datos (PostgreSQL)

Cliente de base de datos (pgAdmin)

Este enfoque facilita el despliegue de aplicaciones complejas con múltiples servicios dependientes.

## 4. Conocimientos previos
El estudiante debe contar con conocimientos sobre:

Fundamentos de Spring Boot y React.

Comandos básicos de Docker.

Creación y lectura de archivos Dockerfile.

Estructura de archivos .env.

Bases de datos relacionales con PostgreSQL.

Configuración básica de Nginx.

## 5. Objetivos a alcanzar
Crear un Dockerfile para la etapa de construcción del frontend usando Node.js.

Crear un segundo Dockerfile para servir el frontend con Nginx.

Contenerizar el backend con un Dockerfile.

Desplegar todos los servicios (frontend, backend, base de datos y pgAdmin) con docker-compose.

Validar el correcto funcionamiento entre todos los contenedores.

## 6. Equipo necesario
Computador con Docker y Docker Compose instalados.

Acceso a internet.

Navegador web.

Repositorio del proyecto (frontend y backend).

## 7. Material de apoyo
Documentación oficial de Docker y Nginx.

Documentación oficial de Node.js y React.

Guía de la asignatura.

Videos tutoriales recomendados.

Repositorio del proyecto.

## 8. Procedimiento
Pasos
Clonar el proyecto frontend y backend desde GitHub.

git clone https://github.com/usuario/proyecto

Crear archivo .env para configuración de variables del backend.

Crear Dockerfile para construir el frontend:

Dockerfile
Copiar
Editar
# Etapa 1 - Build
FROM node:18 as builder
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build
Crear Dockerfile para servir el frontend con Nginx:

Dockerfile
Copiar
Editar
# Etapa 2 - Producción
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
Crear Dockerfile para backend con multi-stage:

Dockerfile
Copiar
Editar
FROM eclipse-temurin:17-jdk AS build
WORKDIR /app
COPY . .
RUN ./mvnw package -DskipTests

FROM eclipse-temurin:17-jdk
WORKDIR /app
COPY --from=build /app/target/app.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
Crear archivo docker-compose.yml con servicios:

yaml
Copiar
Editar
version: '3.8'
services:
  frontend:
    build:
      context: ./frontend
    ports:
      - "80:80"
    depends_on:
      - backend

  backend:
    build:
      context: ./backend
    ports:
      - "8080:8080"
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/mydb
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
    volumes:
      - postgres_data:/var/lib/postgresql/data

  pgadmin:
    image: dpage/pgadmin4
    ports:
      - "5050:80"
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@admin.com
      PGADMIN_DEFAULT_PASSWORD: admin
    depends_on:
      - db

volumes:
  postgres_data:
Ejecutar los servicios:

bash
Copiar
Editar
docker-compose up --build -d
Verificar conexión del backend a PostgreSQL y acceso desde pgAdmin.

Validar el despliegue en producción accediendo a http://localhost.

## 9. Resultados esperados
Se logró contenerizar el frontend, backend y la base de datos exitosamente. La aplicación fue desplegada en producción con Nginx, mostrando correctamente la interfaz de usuario. El backend se conectó con PostgreSQL y ejecutó migraciones con Flyway. Desde pgAdmin se confirmó el acceso a la base de datos.

Todos los servicios fueron levantados de forma automática con Docker Compose, y la red interna permitió la comunicación entre contenedores.

## 10. Bibliografía
Docker Docs. (n.d.). Multi-stage builds. Retrieved June 15, 2025, from https://docs.docker.com/get-started/docker-concepts/building-images/multi-stage-builds/

IONOS. (n.d.). ¿Qué es el Dockerfile?. Retrieved June 15, 2025, from https://www.ionos.com/es-us/digitalguide/servidores/know-how/dockerfile/

DigitalOcean. (n.d.). How to use Docker Compose for multi-container applications. Retrieved June 15, 2025, from https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose
