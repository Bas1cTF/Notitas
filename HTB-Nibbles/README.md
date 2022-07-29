Escaneo con nmap

whatweb para deducir

- Fuzzing chiquito
- `nmap -p80 --script http-enum <IP> -oN webscan` 
- Hovering?
- Si encontramos el repositorio podemos ver que contiene el software utilizarlo y buscarlos.
- En un CMS ver los plugins, para ver si se puede obtener una ruta o si hay un plugin vulnerable.
- Buscar un exploit del CMS.
- A veces el login es el nombre de la máquina.
- Probar combinaciones obvias.
- `system($_REQUEST['cmd'])`
- Para buscar donde se almacena lo que subimos en un CMS, por lo general es content en WP es wp-content.
- Alternativas a which
	- `command -v <BIN>`
	- `locate`
- Obtener columnas 
	- `stty size`
- Dar permisos a la bash
	- `chmod u+s /bin/bash`
	- `bash -p?`