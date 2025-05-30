# Creación del contenedor de Springboot

## 1. Preparación
Este comando le permitirá generar el JAR para ejecutar la aplicación
```sh
mvn clean package -DskipTests
``` 
Se genera el .jar, luego se debe hacer el dockerfile


## 2. Generación de imagen

```Dockerfile
# Utiliza una imagen base de Java
FROM amazoncorretto:22-jdk

# Establece el directorio de trabajo en la imagen
WORKDIR /app

# Copia el archivo JAR del backend al directorio de trabajo
COPY ./target/artifact.jar /app/backend.jar

# Expone el puerto en el que se ejecutará el backend (ajusta el número de puerto según tus necesidades)
EXPOSE 8080

# Comando para ejecutar la aplicación Spring Boot
CMD ["java", "-jar", "backend.jar"]
```
Luego se hace el build de la imagen de docker
```sh
docker build -t back:0.0.1 .
```
Todas las versiones vienen con un nombre de imagen y un número de versión. Se puede usar un formato de tres puntos de versión.</br></br>

## 3. Ejecutar imagen
Para ejecutar la imagen en un contenedor use
```sh
docker run -p 8080:8080 back:0.0.1
```
Esto generará un contenedor con nombre aleatorio que se ejecutará en el puerto 8080 y que está mapeado al puerto 8080 del contenedor.

# Entendiendo las redes de contenedores

Probablemente necesite esta linea. Entienda primero las redes de contenedores. Luego use
```
spring.datasource.url=jdbc:postgresql://host.docker.internal:5432/db
```


# Frontend

## 1. Preparación

Hacemos el bundle de la aplicación, por medio del comando

```
npm run build
```


Cree el archivo `nginx.conf`. Este archivo se deberá copiar al contenedor de NGINX

```conf
server {
    listen 3000;
    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

## 2. Generación de imagen

Especifiquemos la imagen por medio de un nuevo archivo llamado `Dockerfile`

```Dockerfile
# Utiliza una imagen base con Nginx instalado
FROM --platform=linux/amd64 nginx:latest

# Copia configuración de NGINX
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copia los archivos estáticos construidos
COPY dist/ /usr/share/nginx/html/

# Expone el puerto 3000
EXPOSE 3000
```

Cree la imagen de contenedor así

```sh
docker build -t front:0.0.1 .
```

## 3. Ejecutar imagen
Para ejecutar la imagen en un contenedor use
```sh
docker run -p 3000:3000 front:0.0.1
```
Esto generará un contenedor con nombre aleatorio que se ejecutará en el puerto 3000
