---

copyright:
  years: 2017
lastupdated: "2017-11-02"

---
{:new_window: target="_blank"}  
{:shortdesc: .shortdesc}  
{:screen: .screen}  
{:codeblock: .codeblock}  
{:pre: .pre}  

# {{site.data.keyword.dev_cli_short}}
{: #developercli}	

El {{site.data.keyword.dev_cli_long}} proporciona un enfoque basado en mandatos extensibles para crear, desarrollar y desplegar un proyecto web con el plugin `dev`. Ideal para desarrolladores que prefieren utilizar el control de línea de mandatos para desarrollar aplicaciones de microservicios completas.

{: shortdesc}

El {{site.data.keyword.dev_cli_notm}} utiliza dos contenedores para facilitar la creación y la realización de pruebas de su aplicación. El primero es el contenedor de herramientas que contiene las utilidades necesarias para crear y probar la aplicación. El Dockerfile para este contenedor está definido por el parámetro [`dockerfile-tools`](#command-parameters). Puede considerarlo como un contenedor de desarrollo, ya que contiene las herramientas que suelen utilizarse para el desarrollo de un tiempo de ejecución determinado.

El segundo contenedor es el contenedor de ejecución. El formato de este contenedor es adecuado para desplegarlo y utilizarlo en, por ejemplo, {{site.data.keyword.Bluemix}}. Como resultado, se define un punto de entrada que inicia su aplicación. Al seleccionar ejecutar la aplicación a través de {{site.data.keyword.dev_cli_short}}, utiliza este contenedor. El Dockerfile para este contenedor está definido por el parámetro [`dockerfile-run`](#run-parameters).


## Adición de {{site.data.keyword.dev_cli_notm}}
{: #add-cli}


### Requisitos previos
{: #prereq}

Debe obtener algunos requisitos previos para explorar completamente y utilizar correctamente el {{site.data.keyword.dev_cli_short}}, ya que es muy ampliable para utilizar tecnologías complementarias.

<!--1. Install the [Cloud Foundry CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli#getting-started "External link icon").-->

1. Instale [{{site.data.keyword.Bluemix}} CLI ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://clis.ng.bluemix.net/ui/home.html "Icono de enlace externo").

2. Obtenga un [ID de {{site.data.keyword.Bluemix_notm}}](https://www.bluemix.net).

3. Si tiene intención de ejecutar y depurar aplicaciones a nivel local, deberá instalar también [Docker ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.docker.com/get-docker "Icono de enlace externo").


### Antes de empezar
{: #before-install}

1. Conecte con un punto final de API de la región de [{{site.data.keyword.Bluemix_notm}}](/docs/overview/whatisbluemix.html#ov_intro_reg). Por ejemplo, escriba el siguiente mandato para conectar con la región de {{site.data.keyword.Bluemix_notm}} EE.UU. Sur:

	```
	bx api https://api.ng.bluemix.net
	```
	{: codeblock}
	
2. Inicie sesión en {{site.data.keyword.Bluemix_notm}} especificando su ID de IBM y contraseña:

	```
	bx login
	```
	{: codeblock}
	
	**Nota:** si se rechazan sus credenciales, puede que esté utilizando un ID federado. Siga estos pasos para autenticarse mediante un ID federado.
	
	<!-- 
	POINT TO IBM Cloud CLI LOG IN DOCUMENTATION !!!
	
	This link does not work in production yet --> 
	
	1. Inicie sesión en [{{site.data.keyword.iamshort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.bluemix.net/iam/#/apikeys "Icono de enlace externo"){: new_window}.
	2. Seleccione **Crear clave de API**.
		* Especifique un nombre y descripción para la clave de API
	3. Descargue su clave de API.
	4. Abra el archivo y copie el valor del campo `apiKey`.
	5. Inicie sesión utilizando el siguiente mandato:
	 
		```
		bx login --apikey <value>
		```
		{: codeblock}


### Instalación
{: #installation}

1. Instale el [{{site.data.keyword.dev_cli_short}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli/reference/bluemix_cli/index.html#install_plug-in "Icono de enlace externo"){: new_window} ejecutando el siguiente mandato:
 
	```
	bx plugin install dev
	```
	{: codeblock}

2. 	Valide la correcta instalación del plugin ejecutando el siguiente mandato:  
 
	```
	bx dev
	```
	{: codeblock}


## Mandatos
{: #commands}

Utilice los siguientes mandatos para crear un proyecto, desplegarlo, depurarlo y probarlo.

### Build
{: #build}

Puede crear su aplicación mediante el mandato `build`. El elemento de configuración `build-cmd-run` se utiliza para crear la aplicación. Los mandatos `test`, `debug` y `run` esperan encontrar un proyecto compilado, de modo que debe ejecutar de antemano una operación de compilación al menos una vez.

Ejecute el siguiente mandato en el directorio del proyecto actual para crear la aplicación:  

```
bx dev build
```
{: codeblock}


[Parámetros del mandato de compilación](#command-parameters)


### Code
{: #code}

Utilice el mandato `code` para descargar el código de aplicación después del despliegue, para que pueda revisarlo de forma local o realizar cambios.

Ejecute el siguiente mandato para descargar el código de un proyecto específico.

```
bx dev code <projectName>
```
{: codeblock}


### Create
{: #create}

Cree un proyecto solicitando toda la información, incluido idioma, nombre del proyecto y tipo de patrón de app. El proyecto se crea en el directorio actual. 

Para crear un proyecto en el directorio del proyecto actual y asociarle servicios, ejecute el siguiente mandato:

```
bx dev create
```
{: codeblock}


### Debug
{: #debug}

Puede depurar la aplicación a través del mandato `debug`. Se debe completar primero una compilación sobre el proyecto utilizando el mandato build. Al invocar el mandato `debug`, se inicia un contenedor que proporciona un puerto o puertos de depuración tal y como se define en el valor `container-port-map-debug`. Conecte su herramienta de depuración preferida al puerto o puertos y podrá depurar su aplicación de modo normal.

**Limitación**: los proyectos Swift no están disponibles para depuración.

En primer lugar, compile su proyecto:

```
bx dev build
```
{: codeblock}

Ejecute el siguiente mandato en el directorio del proyecto actual para depurar la aplicación:

```
bx dev debug
```
{: codeblock}	

Para salir de la sesión de depuración, utilice `CTRL-C`.


#### Parámetros del mandato de depuración
{: #debug-parameters}

Los siguientes parámetros son exclusivos del mandato `debug` y ayudan a depurar una aplicación.

##### `container-port-map-debug`
{: #port-map-debug}

* Correlaciones de puertos para el puerto de depuración. El primer valor es el puerto que se utilizará en el sistema operativo del host, el segundo es el puerto del contenedor [host-port:container-port].
* Uso: `bx dev debug --container-port-map-debug [7777:7777]`

##### `build-cmd-debug`
{: #build-cmd-debug}

* Parámetro utilizado para generar código para DEBUG.
* Uso: `bx dev debug --build-cmd-debug build.command.sh`

##### `debug-cmd`
{: #debug-cmd}

* Parámetro que se utiliza para especificar un mandato para invocar debug en el contenedor de herramientas. Utilice este parámetro si `build-cmd-debug` inicia la aplicación en depuración.
* Uso: `bx dev debug --debug-cmd /the/debug/command`

#### Depuración de aplicaciones locales:
{: #local-app-dev}

Para obtener más información sobre la depuración de aplicaciones locales, consulte [Depuración de aplicaciones locales](docs/cloudnative/dev_cli_local_debug.html#local-debug).


### Delete
{: #delete}

Utilice el mandato `delete` para eliminar proyectos de su espacio {{site.data.keyword.Bluemix}}. Puede ejecutar el mandato sin parámetros para ver una lista de los proyectos que se pueden suprimir. El código y los directorios del proyecto no se eliminan del espacio de disco local.

Ejecute el siguiente mandato para suprimir su proyecto de {{site.data.keyword.Bluemix}}:

```
bx dev delete <projectName>
```
{: codeblock}
 

**Nota:** los servicios de {{site.data.keyword.Bluemix}} **no** se eliminan.


### Deploy
{: #deploy}

Puede enviar por push una aplicación a {{site.data.keyword.Bluemix}} mediante el mandato `deploy` cuando hay un archivo `manifest.yml` presente en el directorio raíz de su proyecto.

Ejecute el siguiente mandato en el directorio del proyecto actual para crear la aplicación:  

```
bx dev build
```
{: codeblock}

Ejecute el siguiente mandato para desplegar su proyecto en {{site.data.keyword.Bluemix}}:

```
bx dev deploy
```
{: codeblock}


### Help
{: #help}

De forma predeterminada, si no se pasa ninguna acción ni argumento, o si se proporciona la acción 'help', este mandato muestra un texto "Help" general. La ayuda general mostrada incluye una descripción de los argumentos base, así como una lista de las acciones disponibles.  

Ejecute el siguiente mandato para visualizar la información de ayuda general:

```
bx dev help
```
{: codeblock}


### List
{: #list}

Puede listar todos sus proyectos {{site.data.keyword.Bluemix_notm}} en un espacio.

Ejecute el siguiente mandato para listar sus proyectos:

```
bx dev list
```
{: codeblock}


<!--
### Editing a project
{: #edit}

You can edit a project, such as changing the name, pattern or starter type, or adding services to your project. Run the following command:

```
bx dev edit
```
{: codeblock}
-->


### Run
{: #run}

Puede ejecutar la aplicación mediante el mandato `run`. Se debe completar primero una compilación sobre el proyecto utilizando el mandato `build`. Al invocar el mandato run, se inicia el contenedor de ejecución y expone los puertos tal y como se define en el parámetro `container-port-map`. El parámetro `run-cmd` se puede utilizar para invocar la aplicación si el contenedor de ejecución Dockerfile no contiene un punto de entrada para completar este paso. 

En primer lugar, compile su proyecto:

```
bx dev build
```
{: codeblock}

Ejecute el siguiente mandato en el directorio del proyecto actual para iniciar la aplicación:

```
bx dev run
```
{: codeblock}

Para salir de la sesión, utilice `CTRL-C`.


#### Parámetros de ejecución
{: #run-parameters}

Los siguientes parámetros son exclusivos del mandato `run` y ayudan a gestionar la aplicación dentro del contenedor de ejecución.

##### `container-name-run`
{: #container-name-run}
	
* Nombre del contenedor para el contenedor de ejecución.
* Uso: `bx dev run --container-name-run [<projectName>]`

##### `container-path-run`
{: #container-path-run}

* Ubicación en el contenedor para compartir en ejecución.
* Uso: `bx dev run --container-path-run [/path/to/app]`

##### `host-path-run`
{: #host-path-run}

* Ubicación en el sistema host para compartir en el contenedor en ejecución.
* Uso: `bx dev run --host-path-run [/path/to/app/bin]`

##### `dockerfile-run`
{: #dockerfile-run}

* Dockerfile para el contenedor de ejecución.
* Uso: `bx dev run --dockerfile-run [/path/to/Dockerfile.yml]`

##### `image-name-run`
{: #image-name-run}

* Imagen para crear desde `dockerfile-run`.
* Uso: `bx dev run --image-name-run [/path/to/image-name]`

##### `run-cmd`
{: #run-cmd}

* Parámetro utilizado para ejecutar código en el contenedor de ejecución. Utilice este parámetro si su imagen inicia la aplicación.
* Uso: `bx dev run --run-cmd [/the/run/command]`
	
### Status
{: #status}

Puede consultar el estado de los contenedores que utiliza {{site.data.keyword.dev_cli_short}} tal y como se definen en `container-name-run` y `container-name-tools`. 

Ejecute el siguiente mandato en el directorio del proyecto actual para comprobar el estado de los contenedores:

```
bx dev status
```
{: codeblock}


[Parámetros del mandato de estado](#command-parameters)


### Stop
{: #stop}

Puede detener sus contenedores mediante el mandato `stop`.

Para detener las herramientas y ejecutar los contenedores tal como se define en el archivo `cli-config.yml`, ejecute:

```
bx dev stop
```
{: codeblock}

Para detener un contenedor que no está definido en el archivo `cli-config.yml`, puede especificar un parámetro de línea de mandatos adicional para sustituirlo. Para el contenedor de herramientas, utilice el parámetro [`--container-name-tools`](#container-name-tools) y, para el contenedor de ejecución, utilice el parámetro [`--container-name-run`](#container-name-run). Si no se especifica ningún contenedor en el archivo `cli-config.yml` o en la línea de mandatos, el mandato stop vuelve.


### Test
{: #test}

Puede probar la aplicación a través del mandato `test`. Se debe completar primero una compilación sobre el proyecto utilizando el mandato `build`. A continuación, el contenedor de herramientas se utiliza para invocar `test-cmd` para la aplicación.

En primer lugar, compile su proyecto:

```
bx dev build
```
{: codeblock}

Ejecute el siguiente mandato para probar la aplicación: 

```
bx dev test
```
{: codeblock}


[Parámetros del mandato de prueba](#command-parameters)


## Parámetros para crear, depurar, ejecutar y probar
{: #command-parameters}

Los siguientes parámetros se pueden utilizar con los mandatos `build|debug|run|test` o se puede actualizar directamente el archivo `cli-config.yml` del proyecto. Dispone de parámetros adicionales para los mandatos [`debug`](#debug-parameters) y [`run`](#run-parameters).

**Nota**: los parámetros de mandatos especificados en la línea de mandatos tienen prioridad sobre la configuración `cli-config.yml`.

### `container-name-tools`  
{: #container-name-tools}

* Nombre del contenedor para el contenedor de herramientas.
* Uso: `bx dev <build|debug|run|stop|test> --container-name-tools [<projectName>]`

### `host-path-tools`
{: #host-path-tools}

* Ubicación en el host para compartir para la compilación, depuración, pruebas.
* Uso: `bx dev <build|debug|run|test> --host-path-tools [/path/to/build/tools]`

### `container-path-tools`
{: #container-path-tools}

* Ubicación en el contenedor para compartir para la compilación, depuración, pruebas.
* Uso: `bx dev <build|debug|run|test> --container-path-tools [/path/for/build]`

### `container-port-map`
{: #container-port-map}

* Correlaciones de puertos para el contenedor. El primer valor es el puerto que se utilizará en el sistema operativo del host, el segundo es el puerto del contenedor [host-port:container-port].
* Uso: `bx dev <build|debug|run|test> --container-port-map [8090:8090,9090,9090]`

### `dockerfile-tools`
{: #dockerfile-tools}

* Dockerfile para el contenedor de herramientas.
* Uso: `bx dev <build|debug|run|test> --dockerfile-tools [path/to/dockerfile]`

### `image-name-tools`
{: #image-name-tools}

* Imagen para crear desde `dockerfile-tools`.
* Uso: `bx dev <build|debug|run|test> --image-name-tools [path/to/image-name]`

### `build-cmd-run`
{: #build-cmd-run}

* Parámetro que se utiliza para especificar un mandato para generar código para todos los usos menos DEBUG.
* Uso: `bx dev <build|debug|run|test> --build-cmd-run [some.build.command]`

### `test-cmd`
{: #test-cmd}

* Parámetro que se utiliza para especificar un mandato para probar código en el contenedor de herramientas.
* Uso: `bx dev <build|debug|run|test> --test-cmd [/the/test/command]`

### `trace`
{: #trace}

* Utilice este parámetro para proporcionar una salida detallada.
* Uso: `bx dev <build|debug|run|test> --trace`


