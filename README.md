# Module-TutorialE

En este tutorial vamos a explicar como poder cear modulos para luego usarlos en la plataforma Odoo.

Para empezar necesitaremos una serie de programas:

* Virtual Box: en este programa crearemos una maquina virtual capaz de lanzar un servidor Odoo completamente funcional para poder ir subiendo nuestro trabajo y luego acceder a el desde la plataforma Odoo en un navegador web.
* Netbeans: aqui crearemos la estructura de carpetas y archivos necesarios para que nuestro tema sea funcional y reconocido por la plataforma Odoo.
* Putty: usaremos este progama para hacer conexiones ssh al servidor creado con la maquina virtual y  poder explorar mediante consola si el trabajo que vamos subiendo al servidor esta realmente subido, asi como, lanzar el servidor Odoo.

Ya hemos visto anteriormente como crear y configurar la maquina virtual de cero para luego implementar Odoo y asi poder usarlo. Esto puede ser tedioso y dar errores, existen maquinas virtuales prehechas de todo tipo, tambien las hay para Odoo. Usar una turnkey(asi se llaman estas maquinas preconfiguradas) es el camino mas rapido aunque en algún caso puede no ajustarse a nuestras necesidades. En este ccaso usamos una turnkey postgresql.

Vamos con los pasos a seguir:

Lanzar la maquina virtual para obtener las direcciones necesarias para diferentes tipos de conexion.

![**](/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/primera.png)

Ahora vamos a abrir con PUTTY dos consolas; no son necesarias dos pero a la hora de trabajar es mucho mas sencillo y util, usaremos una para lanzar odoo y otra para navegar por la estructura de carpetas. De no hacerlo asi tendriamos que constantemente ir cerrando odoo y volver a lanzarlo.

![**](/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/segunda.png)

Nos pedira un usuario y contraseña, en este caso son root y castelao. Si usas una maquina creada desde cero por ti mismo ese usuario y contraseña lo habrás definido en el proceso.

![**](/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/tercera.png)

Una vez dentro usaremos una de las dos consolas que hemos abierto para lanzar odoo.
Tendremos que lanzar el siguiente comando para ello:

/opt/odoo/odoo.py

si todo a ido bien al final veremos el puerto necesario para acceder tando desde el proyeto que creemos en netbeans como desde el navegador:  8069

![**](/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/cuarta.png)

Vamos a abrir el navegador para acceder a odoo desde alli. Recomiendo usar Firefox es el que menos problemas me a dado para acceder.

Tendremos que acceder a esta ruta:    https://192.168.56.101:8069

La primera vez que lo hagamos la plataforma nos pedira que creemos una base de datos y escojamos el idioma asi como la descarga opcional de unos archivos iniciales, es recomendable descargalos. Una cosa que deberemos hacer es registranos en Odoo y confirmar un correo que ellos nos mandarán si no lo hacemos la base creada terminará siendo eliminada y perderemos el trabajo hecho debiendo empezar desde cero.

Si todo ha ido bien deberiamos tener esto:

![**](/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/quinta.png)

En la otra consola que tenemos vamos a instalar un modulo de prueba que posteriormente descargaremos al proyecto que creemos en NetBeans. Usaremos este comando:

odoo.py scaffold openacademy addons

esto crear una estructura cuya carpeta principal sera openacademy y estará situada en addons.

Bien ahora vamos a crear un proyecto en Netbeans. Necesitaremos añadir python y php a Netbeans si no los tenemos ya para que nos de la posibilidad de crear un proyecto a partir  de estos.

![**](/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/sexta.png)

Usaremos php application from remote server, le daremos el nombre que querramos al proyecto (este no sera el nombre que despues usaremos para crear el modulo).

En la pantalla de remote connection tendremos que realizar una configuraciones previas:

Project URL  http//192.168.56.101:8069

En la pestaña Manage crearemos una conexión remota(la creada anteriormente para crear temas en odoo vale):

![**](/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/septima.png)

Testearemos la conexión para comprobar que lo hemos hecho bien y que todo esta funcionando correctamente.

Hay que saber que para que odoo reconozca los temas o modulos que creemos deberemos alojarlos en una ruta muy concreta:   sftp://192.168.56.101/opt/odoo/addons

En la ventana de confirmacion unicamente seleccionaremos openacademy es un modulo por defecto de odoo, asi tendremos un ejemplo ya creado sobre el que trabajar.

La estrutura que tenemos en openacademy es mas complicada de lo necesario asi que vamos a modificarla.(podeis crear una copia de openacademy para no perder el original y renombralo).

Los archivos controllers.py y models.py los sacaremos de sus respectivas carpetas para ponerlos ala altura de __init__.py. Hecho esto borraremos las carpetas models y controllers con lo que tengan dentro.

Con los archivos demo.xml y templates.xml haremos lo mismo pero no borramos la carpeta de donde provienen.
