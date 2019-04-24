# docker-intro
El objetivo de este repo es generar un ejemplo de uso básico de docker y un contenedor generado por cuenta propia. 
## Tareas: 
* Instalar Docker en Windows 10. 
* Instalar Docker en Ubuntu 18.04
* Usar Imagen de ubuntu server con docker pre-instalado. 
1. [Crear un ejemplo de un app.js](#Crea-una-app-en-node-js)
2. Crear un DockerFile para contenerizar esa app.
3. arrancarla sin balanceador. 
4. Comandos básicos docker. 
5. definir Dockerfile para un balanceador. 
6. arrancar la app con ayuda del balanceador.
### Instalar Docker Engine en Windows 10.
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

4. abre un powershell y asegurate que docker esté funcionando:

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
