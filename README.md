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
----
![image](https://github.com/user-attachments/assets/7bce483d-2476-4055-84df-122b10c3ef67)
----
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
Crear un Dockerfile para la etapa de construcción del frontend utilizando una imagen base de Node.js, que permita compilar y generar los archivos estáticos de producción de la aplicación React.

Crear un segundo Dockerfile para servir el frontend con Nginx, copiando los archivos generados desde la etapa de construcción y configurando Nginx para exponer la aplicación en un entorno de producción.

Contenerizar la aplicación backend desarrollada en Spring Boot, mediante un Dockerfile que aplique la técnica de multi-stage build, asegurando una imagen liviana y optimizada para su ejecución en contenedores.

Desplegar todos los servicios necesarios para la aplicación (frontend, backend, base de datos PostgreSQL y cliente pgAdmin) integrándolos mediante un archivo docker-compose.yml, que facilite el levantamiento, configuración y conexión entre los contenedores.

Validar el correcto funcionamiento de todos los servicios, comprobando que:

El backend se comunica con la base de datos PostgreSQL.

El frontend se muestra correctamente al acceder desde el navegador.

pgAdmin permite conectarse al servicio de base de datos.

Todos los servicios se ejecutan correctamente en segundo plano usando Docker Compose.



## 6. Equipo necesario
Computador con Docker y Docker Compose instalados.

Acceso a internet.

Navegador web.

## 7. Material de apoyo
Documentación oficial de Docker y Nginx.

Documentación oficial de Node.js y React.

Guía de la asignatura.

Videos tutoriales recomendados.

Repositorio del proyecto.

## 8. Procedimiento
Crear un Dockerfile para la etapa de construcción del frontend usando Node.js.

---
![image](https://github.com/user-attachments/assets/61b3747d-d50e-468e-b70b-120495f3cbb0)

Crear un segundo Dockerfile para servir el frontend con Nginx.

---
![image](https://github.com/user-attachments/assets/5ae9c2ec-8503-4dba-b578-0b0699969c18)

Contenerizar el backend con un Dockerfile.

---
![image](https://github.com/user-attachments/assets/85117826-0531-42d9-89d1-b55e5dccf869)

Desplegar todos los servicios (frontend, backend, base de datos y pgAdmin) con docker-compose.

---
![image](https://github.com/user-attachments/assets/79969eb3-0976-4c51-8260-c3fd3cc9bc9c)
---
![image](https://github.com/user-attachments/assets/cb8e17f4-ea45-4b07-b67c-0d4d5b250a75)
---
![image](https://github.com/user-attachments/assets/e82668bd-2463-40a5-a6f7-1b8fe1e0db31)
---
Validar localhost 

---
![image](https://github.com/user-attachments/assets/2d36a6ce-9d19-4bfe-9173-489d72ca4c3b)

Ingreso a Deploy

----
![image](https://github.com/user-attachments/assets/2a33dbb5-b9b3-4b2d-a726-5df644dcdfe5)

Validar contendor Nginx

---
![image](https://github.com/user-attachments/assets/894bf745-3e83-4ed4-8989-e55a777ba05b)

Validar interior del contenedor nginx

---
![image](https://github.com/user-attachments/assets/b6d04a6a-d615-4802-8b06-6848bc018234)


## 9. Resultados esperados
Se logró contenerizar el frontend, backend y la base de datos exitosamente. La aplicación fue desplegada en producción con Nginx, mostrando correctamente la interfaz de usuario. El backend se conectó con PostgreSQL y ejecutó migraciones con Flyway. Desde pgAdmin se confirmó el acceso a la base de datos.

Todos los servicios fueron levantados de forma automática con Docker Compose, y la red interna permitió la comunicación entre contenedores.

## 10. Bibliografía
Docker Docs. (n.d.). Multi-stage builds. Retrieved June 15, 2025, from https://docs.docker.com/get-started/docker-concepts/building-images/multi-stage-builds/

IONOS. (n.d.). ¿Qué es el Dockerfile?. Retrieved June 15, 2025, from https://www.ionos.com/es-us/digitalguide/servidores/know-how/dockerfile/

DigitalOcean. (n.d.). How to use Docker Compose for multi-container applications. Retrieved June 15, 2025, from https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose
