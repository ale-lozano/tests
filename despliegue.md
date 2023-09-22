---
tags:
  - despliegue
  - dockers
  - portales
---
# Despliegue de portales web

En el mantenimiento de portales **ecom_portales** se encuentra una pestaña despliegue con la cual podremos configurar y generar de manera automática los entornos de los portales web, tanto en modo **desarrollo** como **producción**, utilizando el nuevo despliegue mediante contenedores de **Docker**.

# Contenido

- [Campos](#Campos)
	- [Base de datos del portal](#Base%20de%20datos%20del%20portal)
	- [Contenedor Docker](#Contenedor%20Docker)
	- [Parametrización Lisa](#Parametrizaci%C3%B3n%20Lisa)
	- [Backend](#Backend)
	- [GitHub y ARM](#GitHub%20y%20ARM)
- [Generación de código y despliegue](#Generaci%C3%B3n%20de%20c%C3%B3digo%20y%20despliegue)
	- [Producción](#Producci%C3%B3n)
	- [Desarrollo](#Desarrollo)
- [Últimos pasos](#%C3%9Altimos%20pasos)


## Campos
### Base de datos del portal

> Estos campos serán utilizados para establecer la
> conexión del backend con la base de datos

- **Host:** ubicación de la maquina en la que está ubicada la base de datos.. En caso de ser un despliegue final o que se quiera usar la base de datos dockerizada utilizar **db** en este campo
- **Puerto:** Puerto en el que está ubicada la base de datos
- **Nombre:** Nombre de la base de datos a la que se va a conectar
- **Usuario:** Usuario que realizará la conexión
- **Contraseña:** Contraseña del usuario

### Contenedor Docker

> Estos campos son para controlar los puertos de los contenedores

- **Puerto bbdd:** Puerto para el contenedor de la base de datos
- **Puerto frontend:** Puerto para desplegar el frontend `Principalmente para desarrollo`
- **Puerto backend:** Puerto en el que se expondrá el backend  `En producción se establecerá el puerto proporcionado por el cliente, en el que se expondrá un nginx` 

### Parametrización Lisa

> Estos campos son para configurar el usuario con el que se realizarán las peticiones a Lisa y el tiempo de vida para la cache 

- **Usuario Lisa:** El usuario con el que se realizarán las peticiones `De momento ECOMOAUTH`
- **Cache peticiones Lisa:** El tiempo en segundos para la duración del cache de las peticiones a lisa.

### Backend

> Estos campos son para la configuración de DJANGO

- **Allowed hosts:** Arreglo de direcciones permitidas separadas por comas `Dominios de la aplicación`
- **Allowed origins:** Ubicaciones web permitidas `CORS (Desde donde se pueden realizar llamadas a la aplicación`
- **Lista de apps:** Listado de las aplicaciones que serán utilizadas por este portal
- **Access token timeout:** tiempo de vida en segundos para el token
- **Refresh token timeout:** tiempo de vida en segundos para el refresh token
- **Resultados por página:** Número de elementos por defecto para la paginación de las consultas
- **Nivel log de acceso:**  Nivel a partir del cual se mostrará los log de acceso
- **Nivel log general:** Nivel a partir del cual se mostrarán los logs

### GitHub y ARM

> Esta sección situada al final de programa es para configurar con que usuario de GitHub se realizará el despliegue
> Además de poder seleccionar ARM como la arquitectura de despliegue

- **Usuario de GitHub:** Texto que se mostrará como nombre en los commits
- **Email de GitHub:** Texto que se mostrará como email en los commits
- **Backend Branch:** Rama en la que se posicionará el backend
- **Frontend Branch:** Rama en la que se posicionará el frontend `Principalmente desarrollo`
- **Arquitectura ARM:** Si está marcado se utilizarán dockers en modo ARM

## Generación de código y despliegue

Luego de parametrizar los campos anteriores se pueden generar los despliegues utilizando los **plugins** laterales del programa
### Producción

Para desplegar este entorno se utilizará el primer plugin *(hacia abajo)* 
Este plugin utilizará el **usuario ftp** del portal objetivo subirá automáticamente el código de despliegue a la máquina objetivo

### Desarrollo

Para desplegar en desarrollo se utilizarán el segundo y tercer plugin *(hacia abajo)* 

El primero de estos **Generar desarrollo** generará los archivos necesarios para realizar el despliegue y los almacenará en **Libra**

El ultimó de estos **Descargar desarrollo**  nos permitirá descargar un archivo `.zip` que contendrá los archivos necesarios.

## Últimos pasos

Una ves tenemos ubicados los archivos de despliegue haremos ejecutable el archivo `run.sh`

```bash
chmod +x ./run.sh
```

A continuación ejecutaremos este archivo

```bash
./run.sh
```

Este `script` realizará todo el trabajo pesado configurando todos los contenedores, volúmenes y redes de `docker`.

**Listo.... con suerte**
