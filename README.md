# docker-intro
El objetivo de este repo es generar un ejemplo de uso básico de docker.
Cada uno de los puntos son simples extractos de código para usarse en modo "copy-paste" y poder ejecutar el tutorial en menos de 15 minutos.
## Tareas: 
* [Instalar Docker en Windows 10](#Instalar-Docker-Engine-en-Windows-10)
* Instalar Docker en Ubuntu 18.04
* Usar Imagen de ubuntu server con docker pre-instalado. 
1. [Crear un ejemplo de un app.js](#Crea-una-app-en-node-js)
2. [Crear un DockerFile para contenerizar esa app.](#Crear-un-DockerFile-para-contenerizar-esa-app)
3. [arrancarla sin balanceador. ](#arrancarla-sin-balanceador)
4. [Comandos básicos docker.](#Comandos-básicos-docker)
5. [Definir Dockerfile para un balanceador.](#Definir-Dockerfile-para-un-balanceador)
6. [arrancar la app con balanceador](#arrancar-la-app-con-balanceador)
### Instalar Docker Engine en Windows 10
1. [Descarga Docker](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe)
2. Instala Docker
3. Arranca docker buscándolo desde el menú de Inicio.

<p><br><img src="https://docs.docker.com/docker-for-windows/images/docker-app-search.png" width="199px" height="242px" float="left">
<img src="https://docs.docker.com/docker-for-windows/images/docker-app-welcome.png" width="199px" height="242px" float="right"></p>


**Uso de proxy en docker y windows 10** (opcional)



En caso de que estes detrás de un proxy, asegurate de configuarlo en


* Ve a la barra de tareas y selecciona con clic derecho el ícono de docker.


* Navega a settings: 

<p><img src="https://docs.docker.com/docker-for-windows/images/whale-icon-systray-hidden.png" width="199px" height="242px" float="left">
<img src="https://docs.docker.com/docker-for-windows/images/docker-menu-settings.png" width="199px" height="242px" float="right"></p>


* opción proxies: 
El formato de proxy corporativo es : http://username:password@proxy:port/

<p><img src="https://docs.docker.com/docker-for-windows/images/settings-proxies.png" width="50%" height="50%" float="left"></p>

4. abre un powershell y asegurate que docker esté funcionando, ejecutando:

```PowerShell
docker run hello-world
```
La salida esperada es similar a esta: 
```PowerShell
docker : Unable to find image 'hello-world:latest' locally
...

latest:
Pulling from library/hello-world
ca4f61b1923c:
Pulling fs layer
ca4f61b1923c:
Download complete
ca4f61b1923c:
Pull complete
Digest: sha256:97ce6fa4b6cdc0790cda65fe7290b74cfebd9fa0c9b8c38e979330d547d22ce1
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
```

### Crea una app en node js
Prepara un directorio de trabajo y colocate dentro de él.
EJEMPLO: DEMO/app

1) copia el siguiente código en un editor de texto. 
```JavaScript
//jrblanno
//201904
//JS "hello World" V1

//variables
var http = require('http');  
var os = require("os");
var hostname = os.hostname();
//variables
  
http.createServer(function (req, res) { 
	res.writeHead(200, { 'Content-Type': 'text/html' });  //formateo
	res.write (`<h1>${process.env.MESSAGE}</h1>`); //toma la entrada de la consola variable "MESSAGE"
    res.write('Hello World!'+'<br>'); //el saludo
    res.write(hostname);  //hostname 
    res.end(); 
}).listen(5000); //puerto escucha
  

console.log('port:5000'); 
```
2) Guarda el archivo como "app.js" 
## Crear un DockerFile para contenerizar esa app
En el mismo directorio que el paso anterior: 
EJEMPLO: DEMO/app

1) en un editor de texto copia el siguiente código y guardalo: 
```Dockerfile
#seleccionamos la imagen mínima y oficial de "node" basada en linux alpine
FROM node:lts-alpine
#Creamos nuestro directorio de trabajo
WORKDIR /opt/testjs
#Copiamos nuestra app
COPY app.js .
#exponemos el puerto 5000 de nuestra app, para que otros contenedores lo consuman
EXPOSE 5000
#este comando ejecuta y "convierte" la imagen en un contenedor ejecutable.
CMD ["node","app.js"]
```
4) Guarda el archivo como "Dockerfile" ( sin extensión)

5) Construye tu imagen ejecutando desde una consola:

```Shell
docker build -t dintro/myapp:test .
```

la opción "-t" es el nombre de nuestro imagen en formato repositorio/nombre:version .
El  "."  implica que el archivo "Dockerfile" esta en la ruta donde te encuentras.
## arrancarla sin balanceador
```Shell
  docker run -d -e MESSAGE="contenedor1" --name app1 dintro/myapp:test
  docker run -d -e MESSAGE="contenedor2" --name app2 dintro/myapp:test
```
Estos comandos arrancan respectivamente un *contenedor* basado en la *imagen* que recién construimos.

la opción "-e" le entrega la variable de ambiente que necesitamos.

la opción "-d" hace que el contenedor corra en el fondo. 

## Comandos básicos docker
```Shell
  docker ps 
  ```
Esta comando enlista los contenedores corriiendo actualmente.
  ```Shell
  CONTAINER ID        IMAGE                                COMMAND                  CREATED             STATUS              PORTS                  NAMES
6f2bb728b23b        dintro/mybalance:test                "nginx -g 'daemon of…"   13 minutes ago      Up 13 minutes       0.0.0.0:9999->80/tcp   mybalancer
59827fa72a43        dintro/myapp:test                    "node app.js"            23 minutes ago      Up 23 minutes       5000/tcp               app2
b56a364a577e        dintro/myapp:test                    "node app.js"            23 minutes ago      Up 23 minutes       5000/tcp               app1
  ```
  para detener un contenedor en particular ejecutamos: 
  ```Shell
  docker stop <id_contenedor>
  ```
  para enlistar las * *imagenes disponibles* *
  ```Shell
docker image ls
  ```
## definir Dockerfile para un balanceador
  1) en un subdirectorio crear el siguiente archivo "nginx.conf" y copiar el siguiente código:
	

  EJEMPLO DIR: DEMO/nginxbalancer

  ```Shell
worker_processes 4;

events { worker_connections 1024; }

http {
upstream my-app {
    server app1:5000 weight=1;
    server app2:5000 weight=1;
}

server {
    location / {
        proxy_pass http://my-app;
    }
}
}
```
( no entraremos a detalle ya que es configuración de nginx) 


2) Dockerfile para NGINX


```Dockerfile
	#IMAGEN BASE DE NGINX
	FROM nginx
	#DIRECTORIO DE TRABAJO DEL CONTENEDOR
	WORKDIR /etc/nginx/
	#INGRESAMOS NUESTRA CONFIGURACION
	COPY nginx.conf .
	#EXPONEMOS EL PUERTO 80
	EXPOSE 80
```
3) construimos la imagen de nuestro balanceador.
```Shell
docker build -t dintro/mybalance:test .
```
## arrancar la app con balanceador
Partimos del hecho de que los contenedores app1 y app2 están corriendo, de lo contrario rearrancalos.
Estas instrucciones arrancan el contenedor tal cual lo arrancaste la primera vez.
```Shell
docker start app1
docker start app2 
```
Para arrancar el balanceador :


```Shell
docker run -d --name mybalancer -p 9999:80 --link app1:app1 --link app2:app2 dintro/mybalance:test
```

en este caso, la opción "--link" es crucial , ya que le permite al contenedor de nuestro balanceador, hablar con los contenedores app1 y app2.
la opción "-p" mappea el puerto 80 del contenedor al 9999 de nuestro equipo host.

Ahora ingresa en un explorador a : 
```Shell
http://localhost:9999
```
Y carga multiples veces la página para que lo puedas ver en acción!!!

