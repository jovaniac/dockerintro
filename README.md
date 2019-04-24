# docker-intro
El objetivo de este repo es generar un ejemplo de uso básico de docker y un contenedor generado por cuenta propia. 
## Tareas: 
* Instalar Docker en Windows 10. 
* Instalar Docker en Ubuntu 18.04
* Usar Imagen de ubuntu server con docker pre-instalado. 
1. crear un ejemplo de un app.js 
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


**Uso de proxy en docker y windows 10**



En caso de que estes detrás de un proxy, asegurate de configuarlo en


* Ve a la barra de tareas y selecciona con clic derecho el ícono de docker.


* Navega a settings: 

<p><img src="https://docs.docker.com/docker-for-windows/images/whale-icon-systray-hidden.png" width="199px" height="242px" float="left">
<img src="https://docs.docker.com/docker-for-windows/images/docker-menu-settings.png" width="199px" height="242px" float="right"></p>


* opción proxies: 
El formato de proxy corporativo es : http://username:password@proxy:port/

<p><img src="https://docs.docker.com/docker-for-windows/images/settings-proxies.png" width="50%" height="50%" float="left"></p>
