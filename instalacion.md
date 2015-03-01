# Manuales de instalación #

En este manual, queremos indicar los pasos seguidos en la instalación de la aplicación cliente en un servidor.


## Manual de instalación cliente web  ##

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


Solo nos queda configurar los sites de apache para apuntar a nuestra página, y el fichero /etc/hosts en Linux, y C:\Windows\System32\drivers\etc\hosts en Windows para configurar el servidor de nombres. Combiar por el directorio donde se ubica en el directorio web de nuestra aplicación.

    [cli]
    #VirtualHost de Apache
    <VirtualHost *:80>
        ServerAdmin admin@admin.com
        DocumentRoot "c:\wamp\www\directorioInstalacion\web"
        ServerName local.loborojo
        ServerAlias local.loborojo
        ErrorLog "logs/dummy-host.example.com-error.log"
        CustomLog "logs/dummy-host.example.com-access.log" common
    </VirtualHost>
    
    #/etc/hots
    127.0.0.1 local.loborojo
    
Una vez reiniciados los servicios de apache, la web se encontrará en las siguientes urls

[URL producción](http://local.loborojo/app.php) 

[URL desarrollo](http://local.loborojo/app_dev.php)


## Servicios SOAP y REST  ##

Una vez instalado el servicio MySQL, instalamos la base de datos MySQL db_loborojo.sql que se encuentra en el directorio Servicios\Base de datos mySQL

Se han seguido las instrucciones de las trasparencias de la asignatura y las versiones indicadas, luego solo será necesario iniciar los servicios en el servidor de Apache Tomcat.

## Demonio ThingSpeak  ##

Para iniciar el simulador de señales, hacemos click derecho sobre "com.thingspeak.test-> Main.java" Run As -> Java application.

La URL del thingSpeak asociado es la siguiente [thingSpeak](https://thingspeak.com/channels/23808)

## Manual de uso ##

Hay que recordar que los servicios SOAP deben estár en funcionamiento para que la web funcione correctamente.

Una vez accedemos a la url de la landing page, hacemos click sobre el servicio que actualmente está en funcionamiento, es decir, Control de Amenazas -> Monitorización Sonora.

Se mostrarán los registros cargados inicialmente en la base de datos. Además, se disponen varias llamadas bajo el mapa de google para ejecutar las distintas operaciones de los servicios.

