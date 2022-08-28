# Baby RSA
La mala implementación fue al cifrar caracter por caracter, en lugar de toda la cadena. 

La forma de resolverlo era hacer fuerza bruta.

# CookieBreak
El urandom no era vulnerable.

El modo ECB te da bloques independientes en cada cifrado.

Por el tamaño de la llave (16 bytes), te va a dar bloques de ese tamaño (16 bytes), si se supera el tamaño del bloque, entonces lo mete en otro bloque y le mete padding. 

No pasa nada si no envías todos los bloques. 

Se puede poner 16 caracteres antes de la palabra admin para generar un bloque independiente. Entonces en la cookie tendremos dos bloques, del cual extraemos 

# CookieBreak v2
Esquema de firma de RSA

Firmas un mensaje con el exponente privado, es decir, con la llave privada.

Las firmas deben de tener padding.

Se resolvía por medio de un blind signature attack

# Bad RSA

Chinese Remain Theorem - CRT 
