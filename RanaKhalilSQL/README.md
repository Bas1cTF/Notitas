# SQL Injection 
- Es una vulnerabilidad en la que un atacante interfiere con las consultas (*queries*) que hace una aplicación a una base de datos.
## Clasificación:
- **Clásica**. El atacante usa el mismo canal para realizar el ataque y obtener los resultados.
	- Error. Forzar a la base de datos a un error que da información acerca de como funciona, el tipo, la versión.
	- Union. Se utiliza el operador `UNION` para combinar el resultado de dos consultas, a parte de la original, puedes obtener el resultado de la consulta que prefieras como usuarios de una tabla.
- **Blind**. No hay transferencia de datos entre la aplicación y la base de datos, por lo que, no puedes visualizar la información directamente en la aplicación. Se realizan consultas de verdadero o falso. Toma más tiempo realizarlo que una inyección clásica.
	- *Boolean-based*. Utiliza sentencias con condicionales booleanas, el resultado depende de si el resultado de la query es TRUE o FALSE.
	- *Time-based*. En esta técnica se utiliza pausas en la base de datos por un tiempo especifico, antes de conseguir los resultados, si se obtiene una respuesta después del tiempo especificado, entonces la sentencia se ejecutó correctamente.
- **Out-of-band**. Consiste en llevar una conexión a la red fuera de banda (*Out-of-Band*) a un sistema que controlamos. No es común. Se utilizan varios protocolos (DNS, HTTP, etc...)
## Finding SQLi Vulnerabilities
Tenemos dos principales categorías de escenarios:
- **Black Box Testing**. Cuando no se nos proporciona mucha información acerca del sistema. Por lo general, solo tenemos acceso a la URL, simulando a un atacante externo que de igual forma solo tiene acceso a la URL.
- **White Box Testing**. Tenemos acceso total a la aplicación incluyendo el código fuente.
### Black Box Testing
- "Mapear" la aplicación: Visitar la URL de la aplicación, ir a tráves de las páginas a las cuales se puede acceder como el  usuario al que tengamos acceso (o simplemente sin usuario). 
	- Hacer notas de los posibles input que interactuan con el back-end. 
	- Tratar de entender como funciona la aplicación. 
	- Tratar de figurar la lógica de la aplicación.
	- Buscar subdominios de la aplicación.
	- Enumerar directorios y páginas que no sean visibles directamente desde la aplicación.
	- Tener un proxy capturando silenciosamente todas las peticiones realizadas a la aplicación.
	- Listar inputs que posiblemente interactuen con una base de datos.
*La fase de reconocimiento es muy importante y por lo general es pasada por alto por la mayoria de principiantes que prefieren tirar ataques a las entradas que encuentren. Lo anterior, es como si pusieras una herramienta automatizada a realizar un test en la aplicación, la cual solo puede encontrar las vulnerabilidades menores, pero también es posible encontrar vulnerabilidades en fallas en el código o en páginas que el scanner no puede acceder.*
- ***Fuzz the application***. Enviar caracteres especificos de SQL como `', ", #, --`,  y observar si hay errores o anomalías. Podemos ir refinando nuestrea query para obtener mejores resultados.
	- Los errores pueden ser muy útiles ya que muestran la versión de la base de datos o incluso tener la suerte de obtener la query original.
	- Enviar condicionales Booleanos como `OR 1=1` y `OR 1=2`, observar si hay diferencias en las respuestas de la aplicación. (*Boolean-based*)
	- Enviar payloads diseñados para probar delays en el time cuando se ejecuta una sentencia SQL y observar las diferencias en el tiempo que toma obtener la respuesta. (*Time-based*).
	- Enviar payloads para probar interacción con redes hacia un servidor del que tengamos control. Si obtenemos un resultado en el servidor, entonces es vulnerable.
### White-Box Testing
- Activar el registro (*logging*) del servidor web. Con el proposito de ver como registra el servidor las distintas peticiones que se realizan durante el *fuzzing*, que caracteres pasan y como los procesa, de esta forma es posible saber si es vulnerable a SQL injection
- Activar el registro de eventos (*logging*) de la base de datos.
- "Mapear" la aplicación.
	- Listar todas las funcionalidades visibles.
	- Listar todos los posibles vectores de entrada que interactuen con el backend.
	- Realizar una busqueda con regex en todas las instancias del código que interactué con la base de datos. Puede haber cosas que no encontramos desde la perspectiva de Black-Box
- Code review.
	- Si en una página se tienen sospechas de que puede haber una inyección SQL, se revisa la funcionalidad del código desde el principio hasta donde se confirme teóricamente que se trata de una inyección SQL y luego se confirma prácticamente mediante pruebas.
## How to exploit SQLi vulnerabilities?
- **Error-Based SQLi**. 
	- Diferentes caracteres pueden darte diferentes resultados.
	-  Enviar caracteres especificos de SQL como `', ", #, --`,  y observar si hay errores o anomalías.
- **Union-Based SQLi**. Hay dos reglas para combinar el resultado de dos queries utilizando `UNION`:
	- El número y el orden de las columnas debe ser el mismo en las queries.
	- Los tipos de datos deben ser compatibles.
	- Exploitation:
		- Figurar el número de columnas que la query esta incluyendo.
		- Figurar los tipos de datos de las columnas (interesa que sea string)
		- Utilizar el operador `UNION` para obtener información de la base de datos.
		- El número de columnas se determinan usando `ORDER BY`: 
```SQL
select title, cost from product where id = 1 order by 1
order by 1--
order by 2--
order by 3-- Da error porque no hay tres columnas.
```
- Número de columnas usando valores NULL:
```SQL
select title, cost from product where id = 1 UNION SELECT NULL-- Muestra un error
-- Aumentar el número de NULL's en cada payload enviado hasta no tener errores
' UNION SELECT NULL -- MUESTRA UN ERROR
' UNION SELECT NULL, NULL -- No muestra error
``` 
- Encontrando columnas con tipos de datos útiles.
	- Enviar una serie de payloads que coloquen una string en cada posible columna en turno.
	- `' UNION SELECT 'a',NULL --` Se obtiene un error de conversión si los tipos no son iguales.
- **Boolean-Based Blind SQLi**. 
	- Enviar una condición Booleana que su valor sea FALSO y observar la respuesta.
	- Enviar una condición Booleana que su valor sea TRUE y observar la respuesta.
	- Escribir un programa que use sentencias condicionales para enviar a la base de datos una serie de preguntas de TRUE/FALSE y monitorear la respuesta.
- **Time-Based Blind SQLi**. 
	- Enviar un payload que pause la aplicación por un periodo de tiempo específico.
	- Escribir un programa que use sentencias condicionales para enviar a la base de datos una serie de preguntas de TRUE/FALSE y monitorear el tiempo de respuesta.
	- Si existe un tiempo de pausa al obtener la respuesta la sentencia es Verdadera si no hay tiempo de espera es Falsa.
- **Out-of-Band SQLi**. 
	- Enviar payloads OAST diseñados para provocar una interacción con una conexión out-of-band cuando se ejecuta dentro de una query SQL, y monitorear si tenemos algún resultado en un servidor que controlemos.
	- Es muy útil saber que base de datos se esta utilizando.
	- Depende de si la base de datos tiene estas funciones activadas y si existe algún otro tipo de servicio como un DNS lookup.
	- Dependiendo de la inyección SQL se utilizan diferentes métodos para extraer datos.
- **Herramientas automatizadas**. 
	- sqlmap
		- Open Source.
		- Utilizada para encontrar vulnerabilidades SQLi.
		- Personalizable. Seleccionas en que parámetros realizar la inyección.
		- Si encuentras una inyección SQL puedes pedirle que intente conseguir una shell o que intente extraer los usuarios y contraseñas de la aplicación.
	- Web Application Vulnerability Scanners (WAVS).
		- No solamente buscan inyecciones SQL sino cualquier tipo de vulnerabilidad posible.
## How to prevent SQLi vulnerabilities?
### Defensas principales
- **Opción 1: Utilizar Declaraciones Preparadas (Consultas parametrizadas) en lugar de concatenar una string.**
	- Esta es la forma correcta de mitigarlo.
- Código vulnerable a SQLi:
	- El usuario proporciona la entrada `"customerName"` la cual es interpretada como parte de la sentencia SQL.
``` Java
String query = "SELECT account_balance FROM user_data WHERE user_name= " 
				+ request.getParameter("customerName");
try{
	Statement st = connection.createStatement(...);
	ResultSet res = st.executeQuery(query);
}
```
- La construcción de sentencias SQL es realizada en dos pasos:
	- La aplicación determina la estrucura de la query con *placeholders* para cada entrada del usuario.
	- La aplicación determina el contenido de cada *placeholder*.
- Código no vulnerable a SQLi
```Java
// Estó debe ser BIEN validado
String custname = request.getParameter("customerName");
// Realizar validación de la entrada para detectar ataques
String query = "SELECT account_balance FROM user_data WHERE user_name = ? ";
PreparedStatement pstmt = connection.preparedStatement( query );
pstmt.setString(1,custname);
ResultSet results = pstmt.executeQuery();
```
- **Opción 2: Uso de Procedimientos Almacenados.**
	- Un procedimiento almacenado es un lote de sentencias agrupadas y almacenadas en la base de datos.
	- No siempre es seguro contra inyecciones SQL, sigue la necesidad de realizar llamada en forma parametrizada.
- **Opción 3: Validación de input con Whitelist**
	- Definir que valores son autorizados. Cualquier otra cosa no es autorizada.
	- Útil para valores que no pueden ser especificados como placeholders de parámetros, como el nombre de la tabla.
- **Escapar todos los input proporcionados por el usuario.**
	- Utilizar solo como último recurso.
### Defensas adicionales
- **Defensa en profundidad**
	- **Menor privilegio**
		- La aplicación debe tener el menor nivel de privilegios cuando accede a la base de datos.
		- Cualquier funcionalidad innecesaria en la base de datos debe ser removidad o deshabilitada.
		- Asegurarse de que el [CIS Benchmark](https://www.tarlogic.com/es/blog/controles-cis-las-mejores-practicas-en-ciberseguridad/) para la base de datos en uso sea correctamente aplicado.
		- Todos los parches de seguridad del proveedor deben ser aplicados en el menor tiempo posible, para no dejar una ventana de tiempo que un atacante puede aprovechar.
## Referencia
[SQL Injection | Complete Guide - YouTube](https://www.youtube.com/watch?v=1nJgupaUPEQ)