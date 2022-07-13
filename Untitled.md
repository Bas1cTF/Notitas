- Ubicaciones flags echoctf:
	-  /etc/passwd (System)
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
		- env (`env`, `strings /proc/*/environ | grep ETSCTF`) /TODO/ Verificar comando en el gist
		- /etc/shadow (System)
		- **Último recurso**
			- `find / -name ETSCTF.* 2>/dev/null`
			-  `grep -iRl "ETSCTF" /`
*Nota: Las variables de entorno cambian de acuerdo al usuario, se pueden buscar en procesos o subprocesos*
##### Protip
- Checklist en equipo
	- Maquinas resueltas.
	- Partes revisadas (Para evitar rabbit holes)
	- Sospechas de vulnerabilidad.
	- Doble/triple chequeo.
	- Compendio de contraseñas encontradas/crackeadas para hacer password spraying
`new_target tutorial <IP>`
##### Escaneo 
`nmap -T5 -vvv -open -p- -n -Pn -oG nmap/all_ports <target>` 

`sudo nmap -sS --min-rate 5000 -vvv -open -p- -n -Pn -oG nmap/all_ports <target>`

`extract_ports nmap/all_ports`

Los puertos se obtienen del resultado obtenido en extract_ports
`nmap -sCV -p <puertos> -n -Pn -oN nmap/targeted <target>`

``` Python
import pty
pty.spawn("/bin/bash")
```
