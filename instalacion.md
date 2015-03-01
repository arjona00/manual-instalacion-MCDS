# Manuales de instalación cliente Web #

En este manual, queremos indicar los pasos seguidos en la instalación de la aplicación cliente en un servidor.


## Manual de instalación  ##

En nuestro sistema, debemos tener instalado php5, mysql y apache2. 

Recomiendo el uso del instalador WAMP si no se dispone de estos elementos instalados.

[Wamp](http://www.wampserver.com/en/)

En nuestro caso hemos utilizado las siguientes versiones:

	mySQL 5.5.24
	php 5.4.3
	apache 2.4.2

Para la instalación de symfony, necesitaremos composer.
Las instrucciones de instalación de composer podemos encontrarlas aquí:

[Composer](http://getcomposer.org/)

Además necesitaremos tener cargadas las extensiones php5-xsl y php5-intl.

	[cli]
	sudo apt-get install php5-xsl
	sudo apt-get install php5-intl


Primero debemos crear nuestro proyecto en un directorio del servidor. Extraemos el contenido de clienteWebSymfony2 en dicho directorio.

Una vez situados en el directorio creado, debemos instalar nuestra aplicación. Para ello ejecutamos:

	[cli]
	~/composer.phar install

Composer suele instalarse en el directorio home de su usuario logado.

Cuando terminen de descargarse los vendors indicados en el fichero composer.json de nuestra aplicación, nos pedirá los datos de la configuración inicial, que se guardará en el fichero app/config/parameters.yml. Esta configuración incluye el motor de la base de datos, el nombre de la base de de datos, su usuario de acceso y los datos de correo para los formularios, que en nuestro caso no se utilizarán. Aquí indicamos los datos de configuración usuales.

	[yaml]
    database_driver: pdo_mysql
    database_host: 127.0.0.1
    database_port: null
    database_name: db_loboRojo
    database_user: root
    database_password: xxxxx
    mailer_transport: smtp
    mailer_host: 127.0.0.1
    mailer_user: null
    mailer_password: null
    locale: en
    secret: ThisTokenIsNotSoSecretChangeIt


Si previamente no hemos creado la base de datos a través de phpmyadmin o consola, podemos hacerlo ahora usando doctrine. En una segunda orden, creamos el esquema de la base de datos.

	[cli]
	php app/console doctrine:database:create
	php app/console doctrine:schema:create


Ahora ejecutamos los fixtures, que rellenarán la base de datos con los datos iniciales de prueba.

	[cli]
	php app/console doctrine:fixture:load

Symfony usa dos directorios para la caché y logs, a los cuales tenemos que dar los siguientes permisos:

	[cli]
	rm -rf app/logs/*
	rm -rf app/cache/*
	chmod 777 app/logs
	chmod 777 app/cache

Finalmente, ejecutamos los assets, limpiamos la caché y comprimimos los ficheros de recursos. Esta secuencia deberá ejecutarse cuando se modifiquen los recursos, css, js o imágenes.

	[cli]
	php app/console assets:install
	php app/console cache:clear
	php app/console cache:clear --env=prod
	php app/console assetic:dump


Solo nos queda configurar los sites de apache y el fichero /etc/hosts para apuntar a nuestra página, que por defecto se ubica en el directorio web de nuestra aplicación.


## Manual de usuario ##

Esta aplicación web permite al usuario realizar los cuestionarios de las asignaturas registradas y sus temas. Cada tema podrá incluir varias subcategorías de las que también podrá realizar cuestionarios.

Para ello acceda a la aplicación web a través de su navegador. Seleccione la asignatura de la que desea realizar los tests, del menú Asignaturas o desde la propia página de inicio.

Una vez dentro de la asignatura, puede seleccionar realizar un test de la asignatura completa, de un tema o de una de las categorías disponibles. Pulse sobre su elección.

Para cada uno de los cuestionarios, pueden existir varios niveles de dificultad. Están encuadrados en niveles del 1 a 10, pero también puede realizar un cuestionario de preguntas aleatorias de cualquier nivel.

Una vez hecha su elección, aparecerán en su pantalla un máximo de 10 preguntas. Responda a las preguntas y pulse enviar. Aparecerá el resultado de su test.


## Manual de administrador ##

Como administrador de las asignaturas, tiene la responsabilidad de crearlas, crear temas en las mismas y de cada tema, tantas subcategorías como sean necesarias.
Además se encargará de la importación y exportación de cuestiones.

Para ello deberá acceder a la aplicación web desde su navegador.

En el menú, pulse sobre "Administración".
El sistema le indica que está accediendo a una zona restringida, y le solicita usuario y password.

Tras introducir los datos correctamente, se mostrarán la lista de asignaturas, sus categorías y subcategorías.


### Creación de elementos ###

Para crear una nueva asignatura, solo necesita ir al final de la página y pulsar sobre "Nueva asignatura". En el cuadro de diálogo de edición, introduzca el nombre de la asignatura y pulse sobre la confirmación.

Su asignatura ha sido creada. Pulse sobre el botón de recarga a la derecha de la asignatura creada y podrá ver su asignatura en la lista de asignaturas en orden alfabético. Ahora ya podrá editar sus campos.

Para las categorías y subcategorías el mecanismo es similar. Vaya al final de la lista de temas de una asignatura. Pulse "Nuevo tema". Introduzca el nombre del tema y confirme. Luego pulse el botón de recarga a la derecha del nombre introducido, y verá que su tema se ha creado, y que los campos editables del tema, su título y descripción, ya están disponibles para su edición.


### Edición de elementos ###

La edición se produce directamente a través del formulario de edición de cualquier campo editable seleccionado. Pulse sobre el campo a editar. Se abrirá un cuadro de diálogo, realice su modificación y pulse sobre el botón de confirmación. Sus datos se actualizarán de forma inmediata.


### Eliminación de elementos ###

Para eliminar cualquier elemento pulse sobre el botón eliminar (una x blanca sobre fondo rojo). Recuerde que al eliminar cualquier elemento, elimina todas las estructuras que estén relacionadas con la misma. (P. ejem: Borrar una asignatura, elimina también las categorías, subcategorías, preguntas y respuestas relacionadas con dicha asignatura)

Confirme la eliminación del elemento en el cuadro de diálogo.


### Importación y exportación de cuestiones ###

#### Exportación ####

La exportación puede realizarse sobre cualquier elemento contenedor de preguntas, asignaturas, categorías y subcategorías.

Desde la lista de administración de asignaturas, pulse sobre el botón de edición del elemento seleccionado (flecha > blanca con fondo azul).

Esto nos llevará a la zona de importación y exportación.

Para realizar la exportación, escoja el tipo de formato a exportar y pulse el botón "Exportar".

Opcionalmente puede escoger un nivel determinado para las preguntas a exportar, y un número de preguntas máximo a exportar.


#### Importación ####

La importación se realiza en cualquier subcategoría. Desde la lista de administración de asignaturas, pulse sobre el botón de edición del elemento seleccionado (flecha > blanca con fondo azul).

Esto nos llevará a la zona de importación y exportación.

Pulse sobre importar.

En esta nueva página, seleccione el tipo de formato a importar. Luego pulse sobre "Seleccionar fichero".

Nos abrirá un cuadro de diálogo para seleccionar nuestro fichero a importar.

Una vez subido, pulse sobre "Procesar fichero". Si el fichero cumple las condiciones de tipo de fichero y formato, el fichero se procesará y las preguntas serán importadas en la subcategoría elegida.