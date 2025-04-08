# PickleRick
Máquina resuelta de *TryHackMe* en la que se trabaja la enumeración y *fingerprinting*, *reverse shell*.
<div>
  <img src="https://img.shields.io/badge/-Kali-5e8ca8?style=for-the-badge&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/-Nmap-6933FF?style=for-the-badge&logo=nmap&logoColor=white" />
  <img src="https://img.shields.io/badge/-Dirsearch-005571?style=for-the-badge&logo=dirsearch&logoColor=white" />
  <img src="https://img.shields.io/badge/-python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
</div>

## Objetivo

Explicar la realización del siguiente _Capture the flag_ perteneciente a la plataforma *TryHackMe*. Este desafío ambientado en la serie de *Rick y Morty* requiere explotar un servidor web y encuentrar 3 ingredientes (*flags*) para ayudar a *Rick* a crear una poción que lo transforme nuevamente en humano. Para ello, se deberá interactuar con un formulario, penetrar en la máquina y realizar una escalada de privilegios.

## Que hemos aprendido?

- Realizar fingerprinting y enumeración de puertos y enumeración web (mediante *Dirsearch*).

- Escalada de privilegios.

## Herramientas utilizadas

- *Kali Linux*.
- Enumeración: *Nmap*, *Dirsearch*.
- Penetración: . 

## Steps

### Enumeración y fingerprinting

La máquina a vulnerar pertenece a la plataforma *TryHackMe*, la propia web te proporciona la IP de la máquina a vulnerar. La conexión a esta se hace mediante una VPN que te proporciona THM y asigna una nueva IP para que tu máquina interactúe con la máquina víctima.
El primero paso es lanzar **Nmap** sin *ping* (-Pn) y con los *scripts* por *default* de la herramienta (-sC) para que encuentre vulnerabilidades. Además se quieren conocer las versiones que corren en cada puerto (-sV) y el sistema operativo (-O). No se indica, específicamente, que compruebe todos los puertos pues en este tipo de máquina no será necesario.

![Captura de pantalla 2025-04-04 131854](https://github.com/user-attachments/assets/8527c885-9cdd-499a-ba91-bebcf4b73f80)

El comando devuelve 2 puertos TCPs abiertos:  
- En el puerto 22 corre la versión *Openssh 8.2p1*, en un sistema *Ubuntu*, servicio *SSH*.  
- En el puerto 80 corre el servidor *Apache 2.4.41*, en un sistema *Ubuntu*, servicio *HTTP*.

Primero nos dirigimos, desde el navegador, al puerot 80 de la IP dada, donde se puede ver un mensaje de ayuda de *Rick*. Si se inspecciona el código de la página se puede encontrar un comentario con el nombre de usuario *R1ckRul3s*.


![image](https://github.com/user-attachments/assets/52f9925c-635c-4310-bcb4-342b65b72e7e)

Seguidamente, con la ayuda de la herramienta **Dirsearch**, se hace una enumeración de posibles directorios y archivos que se encuentren en la web. Por la parte de directorios, únicamente aparece '/assets' pero su contenido no aporta ninguna pista. Sin embargo,sí aparecen varios archivos interesantes que son alzanzables. *Dirsearch* es lanzado con **Python3** indicando la IP a atacar (-u) y la lista a utilizar para la enumeración (-w); en este caso la 'raft-small-files-lowercase.txt'.

![Captura de pantalla 2025-04-04 134454](https://github.com/user-attachments/assets/664445ee-a900-445c-a035-085657bca083)

Al dirigrnos al archivo '/robots.txt' se encuentra lo que parece ser una contraseña, que junto con el usuario antes encontrado, serán útiles para el otro archivo /login.php', también hallado en la enumeración.

![Captura de pantalla 2025-04-04 134517](https://github.com/user-attachments/assets/efdb16bd-b14c-41bd-bd19-edb158f80200)

### Vulnerabilidades explotadas


**Flag: **
**Flag: **
**Flag: **
