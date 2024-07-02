---
Status: Ok
Refactored: true
Requirements: nvdlib, datetime, os, retrying, requests.exceptions
Last modification: 2024-06-19
1st deploy: 2024-06-25
Author: Juan Enrique López Marcos (CP)
Functions: get_nist_vulnerabilities, create_header_properties, write_dict_to_md
---
**Resumen**

Mediante la librería nvdlib disponemos de acceso a las publicaciones NIST sobre vulnerabilidades, mediante el código desarrollado se facilita la obtención de las vulnerabilidades en formato markdown (orientado a la integración con bóvedas Obsidian). Se han añadido parámetros propios de la vulnerabilidad como propiedades de la misma que permitan ser filtradas.


**Estructura**

No requiere de inputs, únicamente se pide como parámetro el número de días a revisar y tras la ejecución creará una estructura de carpetas de la siguiente manera: `outputs\NIST\save_by_date\{%Y%m%d}`
De este modo, cada vulnerabilidad quedar guardada en la carpeta con la fecha de publicación de la misma.


**Funciones**

Enumeramos las funciones creadas para este código junto con una breve descripción de la utilidad.


- *get_nist_vulnerabilities ()*
	Argumentos requeridos:
	- *s_date*: Fecha de inicio de obtención de las vulnerabilidades.
	- *e_date*: Fecha finalización de obtención de las vulnerabilidades, por defecto será el día de ejecución y vendrá de un parámetro definido previamente.
	
	Funcionalidad: Mediante esta función se llama a la librería nvdlib con el objetivo de retornar un objeto cve (pseudo diccionario) que contendrá las vulnerabilidades publicadas en NIST entre las fechas definidas. Se ha añadido un decorador (@retry) que se encargará de re-ejecutar la llamada un total de 5 ocasiones esperando un total de 2 segundos entre cada intento.

- *create_header_properties ()*
	Argumentos requeridos:
	- *cve*: Objeto retornado por el método nvdlib.searchCVE (muy similar a un diccionario) que contiene las vulnerabilidades retornadas por  *get_nist_vulnerabilities ()*. Al estar dentro de un bucle que recorre , únicamente recibirá una vulnerabilidad.
	
	Funcionalidad: Con el objetivo de integrar en el fichero markdown final las propiedades por las que poder filtrar en obsidian. Mediante la función se mapean una serie de características (no siempre disponibles) y se retorna una cadena para poder añadir al principio del documento .md.


- *write_dict_to_md ()*
	Argumentos requeridos:
	- *cve*: Objeto retornado por el método nvdlib.searchCVE (muy similar a un diccionario) que contiene las vulnerabilidades retornadas por  *get_nist_vulnerabilities ()*. Al estar dentro de un bucle que recorre , únicamente recibirá una vulnerabilidad.
	- *write_name*: Cadena que define el nombre del archivo y path de la ruta de guardado.
	
	Funcionalidad: Esta es la función principal de construcción de las vulnerabilidades. Es llamada dentro de un bucle que recorre el conjunto de vulnerabilidades obtenidas por *get_nist_vulnerabilities ()* y se encarga de crear el archivo en la ruta especificada, llamar al constructor de las propiedades *create_header_properties ()*, añadir los elementos restantes definidos al archivo y guardar y cerrar la escritura del mismo.






