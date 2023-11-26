### 1.5.1 Heterogeneidad
La heterogeneidad se aplica para los siguientes elementos:
* Redes
* Hardware. Los tipos de datos cómo los enteros pueden ser representados de diferentes formas en los distintos tipos de hardware.
* Sistemas Operativos
* Lenguajes de programación. Diferentes lenguajes de programación tienen distintas representaciones para caracteres, y estructuras de datos. 
* Implementaciones de diferentes desarrolladores. Programas implementados por diferentes desarrolladores no pueden comunicarse entre sí, al menos que utilicen estandares en común. Los estandares necesitan acordarse y ser adoptados.
#### Middleware
Capa de software que provee programación abstracta, así como, ocultar la heterogeneidad de las redes, el hardware, sistemas operativos y lenguajes de programación. Para resolver el problema de heterogeneidad, un middleware provee un modelo computacional uniforme para ser usado por los programadores de servidores y aplicaciones distribuidas.
Ejemplos: 
* Common Object Request Broker (CORBA)
* Java Remote Method Invocation (RMI)
##### Heterogeneidad y código movil
El *código movil* es aquel que puede ser transferido de una computadora a otra y poder ser ejecutado en la computadora destino.
Ejemplos:
* Java con el uso de la máquina virtual de Java
* Programas de Javascript de algunas páginas web que se cargan en los navegadores de los clientes. 
### 1.5.2 Apertura
La apertura es la característica que determina si un sistema puede ser extendido y reimplementado de varias formas. 
La apertura de los sistemas distribuidos es determinada principalmente por el grado al cual nuevos servicios de recursos compartidos pueden ser añadidos y dispuestos para su uso por una variedad de programas cliente.
* La apertura no puede ser lograda sin que la especificación y documentación de las interfaces claves de sfotware de los componentes de un sistema esten disponibles para los desarrolladores de software.
* Los sistemas abiertos se caracterizan por el hecho de que sus interfaces claves sean publicadas.
* Los sistemas distribuidos abiertos se basan en un mécanismo de comunicación uniforme e interfaces publicadas para el acceso de recursos compartidos.
* Los sistemas distribuidos abiertos pueden ser construidos con hardware y software heterogéneos. 

### 1.5.3 Seguridad
La seguridad para recursos de información cuenta con tes componentes:
* Confidencialidad, protección contra el acceso de individuos no autorizados.
* Integridad, protección contra la alteración o la corrupción.
* Disponibilidad, protección contra la interferencia en las formas de acceder a los recursos.
Problemas de seguridad:
#### Ataques de denegación de servicios
Puede ser logrado a tráves de bombardear un servicio con una gran cantidad de solicitudes  con el objetivo de que los usuarios serios son incapaces de usar este servicio.
#### Seguridad de código móvil
El código móvil necesita ser manejado con cuidado. Por ejemplo, si alguien recibe un programa ejecutable por correo, este puede ser dañino.
### 1.5.4 Escalabilidad
Un sistema se considera escalable si este puede operar efectivamente cuando tiene un considerable incremento en el número de recursos y el número de usuarios. 
El diseño de sistemas distribuidos escalables presentan los siguientes desafíos: 
#### Controlar el gasto de recursos físicos:
Cuando la demanda de un recurso crece, debería ser posible extender el sistema a un costo razonable. En general, para que un sistema con n usuarios sea escalable, la cantidad de recursos físicos necesarios para soportarles debe ser a lo mucho O(n).
#### Controlar la perdida de rendimiento:
El tiempo para acceder a la información estructurada jerárquicamente es O(log *n*), donde n es el tamaño de la información. Para que un sistema sea escalable, la mayor perdida de rendimiento no debe ser peor que esto.
#### Prevenir el agotamiento de recursos de software:
Un ejemplo de esto es la falta de direcciones IP, las cuales en un principio fueron diseñadas para ser de 32 bits. Es díficil predecir la demanda que va a tener un sistema en los años siguientes. Sin embargo, sobrecompensar para un crecimeiento futuro puede ser peor que adaptarse a un cambio cuando estamos forzados. Direcciones de Internet más grandres ocupan más espacio en mensajes y almacenamiento de la computadora.
#### Evitar cuellos de botella en el rendimiento: 
Los algoritmos deben ser descentralizados para evitar tener cuellos de botella en el rendimiento. Algunos recursos compartidos son accedidos muy frecuentemente, cómo varios usuarios accediendo a la misma página web.

Idealmente el sistema y la aplicación no deberían necesitar cambios cuando la escala del sistema incrementa.

# Lectura 2
En un sistema distribuido lograr un acuerdo con respecto al teimpo no es trivial.

### Relojes físicos
Un cronómetro de computadora es un cristal de cuarzo mecanizado con precisión. 
Cuando un sistema tiene *n* computadoras, los *n* cristales funcionarán a velocidades ligeramente diferentes. 
**Distorsión de reloj**: Los relojes se salgan gradualmente de sincronía y arrojen diferentes valores cuando se leen. **Consecuencia:** Los programas que esperan que la hora asociada de un elemento sea correcta e independiente del reloj utilizado en que se generaron pueden fallar.
Por eficiencia y redundancia se considera deseable tener varios relojes.
Desde el siglo XVII el tiempo se mide astronómicamente. La **Trayectoria solar** es el hecho de que el Sol alcance su punto aparentemente más alto en el cielo. El intervalo entre dos trayectorias se conoce como día solar.  
Aplicaciones tales como correduría financiera, auditoría de seguridad, y detección de colaboración.
el TAI es exactamente el número medio de marcas de los relojes de cesio 133, desde la medianoche del1 de enero de 1958 (el comienzo del tiempo), dividido entre 9 192 631 770.
.
El BIH resuelve el problema introduciendo segundos vacíossiempre que la discrepancia entreel TAI y el tiempo solar alcanza los 800 ms

Esta corrección da lugar a un sistema de tiempo basado en segundos TAI constantes, pero que permanecen en fase con el movimiento aparente del Sol.

El UTC es la base de todo conteo de tiempo civil moderno; prácticamente ha reemplazado el viejo estándar, Tiempo Medio de Greenwich, que es el tiempo astronómico.