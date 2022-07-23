### Identificar SO
`ping -c <target>`

ttl = 63 64 Linux

ttl = 127 128 Windows

***Enumerar Bien!!!***

Probar con los nmap.

Tema a investigar Java Groovy Server Side Template Injection

`whatweb <target>` - Identifica las tecnologías de la página web

`wfuzz -c -hc=404 -t 200 -w <wordlist> <target>/FUZZ.jsp`

Cualquier puerto desconocido que venga intentar conectarse con netcat.

Directorio `/content` para cualquier archivo de la máquina.

Al conectarse a ftp, obtenemos el REAME.txt, donde viene la versión del server, buscando encontramos el CVE-2020-9484.

Ctrl + Q Borrar el comando y guardarlo

En hacktricks puedes buscar el puerto y viene interacciones.

`pushd <dir>` - Viajar y regresar

`popd` - Para regresar a donde estabamos.

*Si no conecta con una reverse shell sencilla, intentar con la de otros lenguajes.*

Serializar los payloads, curl para obtener la reverse shell de nuestra máquina, chmod 777 /tmp/rev.sh
[HTB: Feline | 0xdf hacks stuff](https://0xdf.gitlab.io/2021/02/20/htb-feline.html)