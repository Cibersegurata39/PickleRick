# PickleRick
Máquina resuelta de *TryHackMe* en la que se trabaja la enumeración y *fingerprinting*, *reverse shell*.
<div>
  <img src="https://img.shields.io/badge/-Kali-5e8ca8?style=for-the-badge&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/-Nmap-6933FF?style=for-the-badge&logo=nmap&logoColor=white" />
  <img src="https://img.shields.io/badge/-Dirsearch-005571?style=for-the-badge&logo=dirsearch&logoColor=white" />
  <img src="https://img.shields.io/badge/-Metasploit-2596CD?style=for-the-badge&logo=metasploit&logoColor=white" />
  <img src="https://img.shields.io/badge/-python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/-steghide-FF5200?style=for-the-badge&logo=steghide&logoColor=white" />
  <img src="https://img.shields.io/badge/-Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
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

El comando devuelve 2 puertos TCPs abiertos:  
- En el puerto 22 corre la versión *Openssh 7.6p1*, en un sistema *Ubuntu*, del servicio *SSH*.  
- En el puerto 80 corre el servidor *Nginx 1.14.0*, en un sistema *Ubuntu*, en el servicio *HTTP*.

### Vulnerabilidades explotadas


**Flag: **
**Flag: **
**Flag: **
