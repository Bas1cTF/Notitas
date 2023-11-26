- Turnkey Linux
	- Sitio web con isos de distintos servicios

# Configuración básica de switchs y routers
* [Configuración básica de routers Cisco con GNS3: Paso a paso | NetJNL (wordpress.com)](https://netjnl.wordpress.com/2013/08/01/configuracion-de-red-en-routers-cisco/)
- `transport input ssh telnet`
	- Permite establecer que protocolos se van a usar
- `username <username> privilege 15 password <password>`
	- creación de usuario con privilegios de administrador
- `show ruuning-config`
	- Podemos ver la configuración
- En CMD: `net start npf`
	- Para inicializar manualmente el driver de winpcap
- [How to Set a Static IP on Debian 11 – LinuxWays](https://linuxways.net/debian/how-to-set-a-static-ip-on-debian-11/)
* [Troubleshooting network connectivity issues for Windows virtual machines in VMware Workstation (2019836)](https://kb.vmware.com/s/article/2019836)
- Las máscaras de los enlaces son /30 para no desperdiciar direcciones
- Subnetear la red 0 de la tabla 4 para llenar la tabla 5
- `enable`
	- No me acuerdo :c
- `conf t`
	* Modo exec privilegiado
* `enable secret 1234`
	* Configurar password para entrar al modo exec privilegiado
* `#username patito priv 15 pass patito`
	* Configurar un usuario
* `service password-encryption` 
	* Cifrar la password en el running-config
* `ip domain-name <DOMAIN>`
	* Agregar dominio al router
* `R1(config)#ip ssh rsa keypair-name sshkey` 
	* Configurar RSA en SSH
* `R1(config)#crypto key generate rsa usage-keys label sshkey modulus 1024`
	* Generar llaves RSA
* `R1(config)#ip ssh v 2`
	* Versión de SSH?
* `R1(config)#ip ssh time-out 30`
	* Cierra conexión si no hay interacción en el tiempo establecido.
*  `R1(config)#ip ssh authentication-retries 3`
	* Numero de intentos
* `line vty 0 15`
* `pass cisco`
* `login local`
* `transport input ssh telnet`
* `exit`
* `username cisco priv 15 pass cisco`

# Rip con OSPF
## En el router central
* router rip
	* version 2
	* redistribute ospf 1 metric 1
	* redistribute static
	* network `<ID RED>`
	* no auto-summary
- router ospf 1
	- redistribute rip subnets
	- redistribute static subnets
	- network 

## Conceptos de lista de control de acceso
* Genericas
* Se encuentran en el router
* Sentencias de tipo aceptar y rechazar
* Se puede filtrar por protocolo y por puerto 
* Se limitan a protocolo IP, ICMP, TCP, UDP
Aplicación en interfaces 
* Sentencias (in|out)
* Protip: Hacer un archivo txt 
* De lo más especifico a lo más general.
* En una ACL estándar lo más cerca del destino
* Cuando solo es una dirección IP se puede utilizar la palabra reservada `Host` 
* En una ACL extendida lo más cerca del origen
* `any (o permit?) any` deja pasar todo
* `deny any` bloquea todo

## DNS
* /etc/resolv.conf para no perder el DNS
* /etc/bind