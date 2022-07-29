Con el escaneo en el puerto 3000 encuentras en un GraphQL

Graphql hacktricks con query={user{username,password}}

Cuando nos encontramos con un software desconocido probamos a buscar la documentación, tal vez aparezca en github, podemos ver sus archivos. Por lo general los README no vienen en formato .md, podemos probar con .html

Buscar un exploit: `searchsploit <software>` 

Traer el exploit: `searchsploit -m <Number> .`

`script dev/null bash -c`

`usr/share/wordlists/dirbuster/`

# Escalar privilegios
- Ver que binarios hay con permisos SuID -s
	- `find \-perm -4000 2>/dev/null`
- capabilities dadas a binarios
	- `getcap -r / 2>/dev/null`
- Que permisos sudo tenemos (sudoers)
	- `sudo -l`
	- Vemos con que binarios tenemos permisos sudo
- Ver cual es la versión de SO
	- `lsb_release -a`
	- `uname -a`
		- Puedes ver si esta desactualizado
- Ver en que grupo nos encontramos
`id`
- Comprobar conexiones 
`netstat -nat`
- Comprobar si estamos en la máquina victima o un contenedor
`hostname -I`
- Buscar archivos
- Tareas cron
	- `cat /etc/crontab`
- Si no encuentras nada se usa el buen linpeas
	- El directorio /tmp es el que tiene permisos de escritura
	- Se recomienda primero buscar a manita para no esperar a que acabe el linpeas.
- Rutas comunes (tal vez hay backups)
	- `/var/www`
	- `/opt` 

Por la versión dada con `uname -a`, suponemos que se trata de una exploit de kernel. Lo buscamos con searchsploit

date time python error en el exploit.

Famoso pkexec buscar en github el exploit

apktool d <APK> para decompilar

Buscar virtual hosting 

lxd group privesc

# p0rtector

Nos encontramos con un bad redirect.

Magia de requiem:

Host virtuales en las redirecciones, se puede en nginx y apache.
Virtualiza un espacio en el host para especificar distintas aplicaciones o nombres de dominio.

whatweb para obtener el redirect

Los redirect se guardan en /etc/host 
```
10.0.14.18 239545
```
# hairsplit

Visitamos `v1/api.json` deducido de la pista que nos da la descripción. Header: X-REAL-IP

# r0x02dictu

Scripting!!!!!!

Buscar biblioteca requests python

62b085:1337