# MTIE
Proyecto Final - Modelos de Arquitecturas Orientadas a Servicios

### JUAN JOSE ARREDONDO ESTRADA
### 615432


## Introducción

Este proyecto hace uso de los siguientes componentes por medio del uso de contenedores.

- [x] MySQL 
- [x] Logstash 
- [x] Elasticsearch 
- [x] Kibana 
- [X] Docker-Compose


_Para poder realizar el despliegue correctamente del proyecto ocuparemos lo siguiente_


- Sistema Operativo
  - Windows 10 Pro
  - Memoria RAM 8GB Mínimo
  - Disco Duro 500GB Mínimo
  - Procesador i5 ó i7

- Software
  - [Oracle Virtual Box](https://download.virtualbox.org/virtualbox/6.1.22/VirtualBox-6.1.22-144080-Win.exe/)
  - [Chocolatey](https://chocolatey.org/install)
  - [Docker Desktop](https://www.docker.com/products/docker-desktop)
  - [Docker Compose](https://community.chocolatey.org/packages/docker-compose)

- Archivos
  - Queries SQL Personalizados
  - Rancher OS [Descarga Aquí](https://github.com/rancher/os/releases/download/v1.5.8/rancheros.iso/)



<a name="Instalación"></a>
## Instalación 🔧

<a name="Speed"></a>
#### Ejecución Rápida


# Ejecutamos el siguiente comando
> docker-compose up --build -d

<a name="Win10"></a> 
### Configuración Windows 10

Deshabilitar **Hyper-V** por medio de *Activar o desactivar las características de Windows*. 
   - En caso de contar con Windows 10 Home, deshabilitar las opciones de *Virtual Machine Platform* y *Windows Hypervisor Platform*. 
   
![Hyper-V](https://www.elcegu.com/wp-content/uploads/2019/01/2019-01-31_21-29-55.png)
   
Instalar [VirtualBox](https://www.virtualbox.org/wiki/Downloads). 

[![VirtualBox](https://www.igestweb.es/blog/wp-content/uploads/2017/09/Virtualbox-logo.jpg)](https://www.virtualbox.org/wiki/Downloads)

Abrir la consola de **Windows PowerShell**; de preferencia como administrador y ejecutar el siguiente comando: 

    ``` 
    > bcdedit /set hypervisorlaunchtype off 
    ``` 

<a name="Docker"></a> 
### Instalación de Docker Desktop

Instalar [Docker Desktop](https://www.docker.com/products/docker-desktop) y posteriormente reiniciar la computadora. 

[![Docker Desktop](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSrQRbJtTXmruUZNgXGDTbXEP2yUV0_cKm_D7l6Ahxi5x-QjOci9KHa32Nie3NyCOnyM70&usqp=CAU)](https://www.virtualbox.org/wiki/Downloads) 

    Nota: Si después de reiniciar aparece la siguiente ventana; no hay que preocuparse, solo de clic en el botón OK y continuar con las instalaciones. 
    
![Docker Error](https://user-images.githubusercontent.com/59643335/103649036-13a48000-4f5e-11eb-8154-bce9cfccf31b.png) 

<a name="Chocolatey"></a> 
### Instalación de Chocolatey

Instalar **Chocolatey** <img src="https://upload.wikimedia.org/wikipedia/commons/b/b0/Chocolatey_icon.png" data-canonical-src="https://gyazo.com/eb5c5741b6a9a16c692170a41a49c858.png" width="28" height="28" /> con **Windows PowerShell** <img src="https://4.bp.blogspot.com/-VnHaVPAfOms/XDepW52T1BI/AAAAAAAAGQo/ZzujNs2KPkEmmtF1Astea01BkZ6RGStswCLcBGAs/s1600/powershell.png" width="28" height="28" /> con los siguientes comandos: 


    
Revisar la versión de Chocolatey instalada con el comando: `choco` para visualizar la versión y el comando de ayuda.

<a name="Docker-Machine"></a> 
### Instalación de Docker Machine

Instalar **docker-machine** con **Chocolatey** ejecutar el siguiente comando: 

    ``` 
    > choco install docker-machine 
    ``` 
    
Para validar la instalación ejecutar el comando: `docker-machine version`. 

<a name="Create-VM"></a> 
## Creación de máquina virtual con Docker Machine 

Para crear la máquina, a la cual llamamos MTIE615432; se debe ejecutar el siguiente comando: 

    ``` 
    > docker-machine create --driver virtualbox --virtualbox-cpu-count 2 --virtualbox-disk-size 10000 --virtualbox-memory 4096 --virtualbox-boot2docker-url https://releases.rancher.com/os/latest/rancheros.iso MTIE615432
    ``` 
    
    _--virtualbox-cpu-count: Número de CPU que se utilizarán para crear la máquina virtual._  
    _--virtualbox-disk-size: Tamaño del disco para el host en MB._  
    _--virtualbox-memory: Tamaño de la memoria del host en MB._  
    _--virtualbox-boot2docker-url: URL de la imagen de boot2docker (Última versión disponible)._ 
 
Revisar las máquinas disponibles con: `docker-machine ls`. 
Para iniciar o detener la máquina. 
    ``` 
    > docker-machine stop Name_VM
    > docker-machine start Name_VM 
    ``` 
    
<a name="Create-Containers"></a> 
## Creación de contenedores 

Iniciar sesión a la máquina mediante SSH con el comando:  

    ``` 
    > docker-machine ssh Name_VM 
    ``` 
Configurar variable **vm.max_map_count** dentro del archivo de configuración sysctl para asignar el número máximo de áreas de mapa de memoria que se puede tener en un proceso. Agregar al final del archivo: **vm.max_map_count=2621444**. 

    ``` 
    > sudo vi /etc/sysctl.conf 
    ``` 

 Para volver a cargar la configuración del archivo con el nuevo valor, ejecutar: `sudo sysctl -p`.

Al iniciar RancherOS con un archivo de configuración, se puede seleccionar qué consola se quiere utilizar. Para ver el listado de las consolas disponibles en RancherOS se utiliza el comando `sudo ros console list`. Después de identificar la consola a utilizar, se ejecuta el siguiente comando: 

    ``` 
    > sudo ros console switch ubuntu 
    ``` 
    
    _Al finalizar, se va a cerrar la sesión de la máquina y se tendrá que hacer de nuevo el login con ssh._ 
    
<a name="docker-compose"></a> 
### Instalación de Docker-Compose

Instalar docker-compose dentro de docker-machine:

    ``` 
    > sudo curl -L https://github.com/docker/compose/releases/download/1.27.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose 
    > sudo chmod +x /usr/local/bin/docker-compose
    ``` 

<a name="Clon-Repo"></a> 
### Clonar repositorio

Para ejecutar un Alias Git Temporal para no realizar la instalación. 

    ``` 
    > alias git="docker run -ti --rm -v $(pwd):/git bwits/docker-git-alpine" 
    ``` 
Clonar en la máquina el repositorio de este proyecto. 

    ``` 
    git clone https://github.com/karroyodev/MTIE513-CICD-PGH-KTAC.git 
    ``` 

<a name="Clon-Repo"></a> 
### Configuración de volumenes y data

Crear las carpetas de volumes para Elasticsearch dentro de la carpeta del proyecto y brindar permisos. 

    ``` 
    sudo mkdir -p volumes/elk-stack/elasticsearch 
    cd volumes/elk-stack/ 
    sudo chmod 777 elasticsearch/ 
    ``` 
    ``` 
    sudo mkdir -p volumes/elk-stack/elasticsearch && cd volumes/elk-stack/ && sudo chmod 777 elasticsearch/ 
    ``` 
    Revisar que los permisos se hayan concedido con `ls -l`.
    
<a name="Comandos-Docker"></a> 
## Comandos Docker

Comando                             | Descripción
------------                        | -------------
docker-machine ls                   | Listado de máquinas con estado, dirección y versión de Docker 
docker-machine start                | Inicia la máquina llamada default, en caso de existir
docker-machine stop                 | Detiene la máquina llamada default, en caso de existir
docker-machine start Name_VM       | Inicia la máquina indicada
docker-machine stop Name_VM        | Detiene la máquina indicada
docker-machine restart Name_VM     | Reiniciar la máquina
docker-machine kill Name_VM        | Forza a que la máquina se detenga abruptamente
docker-machine status Name_VM      | Obtierne el estado de la máquina
docker-machine ip Name_VM          | Obtierne la dirección IP de la máquina 
docker-machine rm Name_VM          | Eliminar la máquina creada
docker-machine ssh Name_VM         | Iniciar sesión a la máquina por SSH 
docker ps                           | Listado de los contenedores que están corriendo 
docker ps -a                        | Listado de todos los contenedores 
docker start CONTENEDOR             | Inicializa el contenedor
docker stop CONTENEDOR              | Detiene el contenedor
docker restart CONTENEDOR           | Reinicia el contenedor
docker pause CONTENEDOR             | Suspende todos los procesos del contenedor especificados 
docker unpause CONTENEDOR           | Reanuda todos los procesos dentro del contenedor
docker kill CONTENEDOR              | Envía una señal SIGKILL al contenedor
docker logs --follow --tail n CONTENEDOR | Muestra el número de líneas indicadas del registro de salida (log)
docker stop $(docker ps -a -q)      | Detiene todos los contenedores
docker rm -f $(docker ps -qa)       | Elimina todos los contenedores
docker images                       | Listado de imágenes con su nivel, repositorio, etiquetas y tamaño 
docker image rm IMAGEN              | Elimina la imágen
docker rmi -f $(docker images -a -q) | Eliminar todas las imagenes del repositorio


<a name="Despliegue"></a>
### Despliegue
Entrar a la carpeta creada al clonar el repositorio. Crear los contenedores con el archivo YAML llamado **\*docker-compose\***. 
    
    ``` 
    sudo docker-compose up --build -d 
    ```

<a name="Start"></a>
#### Primer Inicio ✔️

Para comenzar por primera vez el despligue del proyecto, dentro de este mismo se encuentra un archivo de tipo bash, el cual puede copiar directamente a su raiz de su maquina virtual y ejecutar dicho archivo

> initial.sh

    ``` 
    sh initial.sh
    ```

Este archivo lo que hara sera ejecutar complemente todo lo necesario para poder hacer el despliegue de los contenedores y bases de datos

<a name="Update"></a>
#### Actualización ❓

Para realizar una actualizacion del repositorio, ejecute el archivo de tipo bash que se encuentra en este mismo proyecto, para que ejecute la actualizacion correctamente sin problemas

> update.sh

    ``` 
    sh update.sh
    ```

<a name="Errors"></a> 
## Posibles Errores 
En caso de que el contenedor de MySQL durante la revisión de los logs muestre el siguiente error: 

> mbind: Operation not permitted 
 
Agregar en el archivo de **\*docker-compose\*** las siguientes líneas: 

``` 
cap_add:
    - SYS_NICE  # CAP_SYS_NICE
``` 

La cual agrega capacidades del contenedor para aumentar el valor de proceso, establecer políticas de programación en tiempo real y afinidad de CPU, entre otras operaciones. 

