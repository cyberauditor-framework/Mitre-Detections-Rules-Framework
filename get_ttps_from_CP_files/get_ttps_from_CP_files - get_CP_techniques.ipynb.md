---
Status: Ok
Refactored: true
Requirements: csv, os, requests, pandas, stix2
Last modification: 2024-07-01
1st deploy: 2024-05-16
Author: Juan Enrique López Marcos (CP)
Functions: get_data_from_branch, get_list_techniques_from_stix2, find_techniques, get_techniques_from_UCM_Sentinel, get_techniques_from_UCM_Splunk, get_techniques_from_UCM_Qradar
---
**Resumen**

Este Jupyter Notebook es el encargado de obtener las técnicas a partir de los catálogos de reglas de detección facilitados (Sentine, Splunk y Qradar) como archivos .xlsx. 
El proceso es relativamente sencillo, se cargan las hojas de los libros en dataframes de pandas. Se formatea y filtra la información para posteriormente buscar y machear referencias a las técnicas facilitadas.

Los outputs serán archivos csv clasificados por matriz (Enterprise, ICS, Mobile) y origen (Sentinel, Splunk, Qradar) desagregados por técnica y toda la información disponible de los ficheros de origen. También se genera un csv agregado de los 3 orígenes.
Formato del nombre de los archivos finales: `[MITRE-{matrix}]_techniques_UCMCatalog2024_{source}_withquery.csv`

Con la última modificación, antes de ejecutarse este proceso se permite seleccionar la matriz MITRE de técnicas (contra la que buscar). También asociado a la ultima modificación, se ha reemplazado el método con el que se seleccionaban las técnicas. Anteriormente se utilizaba la librería attackcti a modo de intermediario y actualmente se peticiona directamente a la API de MITRE alojada en [GitHub](https://github.com/mitre-attack/attack-stix-data)


**Estructura**

Necesario: Disponer de una carpeta llamada *inputs* a la altura del jupyter notebook donde se encuentre una subcarpeta *UCM Catalog 2024* los archivos .xlsx: `UCM Catalog 2024 [Sentinel]`, `UCM Catalog 2024 [Splunk]` y `UCM Catalog 2024 [Qradar]`
Tras la ejecución se genera una carpeta *outputs* y una o varias subcarpetas relativas a la matriz MITRE (en función de las ejecuciones realizadas) que contendrán los ficheros resultantes.

**Funciones**

Enumeramos las funciones creadas para este código junto con una breve descripción de la utilidad.

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


- *find_techniques ()*
	Argumentos requeridos:
	- *texto*: cadena sobre la que se va a iterar para identificar id de técnicas.
	- *techniques_list*: lista de técnicas sobre las que iterar. 

	Funcionalidad: Función encargada de buscar en un texto facilitado los elementos contenidos en la lista de técnicas. Retorna una lista con las técnicas encontradas.

- *get_techniques_from_UCM_Sentinel ()*
	Argumentos requeridos:
	- *ucm_sentinel_df*: dataframe que contiene la información de `UCM Catalog 2024 [Sentinel]`
	- *techniques_list*: Lista de técnicas sobre las que iterar.
	- *mitre_matrix*: Matriz de MITRE sobre la que se está trabajando.

	Funcionalidad: Función encargada de formatear el contenido del dataframe que contiene la información de sentinel, buscar los id de técnicas en el contenido y retornar un dataframe final con la información final.

- *get_techniques_from_UCM_Splunk ()*
	Argumentos requeridos:
	- *ucm_splunk_df*: dataframe que contiene la información de `UCM Catalog 2024 [Splunk]`
	- *techniques_list*: Lista de técnicas sobre las que iterar.
	- *mitre_matrix*: Matriz de MITRE sobre la que se está trabajando.

	Funcionalidad: Función encargada de formatear el contenido del dataframe que contiene la información de splunk, buscar los id de técnicas en el contenido y retornar un dataframe final con la información final.


- *get_techniques_from_UCM_Qradar ()*
	Argumentos requeridos:
	- *ucm_qradar_baseline_df*: dataframe que contiene la información de `UCM Catalog 2024 [Splunk] - hoja baseline`
	- *ucm_qradar_ws_df*: dataframe que contiene la información de `UCM Catalog 2024 [Splunk] - hoja ws`
	- *ucm_qradar_atomics_df*: dataframe que contiene la información de `UCM Catalog 2024 [Splunk] - hoja atomics
	- *ucm_qradar_linux_df*: dataframe que contiene la información de `UCM Catalog 2024 [Splunk] - hoja linux
	- *ucm_qradar_fw_df*: dataframe que contiene la información de `UCM Catalog 2024 [Splunk] - hoja fw`
	- *ucm_qradar_aws_df*: dataframe que contiene la información de `UCM Catalog 2024 [Splunk] - hoja aws`
	- *techniques_list*: Lista de técnicas sobre las que iterar.
	- *mitre_matrix*: Matriz de MITRE sobre la que se está trabajando.

	Funcionalidad: Función encargada de formatear el contenido del dataframe que contiene la información de qradar, que en este caso está conformado por varios dataframes al estar separada en hojas distintas del libro xlsx. Al igual que las anteriores funciones, busca los id de técnicas en el contenido y retornar un dataframe final con la información final.

