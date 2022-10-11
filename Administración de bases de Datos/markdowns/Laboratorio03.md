author: Author Name
summary: Laboratorio sobre Administración de alamacenamiento y usuarios 
id: laboratorio-03
tags: guide. 
categories: Roles, Usuarios.
environments: Web
status: Published
feedback link: A link where users can go to provide feedback (Maybe the git repo)
analytics account: Google Analytics ID

# Administaricón de Almacenamiento y usuarios 

## Objetivos 
Duration: 0:01:00

  * Administrar el almacenamiento de Oracle a través Tablespace y Datafiles.
* Conocer como es la sintaxis de la creación de un usuario y perfiles.
* Aprender los diferentes tipos de roles para los usuarios.

## Herramientas 
Duration: 0:01:00

Para esta práctica se hará uso de estas herramientas: 

### Oracle DB 19c 

![imagen](img/oracle.png)

Oracle Database es un sistema de gestión de base de datos de tipo objeto-relacional (ORDBMS, por el acrónimo en inglés de Object-Relational Data Base Management System), desarrollado por Oracle Corporation. Oracle Database 19c es la versión actual a largo plazo, que además proporciona el nivel más alto de estabilidad de la versión y el plazo más largo para asistencia y corrección de errores.

### SQL Developer 

![sql](img/sqldev.png)
Es una interfaz gráfica de usuario gratuita que permite a los usuarios y administradores de bases de datos realizar sus tareas con menos clicks y pulsaciones de teclas. SQL Developer es una herramienta de productividad cuyo objetivo principal es ayudar al usuario final a ahorrar tiempo y maximizar el retorno de la inversión en el paquete de tecnología de Oracle Database.

## Introducción
Duration: 0:01:00
En esta guía se dará a conocer las diferencias entre un tablespaces y datafiles, al igual que como se administran cada uno de ellos, como puede ser crearlos, cambiar sus parámetros y eliminarlos. Al igual aprenderemos como crear usuarios, a crear roles que estos mismos usuarios pueden tener y crear perfiles, y como podemos dejarles ciertos permisos dependiendo de las tareas que deben realizar.

## Administración de almacenamiento
Duration: 0:05:00
Como sabemos, Oracle guarda los datos de manera lógica en un tablespace y  físicamente en datafiles, que corresponden o están asociados a los tablespaces. Las bases de datos, tablespaces y datafiles están estrechamente relacionadas, pero es  de importancia saber que existen diferencias muy grandes:

* Una base de datos Oracle consiste en uno o más unidades de almacenamiento lógico llamados tablespaces, estos tablespaces son los que guardan todos los datos de la base de datos.
* Cada tablespaces en una base de datos Oracle consiste en uno o más archivos llamados datafiles, los cuales son la estructura física de los tablespaces.
* Los datos son guardados en datafiles que constituyen cada tablespace de la base   de datos, por ejemplo, la base de datos más simple podría tener como mínimo un tablespace y un datafiles.

![tablespace](img/Tablespace.png)

## Tablespace
Duration: 0:10:00
La estructura lógica está formada por los tablespace y los objetos de un esquema de la base de  datos (tablas, vistas, índices…) 

![tablespace](img/Tablespace2.png)

Una base de datos está formada por una o varias unidades lógicas llamadas Tablespaces. Un tablespace es la unidad de almacenamiento lógico. Además, cada una de estos Tablespaces está formada por uno o varios ficheros físicos que son los datafiles. Un datafile solamente puede pertenecer a un tablespace. Por lo tanto, los datafiles de una base de datos son todos los datafiles que forman parte de todos los tablespaces de la base.
Cuando se crea una base de datos, hay que crear al menos un tablespace, por lo que durante el proceso de creación de la base de datos siempre se indica el tablespace principal de ésta, que se llama SYSTEM.

*Propiedades:* 
* Localización de los ficheros de datos.
* Especificación de máximas cuotas de consumo de disco.
* Backup de datos.

