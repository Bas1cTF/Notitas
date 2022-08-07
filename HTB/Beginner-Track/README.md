# lame
Puertos abiertos
- Puerto 21 ftp 2.3.4
    - Iniciar como anónimo permitido
- Puerto 139 y 445 Samba 3.0.20
    - login con guest
    - Se puede acceder a la carpeta tmp
- Puerto 3632 distccd v1

Buscando un exploit para Samba 3.0.20 encontramos la vulnerabilidad usermap, para aprovecharla usamos el siguiente exploit: 
- [CVE-2007-2447](https://github.com/amriunix/CVE-2007-2447)
