# TALLER 7 - AREP

### Autor: Yesid Camilo Mora Barbosa

## Desafio

Desarrolle una aplicación Web segura con los siguientes requerimientos:

1. Debe permitir un acceso seguro desde el browser a la aplicación. Es decir debe garantizar autenticación, autorización e integridad de usuarios.
2. Debe tener al menos dos computadores comunicacndose entre ellos y el acceso de servicios remotos debe garantizar: autenticación, autorización e integridad entre los servicios. 
Nadie puede invocar los servicios si no está autorizado.
3. Explique como escalaría su arquitectura de seguridad para incorporar nuevos servicios.

## Arquitectura

![image](https://user-images.githubusercontent.com/98135134/226805042-98f79598-05ff-4870-bff0-3877bc39b328.png)

## Solución

### 1. Implementación Servidores

Creamos dos servidores de Sparksecure:

![image](https://user-images.githubusercontent.com/98135134/226807214-11ef04dc-c39e-4659-a2d7-439bdcf8a1ba.png)

Maquina(server) 1:

La maquina 1 corre por el puerto 5000:
![image](https://user-images.githubusercontent.com/98135134/226807402-c97f98f4-5d34-48b9-8ec9-c7cf345f49f5.png)

Este servidor tiene un end-point para conectarse localmente con el mismo y otro end-point para conectarse al otro servidor
![image](https://user-images.githubusercontent.com/98135134/226808158-74ec997d-05de-47f7-ad0a-93bcbbf2dbf0.png)

Maquina(server) 2:
![image](https://user-images.githubusercontent.com/98135134/226807441-d9d09718-a9e0-45ef-97ab-8f4257a318f0.png)

Ambos servidores se conectan a la clase URLReader, la cual se encarga de validar los certificados validos que aceptará el servidor:
![image](https://user-images.githubusercontent.com/98135134/226807663-61d3848e-f09d-4ef0-8081-53699a16dfd8.png)

Este metodo a su vez se conecta con RealURL, el cual es el encargado de conectarse con la URL que se le envia:
![image](https://user-images.githubusercontent.com/98135134/226807786-a9a4c97a-ac50-4580-94e5-9a32de188ba7.png)


### 1. Crear instancias AWS

Creamos dos instancias de AWS.
![image](https://user-images.githubusercontent.com/98135134/226805259-bf4b6145-6bed-422a-b665-3c6287125bd7.png)

### Pruebas

Maquina 1:
![image](https://user-images.githubusercontent.com/98135134/226806960-66f3f556-9ef6-401f-8dda-446b67da30e7.png)

![image](https://user-images.githubusercontent.com/98135134/226807005-52b38179-d087-4da7-9f5e-2ecb0a369cdf.png)

Maquina 2: 
![image](https://user-images.githubusercontent.com/98135134/226807043-48e9cbf1-1903-42c7-844d-1cf7d7e60f1a.png)

![image](https://user-images.githubusercontent.com/98135134/226807079-415863e8-2cbb-492f-9722-b577702c6b63.png)

