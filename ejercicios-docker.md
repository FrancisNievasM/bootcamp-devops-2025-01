# Ejercicios de Docker

## Ejercicio 1: Introducción a Docker
### Objetivo: Familiarizarse con los conceptos básicos de Docker.

#### 1. Verificar si Docker está instalado correctamente
```bash
docker --version
```
#### 2. Probar si los contenedores pueden levantarse correctamente
```bash
docker run hello-world
```
#### 3. Habilitar el servicio de Docker al arrancar el sistema
```bash
sudo systemctl enable docker
```
#### 4. Información proporcionada por el comando `docker info`
**Tres aspectos clave:**
- **Versiones:** Información sobre las versiones de Docker y su motor.
- **Recursos disponibles:** Muestra la cantidad de CPU, memoria y almacenamiento disponible.
- **Contenedores y redes:** Proporciona detalles sobre el número de contenedores, imágenes y redes en el sistema.

---

## Ejercicio 2: Manejo de Imágenes y Contenedores
### Objetivo: Explorar el uso básico de imágenes y contenedores.

#### 1. Descargar una imagen oficial de Nginx desde Docker Hub
```bash
docker pull nginx
```
#### 2. Descargar una imagen de Nginx basada en Alpine
```bash
docker pull nginx:alpine
```
#### 3. ¿Qué significa que una imagen sea Alpine?
- Es una imagen minimalista basada en Alpine Linux.
- **Ventajas:**
  - Menor tamaño.
  - Más rápida de descargar y ejecutar.
  - Reduce la superficie de ataque.

#### 4. Listar todas las imágenes disponibles en tu sistema
```bash
docker images
```
#### 5. Script para listar imágenes ordenadas por peso (de mayor a menor)
```bash
#!/bin/bash
docker images --format "{{.Repository}}:{{.Tag}} {{.Size}}" | sort -rh -k2
```
#### 6. Iniciar un contenedor basado en una imagen descargada
```bash
docker run -d -p 80:80 nginx
```
- **Explicación:**
  - `-d`: Ejecuta el contenedor en segundo plano.
  - `-p 80:80`: Mapea el puerto 80 del contenedor al puerto 80 del host.

#### 7. Detener y eliminar un contenedor
```bash
docker stop <container_id> && docker rm <container_id>
```
#### 8. Diferencia entre `docker stop` y `docker rm`
- **`docker stop`:** Detiene un contenedor en ejecución.
- **`docker rm`:** Elimina un contenedor detenido.

#### 9. Eliminar imágenes que no se están utilizando
```bash
docker image prune -a
```
---

## Ejercicio 3: Creación de un Dockerfile
### Objetivo: Construir una imagen personalizada utilizando un Dockerfile.

#### 1. Pasos principales para crear un Dockerfile
- Definir la imagen base.
- Copiar los archivos necesarios al contenedor.
- Ejecutar comandos para instalar dependencias.
- Configurar el comando predeterminado para ejecutar.

#### 2. Construir una imagen personalizada
```bash
docker build -t mi-imagen-personalizada .
```
#### 3. Verificar que la imagen se creó correctamente
```bash
docker images
```
#### 4. Iniciar un contenedor basado en la nueva imagen
```bash
docker run -d mi-imagen-personalizada
```
#### 5. Diferencia entre `COPY` y `ADD`
- **`COPY`:** Copia archivos o directorios locales al contenedor.
- **`ADD`:** Además de copiar, permite descargar archivos desde una URL o descomprimir archivos tar.

#### 6. Diferencia entre `CMD`, `ENTRYPOINT` y `RUN`
- **`RUN`:** Ejecuta comandos en la construcción de la imagen.
- **`CMD`:** Comando predeterminado al iniciar el contenedor (puede ser sobrescrito).
- **`ENTRYPOINT`:** Comando fijo para iniciar el contenedor.

#### 7. Excluir archivos al copiar al contenedor
- Usar un archivo `.dockerignore`:
```plaintext
node_modules
*.log
secret.env
```
---

## Ejercicio 4: Redes en Docker
### Objetivo: Configurar y gestionar redes Docker.

#### 1. Crear una red personalizada
```bash
docker network create mi-red
```
#### 2. Iniciar un contenedor conectado a una red personalizada
```bash
docker run --network mi-red nginx
```
#### 3. Listar las redes disponibles
```bash
docker network ls
```
#### 4. Comprobar comunicación entre contenedores en la misma red
- Usar `ping` entre contenedores:
```bash
docker exec -it <container_id> ping <otro_container_id>
```
#### 5. Mecanismo para simplificar conexión entre contenedores
- Usar nombres de contenedores como hosts, en lugar de direcciones IP.

---

## Ejercicio 5: Volúmenes y Persistencia de Datos
### Objetivo: Aprender a gestionar volúmenes para la persistencia de datos.

#### 1. Crear un volumen en Docker
```bash
docker volume create mi-volumen
```
#### 2. Usar un volumen para persistir datos en MySQL
```bash
docker run -d -v mi-volumen:/var/lib/mysql mysql
```
#### 3. Verificar los detalles de un volumen
```bash
docker volume inspect mi-volumen
```
#### 4. Eliminar volúmenes no utilizados
```bash
docker volume prune
```
#### 5. Reutilizar un volumen existente
```bash
docker run -d -v mi-volumen:/app nginx
```
---

## Ejercicio 6: Docker Compose
### Objetivo: Orquestar servicios utilizando Docker Compose.

#### 1. Estructura básica de un archivo `docker-compose.yml`
```yaml
version: '3.8'
services:
  web:
    image: nginx
    ports:
      - "80:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
```
#### 2. Iniciar servicios definidos en un archivo Compose
```bash
docker-compose up -d
```
#### 3. Verificar el estado de los servicios
```bash
docker-compose ps
```
#### 4. Detener y eliminar servicios definidos
```bash
docker-compose down
```
#### 5. Diferencia entre `docker-compose down` y `docker-compose stop`
- **`docker-compose stop`:** Detiene los contenedores, pero los mantiene disponibles.
- **`docker-compose down`:** Detiene y elimina los contenedores, redes y volúmenes creados.

- ---

## Ejercicio 7: Inspección y Depuración de Contenedores
### Objetivo: Utilizar herramientas para inspeccionar y depurar contenedores.

#### 1. Inspeccionar los detalles de configuración de un contenedor
```bash
docker inspect <container_id>
```
#### 2. Obtener los logs de un contenedor en ejecución
```bash
docker logs <container_id>
```
#### 3. Abrir una terminal en modo interactivo a un contenedor activo
```bash
docker exec -it <container_id> /bin/bash
```
#### 4. Verificar los recursos (CPU/memoria) utilizados por un contenedor
```bash
docker stats <container_id>
```
---

## Ejercicio 8: Seguridad en Docker
### Objetivo: Implementar configuraciones básicas de seguridad en Docker.

#### 1. Crear un contenedor que utilice un usuario no root
```bash
docker run -u 1000:1000 nginx
```
- **Explicación:** `-u 1000:1000` indica el UID y GID del usuario no root.

#### 2. Limitar el uso de CPU y memoria de un contenedor
```bash
docker run --memory="512m" --cpus="1.0" nginx
```
- **`--memory="512m"`:** Limita la memoria a 512 MB.
- **`--cpus="1.0"`:** Asigna un máximo de un núcleo de CPU.

#### 3. Prácticas recomendadas para mejorar la seguridad en Docker
- **Usar imágenes oficiales y verificadas:** Reduce el riesgo de vulnerabilidades.
- **Mantener las imágenes y contenedores actualizados:** Aplica parches de seguridad regularmente.
- **Evitar ejecutar contenedores con privilegios elevados:** Minimiza riesgos de acceso no autorizado.

