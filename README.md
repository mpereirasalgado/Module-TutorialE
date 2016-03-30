# Module-TutorialE

En este tutorial vamos a explicar como poder cear modulos para luego usarlos en la plataforma Odoo.

Para empezar necesitaremos una serie de programas:

* Virtual Box: en este programa crearemos una maquina virtual capaz de lanzar un servidor Odoo completamente funcional para poder ir subiendo nuestro trabajo y luego acceder a el desde la plataforma Odoo en un navegador web.
* Netbeans: aqui crearemos la estructura de carpetas y archivos necesarios para que nuestro tema sea funcional y reconocido por la plataforma Odoo.
* Putty: usaremos este progama para hacer conexiones ssh al servidor creado con la maquina virtual y  poder explorar mediante consola si el trabajo que vamos subiendo al servidor esta realmente subido, asi como, lanzar el servidor Odoo.

Ya hemos visto anteriormente como crear y configurar la maquina virtual de cero para luego implementar Odoo y asi poder usarlo. Esto puede ser tedioso y dar errores, existen maquinas virtuales prehechas de todo tipo, tambien las hay para Odoo. Usar una turnkey(asi se llaman estas maquinas preconfiguradas) es el camino mas rapido aunque en algún caso puede no ajustarse a nuestras necesidades. En este ccaso usamos una turnkey postgresql.

Vamos con los pasos a seguir:

Lanzar la maquina virtual para obtener las direcciones necesarias para diferentes tipos de conexion.

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/primera.png)

Ahora vamos a abrir con PUTTY dos consolas; no son necesarias dos pero a la hora de trabajar es mucho mas sencillo y util, usaremos una para lanzar odoo y otra para navegar por la estructura de carpetas. De no hacerlo asi tendriamos que constantemente ir cerrando odoo y volver a lanzarlo.

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/segunda.png)

Nos pedira un usuario y contraseña, en este caso son root y castelao. Si usas una maquina creada desde cero por ti mismo ese usuario y contraseña lo habrás definido en el proceso.

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/tercera.png)

Una vez dentro usaremos una de las dos consolas que hemos abierto para lanzar odoo.
Tendremos que lanzar el siguiente comando para ello:

/opt/odoo/odoo.py

si todo a ido bien al final veremos el puerto necesario para acceder tando desde el proyeto que creemos en netbeans como desde el navegador:  8069

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/cuarta.png)

Vamos a abrir el navegador para acceder a odoo desde alli. Recomiendo usar Firefox es el que menos problemas me a dado para acceder.

Tendremos que acceder a esta ruta:    https://192.168.56.101:8069

La primera vez que lo hagamos la plataforma nos pedira que creemos una base de datos y escojamos el idioma asi como la descarga opcional de unos archivos iniciales, es recomendable descargalos. Una cosa que deberemos hacer es registranos en Odoo y confirmar un correo que ellos nos mandarán si no lo hacemos la base creada terminará siendo eliminada y perderemos el trabajo hecho debiendo empezar desde cero.

Si todo ha ido bien deberiamos tener esto:

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/quinta.png)

En la otra consola que tenemos vamos a instalar un modulo de prueba que posteriormente descargaremos al proyecto que creemos en NetBeans. Usaremos este comando:

odoo.py scaffold openacademy addons

esto crear una estructura cuya carpeta principal sera openacademy y estará situada en addons.

Bien ahora vamos a crear un proyecto en Netbeans. Necesitaremos añadir python y php a Netbeans si no los tenemos ya para que nos de la posibilidad de crear un proyecto a partir  de estos.

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/sexta.png)

Usaremos php application from remote server, le daremos el nombre que querramos al proyecto (este no sera el nombre que despues usaremos para crear el modulo).

En la pantalla de remote connection tendremos que realizar una configuraciones previas:

Project URL  http//192.168.56.101:8069

En la pestaña Manage crearemos una conexión remota(la creada anteriormente para crear temas en odoo vale):

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/septima.png)

Testearemos la conexión para comprobar que lo hemos hecho bien y que todo esta funcionando correctamente.

Hay que saber que para que odoo reconozca los temas o modulos que creemos deberemos alojarlos en una ruta muy concreta:   sftp://192.168.56.101/opt/odoo/addons

En la ventana de confirmacion unicamente seleccionaremos openacademy es un modulo por defecto de odoo, asi tendremos un ejemplo ya creado sobre el que trabajar.

La estrutura que tenemos en openacademy es mas complicada de lo necesario asi que vamos a modificarla.(podeis crear una copia de openacademy para no perder el original y renombralo).

Los archivos controllers.py y models.py los sacaremos de sus respectivas carpetas para ponerlos ala altura de __init__.py. Hecho esto borraremos las carpetas models y controllers con lo que tengan dentro.

Con los archivos demo.xml y templates.xml haremos lo mismo pero no borramos la carpeta de donde provienen.

Tendremos algo como esto:

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/octava.png)

Como con los temas tendremos que hacer un upload... de la carpeta openacademy modifica para que   los cambios surtan efecto en el servidor Odoo y poder acceder al modulo mas adelante desde el navegador.

Ahora vamos a empezar a modificarlos archivos:

__openerp__.py: borramos su contenido y le metemos este:

```java
# -*- coding: utf-8 -*-
{
    'name': "Open Academy",

    'summary': """Manage trainings""",

    'description': """
        Open Academy module for managing trainings:
            - training courses
            - training sessions
            - attendees registration
    """,

    'author': "My Company",
    'website': "http://www.yourcompany.com",

    # Categories can be used to filter modules in modules listing
    # Check https://github.com/odoo/odoo/blob/master/openerp/addons/base/module/module_data.xml
    # for the full list
    'category': 'Test',
    'version': '0.1',

    # any module necessary for this one to work correctly
    'depends': ['base'],

    # always loaded
    'data': [
        # 'security/ir.model.access.csv',
        'templates.xml',
    ],
    # only loaded in demonstration mode
    'demo': [
        'demo.xml',
    ],
}
```

openacademy/__init__.py

```java
# -*- coding: utf-8 -*-
from . import controllers
from . import models
```

openacademy/controllers.py

```java
# -*- coding: utf-8 -*-
from openerp import http

# class Openacademy(http.Controller):
#     @http.route('/openacademy/openacademy/', auth='public')
#     def index(self, **kw):
#         return "Hello, world"

#     @http.route('/openacademy/openacademy/objects/', auth='public')
#     def list(self, **kw):
#         return http.request.render('openacademy.listing', {
#             'root': '/openacademy/openacademy',
#             'objects': http.request.env['openacademy.openacademy'].search([]),
#         })

#     @http.route('/openacademy/openacademy/objects/<model("openacademy.openacademy"):obj>/', auth='public')
#     def object(self, obj, **kw):
#         return http.request.render('openacademy.object', {
#             'object': obj
#         })
```

demo.xml:

```java
<openerp>
    <data>
        <!--  -->
        <!--   <record id="object0" model="openacademy.openacademy"> -->
        <!--     <field name="name">Object 0</field> -->
        <!--   </record> -->
        <!--  -->
        <!--   <record id="object1" model="openacademy.openacademy"> -->
        <!--     <field name="name">Object 1</field> -->
        <!--   </record> -->
        <!--  -->
        <!--   <record id="object2" model="openacademy.openacademy"> -->
        <!--     <field name="name">Object 2</field> -->
        <!--   </record> -->
        <!--  -->
        <!--   <record id="object3" model="openacademy.openacademy"> -->
        <!--     <field name="name">Object 3</field> -->
        <!--   </record> -->
        <!--  -->
        <!--   <record id="object4" model="openacademy.openacademy"> -->
        <!--     <field name="name">Object 4</field> -->
        <!--   </record> -->
        <!--  -->
    </data>
</openerp>
```

models.py:

```java
# -*- coding: utf-8 -*-

from openerp import models, fields, api

# class openacademy(models.Model):
#     _name = 'openacademy.openacademy'

#     name = fields.Char()
```

templates.xml:

```java
<openerp>
    <data>
        <!-- <template id="listing"> -->
        <!--   <ul> -->
        <!--     <li t-foreach="objects" t-as="object"> -->
        <!--       <a t-attf-href="{{ root }}/objects/{{ object.id }}"> -->
        <!--         <t t-esc="object.display_name"/> -->
        <!--       </a> -->
        <!--     </li> -->
        <!--   </ul> -->
        <!-- </template> -->
        <!-- <template id="object"> -->
        <!--   <h1><t t-esc="object.display_name"/></h1> -->
        <!--   <dl> -->
        <!--     <t t-foreach="object._fields" t-as="field"> -->
        <!--       <dt><t t-esc="field"/></dt> -->
        <!--       <dd><t t-esc="object[field]"/></dd> -->
        <!--     </t> -->
        <!--   </dl> -->
        <!-- </template> -->
    </data>
</openerp>
```

Con esto tendremos el modulo básico, ahora vamos a ir añadiendole pestañas que tendran diferentes  funcionalidades:

La primera que vamos a añadir es la de Courses, para ello modificaremos models.py añadiendo lo siguiente:

```java
class Course(models.Model):
    _name = 'openacademy.course'

    name = fields.Char(string="Title", required=True)
    description = fields.Text()
Tambien tenemos que modificar el archivo xml demo.xml, añadiendo el siguiente <record>:

<record model="openacademy.course" id="course0">
            <field name="name">Course 0</field>
            <field name="description">Course 0's description

Can have multiple lines
            </field>
        </record>
        <record model="openacademy.course" id="course1">
            <field name="name">Course 1</field>
            <!-- no description for this one -->
        </record>
        <record model="openacademy.course" id="course2">
            <field name="name">Course 2</field>
            <field name="description">Course 2's description</field>
        </record>
```

Ya tenemos courses creado pero no podemos acceder al el asi que vamos a crear un boton llamado courses para acceder, esto lo hacemos con estas lineas en los siguientes archivos:

En __openerp__.py deberemos añadir en data al igual que para los temas el archivo al que hacemos referencia  en este caso 'openacademy.xml'. Posiblemente el archivo este en la carpeta views, para facilitar el tutorial lo sacaremos de ahi para ponerlo a la misma altura que __openerp__.py.

Modificamos openacademy.xml:
```java
<?xml version="1.0" encoding="UTF-8"?>
<openerp>
    <data>
        <!-- window action -->
        <!--
            The following tag is an action definition for a "window action",
            that is an action opening a view or a set of views
        -->
        <record model="ir.actions.act_window" id="course_list_action">
            <field name="name">Courses</field>
            <field name="res_model">openacademy.course</field>
            <field name="view_type">form</field>
            <field name="view_mode">tree,form</field>
            <field name="help" type="html">
                <p class="oe_view_nocontent_create">Create the first course
                </p>
            </field>
        </record>

        <!-- top level menu: no parent -->
        <menuitem id="main_openacademy_menu" name="Open Academy"/>
        <!-- A first level in the left side menu is needed
             before using action= attribute -->
        <menuitem id="openacademy_menu" name="Open Academy"
                  parent="main_openacademy_menu"/>
        <!-- the following menuitem should appear *after*
             its parent openacademy_menu and *after* its
             action course_list_action -->
        <menuitem id="courses_menu" name="Courses" parent="openacademy_menu"
                  action="course_list_action"/>
        <!-- Full id location:
             action="openacademy.course_list_action"
             It is not required when it is the same module -->
    </data>
</openerp>
```

Ahora le añadimos otro record para que podamos ver dentro de cada course el nombre y su descripcion:
```java
<record model="ir.ui.view" id="course_form_view">
            <field name="name">course.form</field>
            <field name="model">openacademy.course</field>
            <field name="arch" type="xml">
                <form string="Course Form">
                    <sheet>
                        <group>
                            <field name="name"/>
                            <field name="description"/>
                        </group>
                    </sheet>
                </form>
            </field>
        </record>
```

Vamos a crear una paginas dentro de cada course añadiendo un notebook de la siguiente manera:
esto va justo depues de los field que añadimos anteriormente.

```java
<notebook>
                            <page string="Description">
                                <field name="description"/>
                            </page>
                            <page string="About">
                                This is an example of notebooks
                            </page>
                        </notebook>
```

En este modulo tenemos una barra de busqueda que podemos modificar, vamos a hacerlo para que podamos buscar por titulo y descripcion de cada course. Para ello añadiremos otro record al archivo openerp.xml:

```java
<record model="ir.ui.view" id="course_search_view">
            <field name="name">course.search</field>
            <field name="model">openacademy.course</field>
            <field name="arch" type="xml">
                <search>
                    <field name="name"/>
                    <field name="description"/>
                </search>
            </field>
        </record>
```

Como en el tutorial del los temas de odoo si queremos ver cambios en el navegador de lo que vamos haciendo tenemos que hacer un upload... de cada archivo de NetBeans o de la carpeta openacademy que los coniene. Tambien tendremos que atualizar el modulo en las aplicaciones en linea y al igual que en tutorial de tamas si al actualizar nos dan errores podemos con la consola de PUTTY con la que arrancamos odoo comprobar los fallos. Ademas si no da fallos pero no vemos cambios ya hemos visto que una posible solucion es volver a instalar el website builder y el modulo.

De momento hemos terminado con Courses ahora vamos con Sessions, es basicamente lo mismo:

Añadimos en models.py una nueva clase:

```java
class Session(models.Model):
    _name = 'openacademy.session'

    name = fields.Char(required=True)
    start_date = fields.Date()
    duration = fields.Float(digits=(6, 2), help="Duration in days")
    seats = fields.Integer(string="Number of seats")
```

Añadimos un record a openacademy.xml:

```java
<!-- session form view -->
        <record model="ir.ui.view" id="session_form_view">
            <field name="name">session.form</field>
            <field name="model">openacademy.session</field>
            <field name="arch" type="xml">
                <form string="Session Form">
                    <sheet>
                        <group>
                            <field name="name"/>
                            <field name="start_date"/>
                            <field name="duration"/>
                            <field name="seats"/>
                        </group>
                    </sheet>
                </form>
            </field>
        </record>

        <record model="ir.actions.act_window" id="session_list_action">
            <field name="name">Sessions</field>
            <field name="res_model">openacademy.session</field>
            <field name="view_type">form</field>
            <field name="view_mode">tree,form</field>
        </record>

        <menuitem id="session_menu" name="Sessions"
                  parent="openacademy_menu"
                  action="session_list_action"/>
```

