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
strings /proc/1/environ | grep ETSCTF 2>/dev/null
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

`sudo nmap -sS --min-rate 5000 -vvv -open -p- -n -Pn -oG nmap/all_ports <target>`

`extract_ports nmap/all_ports`

Los puertos se obtienen del resultado obtenido en extract_ports
`nmap -sCV -p <puertos> -n -Pn -oN nmap/targeted <target>`

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