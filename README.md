## Module-TutorialE

***Tutorial para de creacion de modulos para Odoo***

En este tutorial vamos a explicar como poder cear modulos para luego usarlos en la plataforma Odoo.

# Tabla de contenido
- [Pasos Previos](#pasos-previos).
- [Implementación Básica de Odoo](#implementación-básica-de-odoo).
- [Creación De Courses Y Sessions Y Sus Relaciónes](#creación-de-courses-y-sessions).
- [Creación De Los Contactos Y Tags](#creación-de-los-contactos-y-tags).
- [Modificaciones En Session](#modificaciones-en-session).
- [Warnnings Emergentes](#warnnings-emergentes).
- [Constrains](#constrains).
- [Advanced Views](#advanced-views).
- [Que Más Se Puede Hacer](#que-más-se-puede-hacer).
- [FAQs](#faqs).

## Pasos Previos:

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

## Implementación Básica de Odoo 

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

## Creación De Courses Y Sessions

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

Vamos a crear relaciones many2one , one2many, y many2many entre los cusrsos(courses), las sesiones(sessions) y los asistentes a las sesiones(attendees). Esto hara posible ver las sessiones en cada curso o a que curso pertenece una sesion o los asistentes a una sesion de un curso.

Many2one:

añadimos en models.py a la clase course:

```java
responsible_id = fields.Many2one('res.users',
        ondelete='set null', string="Responsible", index=True)

```

añadimos en models.py a la clase session:

```java
instructor_id = fields.Many2one('res.partner', string="Instructor")
    course_id = fields.Many2one('openacademy.course',
        ondelete='cascade', string="Course", required=True)

```

añadimos en el openacademy.xml:

esto va dentro del group del course_form_view:
```java
<field name="responsible_id"/>
```

es un record dentro de openacademy.xml, sobreescribe la lista visible de cursos convirtiendola en una tabla con u elemento mas el responsable del curso:
```java
<!-- override the automatically generated list view for courses -->
        <record model="ir.ui.view" id="course_tree_view">
            <field name="name">course.tree</field>
            <field name="model">openacademy.course</field>
            <field name="arch" type="xml">
                <tree string="Course Tree">
                    <field name="name"/>
                    <field name="responsible_id"/>
                </tree>
            </field>
        </record>

```

estos dos grupos se añaden al session_form_view, esto nos permitirá al crear ñas sesion indicar a que curso pertenece ponerle un nombre y un instructor; por otro lado nos permite ponerle una fecha una duracion y un numero de asientos disponibles en la sesion:

```java
<group string="General">
                                <field name="course_id"/>
                                <field name="name"/>
                                <field name="instructor_id"/>
                            </group>
                            <group string="Schedule">
                                <field name="start_date"/>
                                <field name="duration"/>
                                <field name="seats"/>
                            </group>

```

este record se añade como los demás, nos va a generar la tabla con las sesiones creadas, nombre de sesion y curso al que pertenece:

```java
<!-- session tree/list view -->
        <record model="ir.ui.view" id="session_tree_view">
            <field name="name">session.tree</field>
            <field name="model">openacademy.session</field>
            <field name="arch" type="xml">
                <tree string="Session Tree">
                    <field name="name"/>
                    <field name="course_id"/>
                </tree>
            </field>
        </record>

```

One2many:

añadimos a la clase course de models.py lo siguiente:

```java
session_ids = fields.One2many(
        'openacademy.session', 'course_id', string="Sessions")
```

en el openacademy.xml dentro del notebook de course_form_view añadimos esta página, podemo borar una de muestra que se llama about

```java
<page string="Sessions">
                                <field name="session_ids">
                                    <tree string="Registered sessions">
                                        <field name="name"/>
                                        <field name="instructor_id"/>
                                    </tree>
                                </field>

```

Many2many:

en models.py añadimos a la clase session:

```java
 attendee_ids = fields.Many2many('res.partner', string="Attendees")

```

en openacademy.xml añadimos un label y un field a session_form_view justo debajo del group antes de cerrar sheet:

```java
<label for="attendee_ids"/>
<field name="attendee_ids"/>

```
Ua vez hecho esto ya podremos crear cursos y sessiones y ver como se relacionan unos con otros.

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/1.png)

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/2.png)

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/3.png)

## Creación De Los Contactos Y Tags

Ahora vamos a crear una lista de contactos y una de tag asociados a ellos para asi a poder añadir esos contactos a las sesiones o como responsable de un curso o instructor de una sesion. Para ello crearemos un partner.py y un partner.xml ambos en la carpeta openacademy

importaremos en __init__.py partner para que se lance:
```java
from . import partner

```

y en partner.py crearemos la clase partner asi como una relacion many2many con las sesiones

```java
# -*- coding: utf-8 -*-
from openerp import fields, models

class Partner(models.Model):
    _inherit = 'res.partner'

    # Add a new column to the res.partner model, by default partners are not
    # instructors
    instructor = fields.Boolean("Instructor", default=False)

    session_ids = fields.Many2many('openacademy.session',
        string="Attended Sessions", readonly=True)

```

en el partner.xml añadimos todo eso:

```java
<?xml version="1.0" encoding="UTF-8"?>
 <openerp>
    <data>
        <!-- Add instructor field to existing view -->
        <record model="ir.ui.view" id="partner_instructor_form_view">
            <field name="name">partner.instructor</field>
            <field name="model">res.partner</field>
            <field name="inherit_id" ref="base.view_partner_form"/>
            <field name="arch" type="xml">
                <notebook position="inside">
                    <page string="Sessions">
                        <group>
                            <field name="instructor"/>
                            <field name="session_ids"/>
                        </group>
                    </page>
                </notebook>
            </field>
        </record>

        <record model="ir.actions.act_window" id="contact_list_action">
            <field name="name">Contacts</field>
            <field name="res_model">res.partner</field>
            <field name="view_mode">tree,form</field>
        </record>
        <menuitem id="configuration_menu" name="Configuration"
                  parent="main_openacademy_menu"/>
        <menuitem id="contact_menu" name="Contacts"
                  parent="configuration_menu"
                  action="contact_list_action"/>
    </data>
</openerp>

```

bien ahora y ya deberiamos ser capaces una vez subido el codigo al servidor y actualizado el modulo de ver una seccion de configuracíon nueva con dos pestañas accesible.

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/4.png)

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/5.png)

Ahora vamos a crear dos tags especificos y vamos a hacer que solo los contactos que tengan estos tags sean seleccionable a la hora de elegir un intructor para una sesion.

en models.py y e la clase session modificamos el instructor_id:
```java
instructor_id = fields.Many2one('res.partner', string="Instructor",
        domain=['|', ('instructor', '=', True),
                     ('category_id.name', 'ilike', "Teacher")])


```

esto lo añadimos dentro de partner.xml antes de cerrar data, esto nos da una lista de tags por defecto y los dos tags que nosotros creamos.

```java
<record model="ir.actions.act_window" id="contact_cat_list_action">
            <field name="name">Contact Tags</field>
            <field name="res_model">res.partner.category</field>
            <field name="view_mode">tree,form</field>
        </record>
        <menuitem id="contact_cat_menu" name="Contact Tags"
                  parent="configuration_menu"
                  action="contact_cat_list_action"/>

        <record model="res.partner.category" id="teacher1">
            <field name="name">Teacher / Level 1</field>
        </record>
        <record model="res.partner.category" id="teacher2">
            <field name="name">Teacher / Level 2</field>
        </record>

```
Ahora a la hora de añadir un instructor a una session solo podremos añadir los contactos con estos dos tags específicos.

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/6.png)

## Modificaciones En Session

Vamos a añadir tanto a la hora de crear una session como una vez creada una barra que nos indique el aforo de la sesion.

en models.py añadimos lo siguiente a la clase session:

```java
taken_seats = fields.Float(string="Taken seats", compute='_taken_seats')

    @api.depends('seats', 'attendee_ids')
    def _taken_seats(self):
        for r in self:
            if not r.seats:
                r.taken_seats = 0.0
            else:
                r.taken_seats = 100.0 * len(r.attendee_ids) / r.seats

```

y en openacademy.xml en session_form_view lo siguiente en group "shedule" al final antes de cerrarlo:

```java
<field name="taken_seats" widget="progressbar"/>

```
en session_tree_view en el tree antes de cerrarlo esto:

```java
<field name="taken_seats" widget="progressbar"/>

```

Vamos a hacer que por defecto al crear una session nos ponga la fecha actual con la opcion de cambiarlo si queremos.

en models.py dentro de la clase session cambiamos el star_date:

```java
start_date = fields.Date(default=fields.Date.today)

```

y añadimos al final de la clase session, asi la sesion esta activa por defecto:


```java
active = fields.Boolean(default=True)

```

en el openacademy.xml en el group "general" de session_form_view añadimos esto antes de cerralo:

```java
<field name="active"/>

```
Ya tenemos la sesion activa por defecto al crearla y la fecha actual seleccionada:

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/7.png)

## Warnnings Emergentes

Vamos a crear unas ventanas de aviso emergentes parecidas a las toast de android que saltaran si determinadas variable no son correcta, por ejemplo si hay mas asistentes que asisentos en la session o si introducimos un numero negativo de asientos.

unicamente añadimos en models.py fuera de las clases lo siguiente:

```java
@api.onchange('seats', 'attendee_ids')
    def _verify_valid_seats(self):
        if self.seats < 0:
            return {
                'warning': {
                    'title': "Incorrect 'seats' value",
                    'message': "The number of available seats may not be negative",
                },
            }
        if self.seats < len(self.attendee_ids):
            return {
                'warning': {
                    'title': "Too many attendees",
                    'message': "Increase seats or remove excess attendees",
                },
            }

```
![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/8.png)

## Constrains

Ahora usaremos otra forma de controlar los datos que vamos introduciendo, por ejemplo que no sea posible que un instructor asista a su propia sesion o que dos cursos se llamen igual. Estos no aparecen asta que intentamos guardar la sesion o el courso mientras que en el caso anterior son instantanes all cambiar de campo de escritura.

Para evitar que un instructor asista a su sesion:

modificamos en modesl.py la linea de importacion: 

```java
from openerp import models, fields, api, exceptions

```
y añadimos fuera de las clases otro metodo o funcion predefinida con la que controlar esto:

```java
@api.constrains('instructor_id', 'attendee_ids')
    def _check_instructor_not_in_attendees(self):
        for r in self:
            if r.instructor_id and r.instructor_id in r.attendee_ids:
                raise exceptions.ValidationError("A session's instructor can't be an attendee")

```

Para comprobar si el nombre de la descripcion y el curso son diferentes, nos saltar un aviso al guardar si son iguales y tambien hacemos que el nombre sea unico.

añadimos en models.py dentro de la clase course al final:

```java
sql_constraints = [
        ('name_description_check',
         'CHECK(name != description)',
         "The title of the course should not be the description"),

        ('name_unique',
         'UNIQUE(name)',
         "The course title must be unique"),
    ]

```

Como hicimos que el nombre sea unico ahora al crea una copia de un curso no nos dejaran hacerlo. Vamos permitir que nos deje duplicar u curso pero cambiado el nombre a "Copy of" [nombre original], asi si podremos hacer duplicados de cursos.

en models.py añadimos en la clase course antes del _sql_constraints:

```java
@api.multi
    def copy(self, default=None):
        default = dict(default or {})

        copied_count = self.search_count(
            [('name', '=like', u"Copy of {}%".format(self.name))])
        if not copied_count:
            new_name = u"Copy of {}".format(self.name)
        else:
            new_name = u"Copy of {} ({})".format(self.name, copied_count)

        default['name'] = new_name
        return super(Course, self).copy(default)

```

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/9.png)

## Advanced Views

Ya hemos terminado con lo básico ahora vamos a ver como implementar algunas herramientas que nos faciliten el uso y visualizacion de todos los datos:

* Tree views: vamos a establecer que las sessiones tengan un color diferente segun su duracion, azul<5>negro<15>rojo, en dias.
* Calendars: un calendario en el que ver de manera organizada las sesiones.
* Search views: añadiremos un tag a la barra de busqueda de los cursos para que busque en mis cursos por defecto, asi un usuario solo verá en principio los cursos en los que participa de alguna forma.
* Gantt: implementaremos este tipo de diagrama.
* Graph views: implementaremos graficos para analizar algunos datos de los cursos y sesiones.
* Kanban: con esto agruparemos las sesiones por cada curso pudiendo minimizar maximizar o desplazar un curso par ordenarlo a nuestro gusto.


TREE VIEWS:

unicmente modificaremos el session_tree_view:

```java
<tree string="Session Tree" decoration-info="duration&lt;5" decoration-danger="duration&gt;15">

```

añadiremos un field al tree antes de la barra de progreso.

```java
<field name="duration" invisible="1"/>

```

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/10.png)

CALENDARS:

es un poco mas complicado de implemetar, al igaul que tenemos un boton para abrir el detalle de una session, tendremos ahora uno para abrir el calendario.

en models.py teenmos que importar lo sigueinte:

```java
from datetime import timedelta

```

añadir esto en la clase session antes de @api.depends:

```java
end_date = fields.Date(string="End Date", store=True,
        compute='_get_end_date', inverse='_set_end_date')

```

y esto en la misma clase antes de @api.constraints:

```java
@api.depends('start_date', 'duration')
    def _get_end_date(self):
        for r in self:
            if not (r.start_date and r.duration):
                r.end_date = r.start_date
                continue

            # Add duration to start_date, but: Monday + 5 days = Saturday, so
            # subtract one second to get on Friday instead
            start = fields.Datetime.from_string(r.start_date)
            duration = timedelta(days=r.duration, seconds=-1)
            r.end_date = start + duration

    def _set_end_date(self):
        for r in self:
            if not (r.start_date and r.end_date):
                continue

            # Compute the difference between dates, but: Friday - Monday = 4 days,
            # so add one day to get 5 days instead
            start_date = fields.Datetime.from_string(r.start_date)
            end_date = fields.Datetime.from_string(r.end_date)
            r.duration = (end_date - start_date).days + 1

```

en oepenacademy.xml añadimos es record antes del session_list_action:

```java
<!-- calendar view -->
        <record model="ir.ui.view" id="session_calendar_view">
            <field name="name">session.calendar</field>
            <field name="model">openacademy.session</field>
            <field name="arch" type="xml">
                <calendar string="Session Calendar" date_start="start_date"
                          date_stop="end_date"
                          color="instructor_id">
                    <field name="name"/>
                </calendar>
            </field>
        </record>

```

y en el session_list_action modificamos es este field:

```java
<field name="view_mode">tree,form,calendar</field>

```

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/11.png)

SEACH VIEWS

unicamente tabajamos en openacademy.xml, dentro de course_search_view añadimos dentre de search:

```java
<filter name="my_courses" string="My Courses"
                            domain="[('responsible_id', '=', uid)]"/>
                    <group string="Group By">
                        <filter name="by_responsible" string="Responsible"
                                context="{'group_by': 'responsible_id'}"/>
                    </group>

```

en el course_list_action un field mas antes del ultimo:

```java
<field name="context" eval="{'search_default_my_courses': 1}"/>

```

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/12.png)

GANTT:

en models.py añadimos dentro de la clase session y despues de end_date:


```java
hours = fields.Float(string="Duration in hours",
                         compute='_get_hours', inverse='_set_hours')

```

en la misma clase antes de @api.constrains:

```java
@api.depends('duration')
    def _get_hours(self):
        for r in self:
            r.hours = r.duration * 24

    def _set_hours(self):
        for r in self:
            r.duration = r.hours / 24

```

en openacademy.xml añadimos un record antes del session_list_action:

```java
<record model="ir.ui.view" id="session_gantt_view">
            <field name="name">session.gantt</field>
            <field name="model">openacademy.session</field>
            <field name="arch" type="xml">
                <gantt string="Session Gantt" color="course_id"
                       date_start="start_date" date_delay="hours"
                       default_group_by='instructor_id'>
                    <field name="name"/>
                </gantt>
            </field>
        </record>

```

y modificamos el ultimo field del mismo list_action:

```java
<field name="view_mode">tree,form,calendar,gantt</field>

```

GRAPH VIEWS

en models.py añadimos en lo clase session despues del hours añadido en pasos anteriores:

```java
attendees_count = fields.Integer(
        string="Attendees count", compute='_get_attendees_count', store=True)

```

tambien antes de @api.constrains lo siguiente:

```java
@api.depends('attendee_ids')
    def _get_attendees_count(self):
        for r in self:
            r.attendees_count = len(r.attendee_ids)

```

en openacademy.py añadimos  un record mas antes de session_list_action:

```java
<record model="ir.ui.view" id="openacademy_session_graph_view">
            <field name="name">openacademy.session.graph</field>
            <field name="model">openacademy.session</field>
            <field name="arch" type="xml">
                <graph string="Participations by Courses">
                    <field name="course_id"/>
                    <field name="attendees_count" type="measure"/>
                </graph>
            </field>
        </record>

```

modificamo el ultimo field del mismo list_action:

```java
 <field name="view_mode">tree,form,calendar,gantt,graph</field>

```

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/13.png)

KANBAN:

en models.py añadimos una linea debajo  de active en la clase session:

```java
color = fields.Integer()


```

en open academy.xml añadimos el record correspondiente:

```java
<record model="ir.ui.view" id="view_openacad_session_kanban">
            <field name="name">openacad.session.kanban</field>
            <field name="model">openacademy.session</field>
            <field name="arch" type="xml">
                <kanban default_group_by="course_id">
                    <field name="color"/>
                    <templates>
                        <t t-name="kanban-box">
                            <div
                                    t-attf-class="oe_kanban_color_{{kanban_getcolor(record.color.raw_value)}}
                                                  oe_kanban_global_click_edit oe_semantic_html_override
                                                  oe_kanban_card {{record.group_fancy==1 ? 'oe_kanban_card_fancy' : ''}}">
                                <div class="oe_dropdown_kanban">
                                    <!-- dropdown menu -->
                                    <div class="oe_dropdown_toggle">
                                        <i class="fa fa-bars fa-lg"/>
                                        <ul class="oe_dropdown_menu">
                                            <li>
                                                <a type="delete">Delete</a>
                                            </li>
                                            <li>
                                                <ul class="oe_kanban_colorpicker"
                                                    data-field="color"/>
                                            </li>
                                        </ul>
                                    </div>
                                    <div class="oe_clear"></div>
                                </div>
                                <div t-attf-class="oe_kanban_content">
                                    <!-- title -->
                                    Session name:
                                    <field name="name"/>
                                    <br/>
                                    Start date:
                                    <field name="start_date"/>
                                    <br/>
                                    duration:
                                    <field name="duration"/>
                                </div>
                            </div>
                        </t>
                    </templates>
                </kanban>
            </field>
        </record>

```

y modificamos el list_action como anteriormente:

```java
<field name="view_mode">tree,form,calendar,gantt,graph,kanban</field>

```

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/14.png)

## Que Más Se Puede Hacer

Con todo esto ya tenemos un tutorial completito, pero se peden hacer mas cosas aun:

* Workflows: esto no facilita el seguimiento de la creacion de sessiones, nos va indicando si queremos confirmar lo que escribimos y estamos de acuerdo con lo escrito o si queremos volver atrás y modificar los campos antes de guardar la session creada.
* Security: he creado un usuario que pertenece a un grupo de usuarios que solo podran podran ver, no modificar, las sessiones; empleando otro metodo un grupo openacademy/manager en el cual sus usuarios tendran todos los permisos posible y tambien hago que los cusros y las sessiones sean visibles a todos los ususario. Aparte de esto he creado una condicion que hace que solo el responsable de un curso pueda modificar dicho curso, en caso de no haber responsable todos los usuarios pueden modificarlo.
* Wizards: entiendo que es una ventana popup con la que puedes interactuar pero e intentado hacerla funcionar varias veces pero el servidor se vuelve loco solo me a pasado en este punto del tutorial, la parte que da error es la parte visual en el xml. Al final lo he tenido que eliminar del xml.
* Internacionalization: sirve para poder implementar varios idiomas, creo que al exportar el idioma elegido de odoo hace una traduccion de todos los texto, esto lo añades al proyecto de Neatbeans y creo que alli puedes modificar la traduccion por si alguna palabra no esta bien traducida o no esta traducida directamente.
* Reporting: aqui hacemos dos cosas, la posibilidad de crear un informe e imprimirlo y por otro lado una tabla en el propio odoo con los datos relevantes de los cursos y sessiones: un grafico, un calendario la lista de los cursos.
* WebServices: aqui se trata de crear dos aplicaciones en pyton una que solicita acceso a los xml de odoo y crear a partir de ello una interfaz supongo lo haré si tengo tiempo y otra en pyton tambien que hace uso de librerias de urllib2 y json, interactuando con un servidor odoo tambien lo haré en cuanto pueda.


Ahora van alguna capturas de pantalla de estos ultimos puntos, los cuales tambien estan agrupados en un commit:

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/15.png)

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/16.png)

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/17.png)

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/18.png)

![**](https://github.com/mpereirasalgado/Module-TutorialE/blob/master/openacademy/images/19.png)

## FAQs

Micael Pereira Salgado

mpereirasalgado@danielcastelao.org

micaelcaballero@gmail.com
