---
Status: Ok
Refactored: true
Requirements: os, re, shutil, wget, zipfile, yaml, csv, toml, pathlib, collections,requests, pandas, stix2
Last modification: 2024-07-01
1st deploy: 2024-05-27
Author: Juan Enrique López Marcos (CP)
Functions: create_output_folder, find_techniques, find_techniques _in_list, get_unique_ttps_generated, get_dict_ttps_rules_asigned, sentineldf_to_md, splunkdf_to_md, qradardf_to_md, download_unzip, download_folder_unzip, get_list_of_files_sub, delete_folder, get_data_from_branch, get_list_techniques_from_stix2, clean_names, copy_file_to_path, NotTTPatTags, get_unique_items, yaml_classifier_techniques, txt_basename_classifier_techniques, toml_find_techniques, toml_classifier_techniques_elastic, md_classifier_techniques, yara_classifier_techniques, yaml_classifier_techniques_sigma, sigma7_classifier_techniques, get_resume_df
---

**Resumen**

Este código permite descargar y clasificar una serie de fuentes de reglas de ciberseguridad según la matriz MITRE ATT&CK y las técnicas identificadas. Se procesan un total de 20 fuentes de información, incluyendo las reglas ya identificadas en _get_CP_techniques.ipynb_. Los archivos de texto plano (como CSV, YAML, Markdown, entre otros) se descargan desde sus respectivos repositorios en GitHub. Posteriormente, se analiza su contenido en busca de TTP's y se clasifican adecuadamente. Al final, se genera un resumen detallado de las clasificaciones realizadas.


**Estructura**

En el caso de este código no es necesario ningún tipo de input ya que directamente llamaremos a la url de descarga del zip y esta se descargará y descomprimirá temporalmente en la carpeta `Descargas` del usuario. Sin embargo los outputs generados si seguirán la construcción de un árbol de carpetas a la altura de la ubicación del jupyter notebook de la siguiente manera: `outputs\{matrix}\{source}\{id_technique}`
De esta manera se podrá acceder a cada regla por matriz y origen.


**Funciones**

- *create_output_folder ()*
	Argumentos requeridos:
	- *path*: ruta de guardado.
	
	Funcionalidad: encargada creada para crear el directorio facilitado en caso de no existir previamente.


- *find_techniques ()*
	Argumentos requeridos:
	- *texto*: cadena sobre la que se va a iterar para identificar id de técnicas.
	- *techniques_list*: lista de técnicas sobre las que iterar. 

	Funcionalidad: Función encargada de buscar en un texto facilitado los elementos contenidos en la lista de técnicas. Retorna una lista con las técnicas encontradas.


- *find_techniques _in_list ()*
	Argumentos requeridos:
	- *texto*: cadena sobre la que se va a iterar para identificar id de técnicas.
	- *techniques_list*: lista de técnicas sobre las que iterar. 

	Funcionalidad: Función que, dado un str, comprueba en la lista de técnicas facilitada si existe dicha técnica y devuelve la cadena (TTP). En caso de no encontrarse devuelve None. A diferencia de *find_techniques ()* retorna directamente el str (TTP id) encontrado.


- *get_unique_ttps_generated ()*
	Argumentos requeridos:
	- *path*: ruta de directorio.

	Funcionalidad: Función encargada de retornar una lista de id de TTP's únicas generadas. Esta función recibirá una ruta generada previamente en la estructura de carpetas de guardado. 


- *get_dict_ttps_rules_asigned ()*
	Argumentos requeridos:
	- *paths*: lista de rutas de directorios.

	Funcionalidad: Función encargada de retornar un diccionario que identifica el número de reglas localizadas (valores) en cada carpeta generada que identifica a una TTP (keys).


- *sentineldf_to_md ()*
	Argumentos requeridos:
	- *df*: Dataframe de pandas que contiene la información de Sentinel.
	- *path_save*: Ruta de guardado de los ficheros generados.
	- *techniques_list*: lista de técnicas sobre las que iterar. 
	
	Funcionalidad: Función creada para guardar las reglas de detección Sentinel obtenidas a partir del output de get_ttps_from_CP_files.ipynb en formato markdown. Se trata de forma específica por el contenido a guardar en el .md. Retorna el número de reglas (archivos) creadas.


- *splunkdf_to_md ()*
	Argumentos requeridos:
	- *df*: Dataframe de pandas que contiene la información de Sentinel.
	- *path_save*: Ruta de guardado de los ficheros generados.
	- *techniques_list*: lista de técnicas sobre las que iterar. 
	
	Funcionalidad: Función creada para guardar las reglas de detección Splunk obtenidas a partir del output de get_ttps_from_CP_files.ipynb en formato markdown. Se trata de forma específica por el contenido a guardar en el .md. Retorna el número de reglas (archivos) creadas.


- *qradardf_to_md ()*
	Argumentos requeridos:
	- *df*: Dataframe de pandas que contiene la información de Qradar.
	- *path_save*: Ruta de guardado de los ficheros generados.
	- *techniques_list*: lista de técnicas sobre las que iterar. 
	
	Funcionalidad: Función creada para guardar las reglas de detección Splunk obtenidas a partir del output de get_ttps_from_CP_files.ipynb en formato markdown. Se trata de forma específica por el contenido a guardar en el .md. Retorna el número de reglas (archivos) creadas.


- *download_unzip ()*
	Argumentos requeridos:
	- *url_zip*: url de descarga del fichero zip.
	- *temp_path*: ruta donde temporalmente se descargará y descomprimirá el fichero.
	
	Funcionalidad: Función encargada de descargar y descomprimir el fichero zip a partir de la url facilitada.


- *download_folder_unzip ()*
	Argumentos requeridos:
	- *url_zip*: url de descarga del fichero zip.
	-  *folder*: nombre de la carpeta contenida en el zip a descomprimir.
	- *temp_path*: ruta donde temporalmente se descargará y descomprimirá el fichero.
	
	Funcionalidad: Función encargada de descargar y descomprimir una carpeta del fichero zip a partir de la url facilitada. A diferencia de download_unzip () se descomprime únicamente una carpeta contenida en el zip.


- *get_list_of_files_sub ()*
	Argumentos requeridos:
	- *dir_name*: path facilitada para obtener la lista de archivos.
	
	Funcionalidad: Función encargada de retornar una lista de archivos ubicados en la ruta facilitada así como en los subdirectorios disponibles.


- *delete_folder ()*
	Argumentos requeridos:
	- *path*: path facilitada para borrar.
	
	Funcionalidad: Función encargada de borrar la carpeta y los archivos ubicados en una ruta facilitada.


- *get_data_from_branch ()*
	Argumentos requeridos:
	- *matrix*: Cadena que se define como parámetro y solo puede obtener como valores: enterprise, ics o mobile.
	
	Funcionalidad: Se encarga de peticionar a la url de GitHub donde está publicada la última versión MITRE de la información en formato stix2. Retorna el objeto que contiene toda la información de MITRE.


- *get_list_techniques_from_stix2 ()*
	Argumentos requeridos:
	- *src*: Se trata de un objeto stix2 que se genera por la función get_data_from_branch().
	- 
	Argumentos opcionales:
	- *include*: Por defecto 'both', para retornar técnicas y subtécnicas. Otras opciones: techniques, subtechniques.

	Funcionalidad: Función encargada de la obtención de la lista de técnicas de los datos facilitados. Por defecto se retornan tanto técnicas como subtécnicas. Retorna una lista de strings con el id correspondiente a cada técnica.


- *clean_names ()*
	Argumentos requeridos:
	- *string*: texto facilitado para limpiar.
	
	Funcionalidad: Función encargada de suprimir caracteres que puedan generar errores en el guardado y/o lectura así como la supresión de caracteres blancos antes o después de la cadena.


- *copy_file_to_path ()*
	Argumentos requeridos:
	- *item_path*: url del archivo (regla de detección) a copiar.
	- *save_path*: url de guardado.
	- *ttp_name*: nombre de la carpeta intermedia asignada por la TTP correspondiente.
	
	Funcionalidad: Función encargada de copiar un fichero de una ubicación a otra dada, construyendo una subcarpeta intermedia con la TTP facilitada.


- *NotTTPatTags ()*
	Funcionalidad: Clase creada para gestionar el mensaje de error en caso de darse.


- *get_unique_items ()*
	Argumentos requeridos:
	- *list_paths*: lista de rutas.
	
	Funcionalidad: Función que, dada una lista de cadenas (rutas), devuelve la misma lista pero de elementos únicos.


- *yaml_classifier_techniques ()*
	Argumentos requeridos:
	- *files_list*: url del archivo (regla de detección) a revisar y clasificar.
	- *techniques_list*: url de guardado.
	- *save_path*: nombre de la carpeta intermedia asignada por la TTP correspondiente.
	
	Funcionalidad: Función encargada de identificar TTP's en el contenido de las reglas de detección almacenadas como ficheros .yaml así como su posterior clasificación y guardado. En el proceso de búsqueda de las TTP's se definen diferentes condiciones para buscar en tanto en el contenido como en el propio nombre del archivo y en caso de no identificar id de TTP se asignan a la carpeta T0000. Retorna una lista con las path de las reglas clasificadas por la extensión .yaml o .yml.

- *txt_basename_classifier_techniques ()*
	Argumentos requeridos:
	- *files_list*: lista de paths de archivos (reglas de detección) a revisar y clasificar.
	- *techniques_list*: lista de id's de técnicas contra las que chequear.
	- *save_path*: nombre de la carpeta intermedia asignada por la TTP correspondiente.
	
	Funcionalidad: Función encargada de identificar TTP's en el nombre del archivo de las reglas de detección almacenadas como ficheros .txt así como su posterior clasificación y guardado. En el proceso de búsqueda de las TTP's se definen diferentes condiciones para realizar la búsqueda en el nombre del archivo y en caso de no identificar id de TTP se asignan a la carpeta T0000. Retorna una lista con las path de las reglas clasificadas por la extensión .txt. Sólo se busca en el nombre del archivo ya que las reglas descargadas vienen formateadas con la asignación del id de TTP en el mismo (Netevert).


- *toml_find_techniques ()*
	Argumentos requeridos:
	- *toml_dict*: diccionario cuyo origen es un objeto .toml.
	- *techniques_list*: lista de id's de técnicas contra las que chequear.
	
	Funcionalidad: Función encargada de identificar TTP's en un diccionario cuyo origen es la lectura del un fichero .toml. Esta función es llamada por toml_classifier_techniques_elastic() en el proceso de identificación de TTP's y clasificación de archivos .toml. La función tiene como característica la capacidad de buscar en toda la profundidad de un diccionario del que no se conoce su esquema. Retorna una lista con los id de técnicas clasificadas.


- *toml_classifier_techniques_elastic ()*
	Argumentos requeridos:
	- *files_list*: lista de paths de archivos (reglas de detección) a revisar y clasificar.
	- *techniques_list*: lista de id's de técnicas contra las que chequear.
	- *save_path*: nombre de la carpeta intermedia asignada por la TTP correspondiente.
	
    Funcionalidad: Función encargada de identificar TTP's en el contenido de las reglas de detección almacenadas como ficheros .toml así como su posterior clasificación y guardado. Se llama a la función toml_find_techniques() que retornará la lista de técnicas identificadas en el diccionario cuyo origen es el objeto toml.load(file). Tras esto el archivo es copiado a la carpeta correspondiente con la construcción de la carpeta que identifica la/s TTP correspondiente, siendo T0000 la asignada para aquellos archivos que no han sido identificados. Retorna una lista con las path de las reglas clasificadas.


- *md_classifier_techniques ()*
	Argumentos requeridos:
	- *files_list*: lista de paths de archivos (reglas de detección) a revisar y clasificar.
	- *techniques_list*: lista de id's de técnicas contra las que chequear.
	- *save_path*: nombre de la carpeta intermedia asignada por la TTP correspondiente.
	
      Funcionalidad: Función encargada de identificar TTP's tanto en el nombre del archivo como en el contenido de las reglas de detección almacenadas como ficheros ..md o .markdown así como su posterior clasificación y guardado. En una primera instancia se pasa como parámetro de búsqueda el contenido del archivo almacenado como cadena y en caso de no identificarse nada se pasa como parámetro el nombre del propio archivo. Tras esto el archivo es copiado a la carpeta correspondiente con la construcción de la carpeta que identifica la/s TTP correspondiente, siendo T0000 la asignada para aquellos archivos que no han sido identificados. Retorna una lista con las path de las reglas clasificadas por la extensión .md o .markdown.


- *yara_classifier_techniques ()*
	Argumentos requeridos:
	- *files_list*: lista de paths de archivos (reglas de detección) a revisar y clasificar.
	- *techniques_list*: lista de id's de técnicas contra las que chequear.
	- *save_path*: nombre de la carpeta intermedia asignada por la TTP correspondiente.
	
      Funcionalidad: Función encargada de identificar TTP's tanto en el nombre del archivo como en el contenido de las reglas de detección almacenadas como ficheros .yara o .yar así como su posterior clasificación y guardado. En una primera instancia se pasa como parámetro de búsqueda el contenido del archivo almacenado como cadena y en caso de no identificarse nada se pasa como parámetro el nombre del propio archivo. Tras esto el archivo es copiado a la carpeta correspondiente con la construcción de la carpeta que identifica la TTP correspondiente, siendo T0000 la asignada para aquellos archivos que no han sido identificados. Retorna una lista con las path de las reglas clasificadas.


- *yaml_classifier_techniques_sigma ()*
	Argumentos requeridos:
	- *files_list*: lista de paths de archivos (reglas de detección) a revisar y clasificar.
	- *techniques_list*: lista de id's de técnicas contra las que chequear.
	- *save_path*: nombre de la carpeta intermedia asignada por la TTP correspondiente.
	
      Funcionalidad: Función encargada de identificar TTP's tanto en el nombre del archivo como en el contenido de las reglas de detección almacenadas como ficheros .yaml o .yml así como su posterior clasificación y guardado.
      A diferencia de yaml_classifier_techniques(), en esta función se evalúan determinadas etiquetas en las que suele indicarse la técnica para la fuente Sigma HQ. Entre estas etiquetas están `tags`,`mitreattack`y `description`. Se ha construido un árbol que evalúa cada revisión asignando un proceso u otro en función de los elementos identificados (o su ausencia). Tras esto el archivo es copiado a la carpeta correspondiente con la construcción de la carpeta que identifica la/s TTP correspondiente, siendo T0000 la asignada para aquellos archivos que no han sido identificados. Retorna una lista con las path de las reglas clasificadas por la extensión .yaml o .yml.

- *sigma7_classifier_techniques ()*
	Argumentos requeridos:
	- *path_source*: lista de paths de archivos (reglas de detección) a revisar y clasificar.
	- *path_destiny*: nombre de la ruta principal de guardado.
	- *techniques_list*: lista de id's de técnicas contra las que chequear.
	
      Funcionalidad: Función singular para las reglas de detección Sigma HQ 7. En este caso los ficheros descargados vienen clasificados por TTP, pero se debe identificar la TTP asignada en el título del archivo con la lista de TTP's facilitada por la matriz MITRE en cuestión. Adicionalmente se filtran los archivos a copiar debido a la presencia de archivos .png adicionales. Todos aquellos archivos cuya TTP no haya sido podido mapear con el listado pasarán a guardarse en T0000.

- *get_resume_df ()*
	Argumentos requeridos:
	- *matrix_output_path*: ruta de guardado de los ficheros para la matriz
	- *matrix*: matriz MITRE que está evaluándose.

      Funcionalidad: Función que construye 3 dataframes de pandas con el resumen de los resultados: El primer df contiene la lista de reglas por TTP asignada y fuente de la regla. El segundo df agrega y cuenta el numero de reglas asignadas por TTP. Y el tercer df contiene la misma información que el primero excluyendo todas aquellas reglas asignadas a la carpeta T0000.  