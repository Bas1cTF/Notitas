Evil-WinRm - Puerto 5985

Cuando se tiene un DNS server, se puede hacer un ataque de zona de transferencia. 

Server Message Block

Kerberos; ¿Que es Kerberos? youtube

Enumeración de smb
- CrackMapExec github
	- `crackmapexec smb <IP>`
- SMBmap
	- `smbmap -H 10.10.11.168 -u null`

- Comprobar con ldap3 
- LDAP no requiere contraseñas
- NTLM - Autenticación de Windows 

Cuando tenemos un usuario:

kerbrute para verificar si existe un usuario

install impacket getnpusers para obtener los usuarios que no requieren autenticación previa

asrepproast

Attacktive Directory THM

./kerbrute userenum --dc <IP> -d <DOMAIN> <wordlist>

- Si la uri no se ponde en minusculas se trata de una máquina windows

No funciona por que la fecha del sistema no es la misma.