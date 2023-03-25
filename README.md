# TALLER 7 - AREP

### Autor: Yesid Camilo Mora Barbosa

## Desafio

Desarrolle una aplicación Web segura con los siguientes requerimientos:

1. Debe permitir un acceso seguro desde el browser a la aplicación. Es decir debe garantizar autenticación, autorización e integridad de usuarios.
2. Debe tener al menos dos computadores comunicacndose entre ellos y el acceso de servicios remotos debe garantizar: autenticación, autorización e integridad entre los servicios. 
Nadie puede invocar los servicios si no está autorizado.
3. Explique como escalaría su arquitectura de seguridad para incorporar nuevos servicios.

## Arquitectura

Implementaremos la siguiente arquitectura:

![image](https://user-images.githubusercontent.com/98135134/226805042-98f79598-05ff-4870-bff0-3877bc39b328.png)

## Solución

### 1. Implementación Servidores

Creamos dos servidores de Sparksecure:

![image](https://user-images.githubusercontent.com/98135134/226807214-11ef04dc-c39e-4659-a2d7-439bdcf8a1ba.png)

Maquina(server) 1:

La maquina 1 corre por el puerto 5000:

![image](https://user-images.githubusercontent.com/98135134/226807402-c97f98f4-5d34-48b9-8ec9-c7cf345f49f5.png)

El servidor 1 tiene un end-point para conectarse localmente con el mismo y otro end-point para conectarse con el servidor 2

![image](https://user-images.githubusercontent.com/98135134/226808158-74ec997d-05de-47f7-ad0a-93bcbbf2dbf0.png)

Maquina(server) 2:

La maquina 2 corre por el puerto 5001:

![image](https://user-images.githubusercontent.com/98135134/226807441-d9d09718-a9e0-45ef-97ab-8f4257a318f0.png)

El servidor 2 tiene un end-point para conectarse localmente con el mismo y otro end-point para conectarse con el servidor 1

![image](https://user-images.githubusercontent.com/98135134/227738385-31b1bc05-862d-4157-81ac-88e34c1fa3fb.png)


Ambos servidores se conectan a la clase URLReader, esta clase tiene dos métodos principales

El método readSecureUrl se encarga de validar si puede conectarse de forma segura con la url ingresada, verificando su keystore:

![image](https://user-images.githubusercontent.com/98135134/226807663-61d3848e-f09d-4ef0-8081-53699a16dfd8.png)

Si puede conectarse de forma segura, este método a su vez se conecta con RealURL, el cual es el encargado de conectarse con la URL que se le envia:

![image](https://user-images.githubusercontent.com/98135134/226807786-a9a4c97a-ac50-4580-94e5-9a32de188ba7.png)

### 2. Creación de certificados

En el directorio raiz creamos una carpeta para almacenar los certificados

![image](https://user-images.githubusercontent.com/98135134/227739596-201762b0-3a34-4612-af08-c2020ddceb2d.png)

Creamos el archivo ecikeysyore.p12 con el siguiente comando:

```
keytool -genkeypair -alias ecikeypair -keyalg RSA -keysize 2048 -storetype PKCS12 -keystore ecikeystore2.p12 -validity 3650 
```

Creamos el archivo ecicert.cer con el siguiente comando:

```
keytool -storetype PKCS12 -export -keystore ./ecikeystore2.p12 -alias ecikeypair -file ecicert2.cer 
```

Para terminar en el archivo trusStore.p12 almacenaremos los certificados en los que confiaremos:

```
keytool -storetype PKCS12 -import -file ../maquina2/ecicert2.cer -alias firstCA -keystore myTrustStore1.p12
```

### 3. Crear instancias AWS

Creamos dos instancias de AWS.

![image](https://user-images.githubusercontent.com/98135134/226805259-bf4b6145-6bed-422a-b665-3c6287125bd7.png)

Abrimos los puertos que utilizaremos (5000 sever 1, 5001 server 2):

![image](https://user-images.githubusercontent.com/98135134/227738120-64507d97-d94b-4994-bf54-fb876da3660b.png)


### 4. Conectar a local

Debemos instalar java en las instancias de los servidores

Para esto debemos conectarlos a la consola de las instancias, para esto usamos la coneccion ssh que nos ofrece aws.

![image](https://user-images.githubusercontent.com/98135134/224522078-4b31bca0-c20d-4de7-95f8-27b565c00190.png)

Abrimos una terminal de linux y ejecutamos el comando, teniendo en cuenta que en el mismo directorio debe estar la clave .pm de la instancia.

![image](https://user-images.githubusercontent.com/98135134/224522087-40551feb-8157-418d-a443-ebf189d204f1.png)


### 5. Instalamos Java (versión 17)

Para instalar java 17 utilizamos el siguiente comando en la terminal de la instancia.

Recuerde que debe instalar java para las 2 instancias:

```
sudo yum install java-17-amazon-corretto-devel
```

Luego de instalar java verificamos que se instalo conrrectamente:

```
java -version
```

### 6. Pruebas

Maquina 1:

Método local:

![image](https://user-images.githubusercontent.com/98135134/226807005-52b38179-d087-4da7-9f5e-2ecb0a369cdf.png)

Método Remoto:

![image](https://user-images.githubusercontent.com/98135134/226806960-66f3f556-9ef6-401f-8dda-446b67da30e7.png)

Maquina 2: 

Método local:

![image](https://user-images.githubusercontent.com/98135134/226807043-48e9cbf1-1903-42c7-844d-1cf7d7e60f1a.png)

Método Remoto:

![image](https://user-images.githubusercontent.com/98135134/226807079-415863e8-2cbb-492f-9722-b577702c6b63.png)


### 7. Video Prueba en Vivo

https://youtu.be/HXA-mjuNozo
