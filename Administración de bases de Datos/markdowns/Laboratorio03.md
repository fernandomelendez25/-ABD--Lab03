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
Duration: 0:30:00
La estructura lógica está formada por los tablespace y los objetos de un esquema de la base de  datos (tablas, vistas, índices…) 

<img src="img/Tablespace2.png" width="500">

Una base de datos está formada por una o varias unidades lógicas llamadas Tablespaces. Un tablespace es la unidad de almacenamiento lógico. Además, cada una de estos Tablespaces está formada por uno o varios ficheros físicos que son los datafiles. Un datafile solamente puede pertenecer a un tablespace. Por lo tanto, los datafiles de una base de datos son todos los datafiles que forman parte de todos los tablespaces de la base.
Cuando se crea una base de datos, hay que crear al menos un tablespace, por lo que durante el proceso de creación de la base de datos siempre se indica el tablespace principal de ésta, que se llama SYSTEM.

*Propiedades:* 
* Localización de los ficheros de datos.
* Especificación de máximas cuotas de consumo de disco.
* Backup de datos.

Cuando un objeto se crea dentro de un cierto Tablespace, este objeto adquiere todas las
propiedades antes descritas del Tablespace utilizado.

*Características:*
*  Debe estar asociado a un Datafiles.
*  Un Tablespace está asociado a uno o varios Datafiles y puede tener uno o ovarios usuarios.

### 5.1 Creación de Tablespace
Una de las tareas habituales en la administración de una base de datos Oracle es la de crear
un nuevo Tablespace para contener nuevos objetos, La sentencia CREATE TABLESPACE sirve
para crear un tablespace en una base de datos Oracle. Recordemos que los Tablespaces
permanentes contienen objetos, tablas, índices, etc. de uno o más esquemas. Los objetos en
los tablespaces permanentes se almacenan en archivos de datos, datafiles.

**Ejemplo 1.1** 
```
    CREATE TABLESPACE tablespace1
    DATAFILE 'C:/datafile_tb1.dbf' SIZE 10M;
```

### 5.2 SQL Developer 
Podemos crear tablespaces donde el/los datafiles se vayan extendiendo de forma automática,
si lo creamos de esta manera debemos tener cuidado en no olvidar la cláusula MAXSIZE, ya
que si la omitimos el datafile se extenderá hasta llenar el filesystem donde esté creado. 

**Ejemplo 1.2** 
```
  CREATE TABLESPACE tablespace2
    DATAFILE 'C:/datafile_tb2.dbf' SIZE 5M
    AUTOEXTEND ON NEXT 1M MAXSIZE 20M;
```
#### Ejercicio 1
Crear un tablespace auto expandible llamado TB_&lt;Carnet&gt;	 (sustituir “&lt;Carnet&gt;” por su número de carnet), asignar un datafile de 8 megabytes y que será guardado en el disco C. El tamaño máximo será de 36 Megabytes con extensiones de 4 Megabytes.

### 5.3 SQL Developer
Para cambiar el tamaño de un tablespace tenemos dos posibles opciones:
*  Modificar el tamaño de un datafile.
*  Agregar un nuevo datafile.
* 
**Ejemplo 1.3. agregando nuevos datafiles.**

Ahora modificaremos el tablespace creado en el Ejercicio 1. Antes de modificarlo es
recomendable ver cuántos datafiles conforman el tablespace, y que tamaño tienen, para en base
a esta información, añadir uno nuevo, siguiendo el estándar de nombres, y el tamaño apropiado.

A continuación, se presenta la forma de ver la información de los datafiles del tablespace.

```
  SELECT * FROM DBA_TABLESPACES;
  SELECT * FROM DBA_DATA_FILES;
```

Si ejecutamos este SELECT con el tablespace llamado “tablespace2” veremos la siguiente información:

<img src="img/tablamuestra.png" width="350">

Lo que nos indica que este tablespace posee un solo datafile de 5 MB de tamaño. A continuación, agregamos un nuevo datafile.

```
  ALTER TABLESPACE tablespace2
   ADD DATAFILE 'C:/datafile_tb2_2.dbf' SIZE 5M AUTOEXTEND OFF;
```

#### Ejercicio 2
Añadir un nuevo datafile al tablespace creado en el ejercicio 1. El tamaño será 10 Megabytes y será almacenado en el disco C:.


Mostrando la información de tablespace2 nuevamente:

<img src="img/tablespace2datafile.png" width ="350">


**Ejemplo 1.4 modificando un datafile.**

También podemos modificar un datafile existente.

```
  ALTER DATABASE DATAFILE 'C:/datafile_tb2_2.dbf' RESIZE 10M;
```

Mostrando la información de tablespace2 nuevamente:

<img src="img/TablespaceEJE1.4.png" width="400">

#### Ejercicio 3
Redimensionar el segundo datafile creado a 16 Megabytes.

### 5.4. SQL Developer
Primero debemos asegurarnos de que el contenido de los archivos de datos no es necesario, o se ha movido a otro tablespace.

**Ejemplo 1.5**

Borrando el tablespace1 anteriormente creado.

```
  DROP TABLESPACE tablespace1;
```


*¡La sentencia anterior borra un tablespace, pero NO los datafiles!* ahora podemos ir a borrar manualmente el datafile. La siguiente instrucción borra un tablespace incluyendo sus datafiles.

```
  DROP TABLESPACE tablespace2 INCLUDING CONTENTS AND DATAFILES;
```

## Datafiles

Un datafile es la representación física de un tablespace. Son los "ficheros de datos" donde se almacena la información físicamente. Los tablespaces están formados por datafiles, el crecimiento en el uso de memoria o el modo de gestión determinarán algunas configuraciones en ellos.

### 6.1 Ejemplos de Administración de Datafiles
<img src="img/datafiles_1.png" width="650">
<img src="img/datafiles_2.png" width="650">

<aside class="positive">
<b>Nota:</b>
<p>
  Los nuevos archivos deben existir antes de poder renombrar, esta sentencia no crea archivos.
</p>
</aside>

### 6.2 Limitantes en la creación de datafiles
- Los sistemas operativos pueden poner un límite con respecto al número de archivos que un proceso puede abrir simultáneamente. También podría poner límites con respecto al tamaño de los  archivos creados.
- Oracle impone un límite máximo de datafiles creados por cualquier base de datos; no se puede exceder del número de datafiles creados por el parámetro de inicialización DB_FILES.

## Administración de Usuarios

Un usuario es un nombre de acceso a la base de datos Oracle. Normalmente va asociado a una clave (password). 

Lo que puede hacer un usuario una vez ha accedido a la base de datos depende de los permisos que tenga asignados ya sea directamente (GRANT) como sobre algún rol que tenga asignado (CREATE ROLE).

El perfil que tenga asignado influye en los recursos del sistema de los que dispone un usuario a la hora de ejecutar Oracle (CREATE PROFILE).

La sintaxis para crear un usuario es:

```
  CREATE USER  <usuario>
  IDENTIFIED BY <contraseña>/EXTERNALLY
  DEFAULT TABLESPACE <espacio>
  TEMPORARY TABLESPACE <espacio>
  QUOTA {<xx> K | UNLIMITED ON <espacio>}
  PROFILE <perfil>
  PASSWORD EXPIRE
  ACCOUNT LOCK/UNLOCK
```


<table>
    <thead>
        <tr>
            <td><strong>Campo</strong></td>
            <td><strong>Descripcion</strong></td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>&lt;usuario&gt;</strong></td>
            <td>Debe ser único con respecto al resto de usuarios</td>
        </tr>
        <tr>
            <td><strong>&lt;contraseña&gt;</strong></td>
            <td>Un usuario autenticado de forma externa se ha de crear con la cláusula “IDENTIFIED EXTERNALLY”.</td>
        </tr>
        <tr>
            <td><strong>DEFAULT TABLESPACE</strong></td>
            <td>Indica aquel espacio de almacenamiento donde se crearán los objetos del esquema del usuario</td>
        </tr>
        <tr>
            <td><strong>TEMPORARY TABLESPACE </strong></td>
            <td>Indica el espacio de almacenamiento donde se almacenan los segmentos temporales requeridos por el usuario. No debe indicarse quota.</td>
        </tr>
        <tr>
            <td><strong>QUOTA</strong></td>
            <td>Indica la cantidad de espacio reservada en un determinado espacio de almacenamiento para el usuario. Por defecto un usuario no tiene quota en ningún espacio de almacenamiento. Indicando UNLIMITED, es ilimitado el espacio a usar.
            </td>
        </tr>
        <tr>
            <td><strong>PROFILE</strong></td>
            <td>Indica el perfil a asignar al usuario.
            </td>
        </tr>
        <tr>
            <td> <strong>PASSWORD EXPIRE</strong> </td>
            <td>Fuerza al usuario a cambiar la clave antes de conectarse a la base de datos.
            </td>
        </tr>
        <tr>
            <td><strong>ACCOUNT LOCK/UNLOCK</strong> </td>
            <td>Bloquea/desbloquea la cuenta de usuario e inhabilita el acceso.
            </td>
        </tr>
    </tbody>
</table>

**Ejemplo**
Creando un usuario sin derechos a guardar datos o crear objetos:
```
  CREATE USER usuariolimitado IDENTIFIED BY miclavesecreta;
```

#### Ejercicio 4
Crear un usuario “user&lt;carnet&gt;” (sustituir <carnet> con su carnet) la contraseña será “pass123”. Asignar como tablespace por defecto el creado en el ejercicio 1, y como tablespace temporal “TEMP”. Definir una cuota de 5MB de uso de espacio en el tablespace por defecto.

### 7.1 Modificación de un usuario 
Los usuarios pueden cambiar sus propias claves, sin embargo, para cambiar cualquier otro parámetro es necesario el privilegio “ALTER USER”.

Sintaxis para modificar un usuario.

```
  ALTER USER  <usuario>
  IDENTIFIED BY <contraseña>/EXTERNALLY
  DEFAULT TABLESPACE <espacio>
  TEMPORARY TABLESPACE <espacio>
  QUOTA {<xx> K | UNLIMITED ON <espacio>}
  DEFAULT ROLE <role>/ALL/ALL EXCEPT <role>/NONE
  PROFILE <perfil>
  PASSWORD EXPIRE
  ACCOUNT LOCK/UNLOCK
```

### 7.2 Eliminación de usuario
Los usuarios pueden ser eliminados de la BD utilizando el comando DROP USER. Este comando tiene un único parámetro, CASCADE, el cual permite borrar todos los objetos del usuario antes de eliminar el usuario.

## Roles y Privilegios

Los roles se pueden utilizar para gestionar los comandos de sistema disponibles para los usuarios. Estos incluyen comandos como CREATE TABLE o SELECT ANY TABLE. Todos los usuarios que quieran acceder a la BD deben tener el rol CONNECT. Un usuario con el rol DBA tiene derecho para ver y manejar todos los datos de la BD.

Algunos roles predefinidos en Oracle son: CREATE ROLE programador; SYSDBA, SYSOPER, OSDBA, OSOPER, EXP_FULL_DATABASE, IMP_FULL_DATABASE, SELECT_CATALOG_ROLE, EXECUTE_CATALOG_ROLE, DELETE_CATALOG_ROLE.

**Ejemplo 3.1**
Administrando roles.

```
-- Creando rol
CREATE ROLE RolGerente;

-- Creando rol con password
CREATE ROLE RolGerenteVentas IDENTIFIED BY password;

-- Borrando rol
DROP ROLE RolGerente;
```


**Parámetros para la creación de un perfil:**

<table>
    <thead>
        <tr>
            <td><strong>Roles</strong></td>
            <td><strong>Autorizaciones</strong></td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>SYSOPER</strong></td>
            <td>
            <ul>
            <li>CREATE SPFILE</li>
            <li>ALTER DATABASE OPEN/MOUNT/BACKUP</li>
            <li>ALTER DATABASE ARCHIVELOG</li>
            <li>ALTER DATABASE RECOVER</li>
            </ul>
            </td>
        </tr>
        <tr>
            <td><strong>SYSDBD </strong></td>
            <td>
            <ul>
            <li>STARTUP</li>
            <li> SHUTDOWN</li>
            <li>ALTER DATABASE: open, mount, back up, or change character set</li>
            <li>CREATE DATABASE</li>
            <li>DROP DATABASE</li>
            <li>CREATE SPFILE</li>
            <li>ALTER DATABASE ARCHIVELOG</li>
            <li>ALTER DATABASE RECOVER</li>
            </ul>
            </td>
        </tr>
        <tr>
            <td><strong>OSOPER</strong></td>
            <td>
            <ul>
            <li>STARTUP</li>
            <li>SHUTDOWN</li>
            <li>ALTER DATABASE: open/mount</li>
            <li>ALTER DATABASE BACKUP CONTROLFILE</li>
            <li>ALTER TABLESPACE BEGIN/END BACKUP</li>
            <li>ARCHIVE LOG</li>
            <li>RECOVER</li>
            </ul>
            </td>
        </tr>
        <tr>
        <td><strong>OSDBA</strong></td>
        <td>OSDBA incluye todo lo que tiene el rol OSOPER, igual este usuario tiene más privilegios que otros usuarios.</td>
        </tr>
        <tr>
            <td><strong>EXP_FULL_DATABASE</strong></td>
            <td>
            <ul>
            <li>SELECT ANY TABLE</li>
            <li>BACKUP ANY TABLE</li>
            <li>EXECUTE ANY PROCEDURE</li>
            <li>EXECUTE ANY TYPE</li>
            <li>ADMINISTER RESOURCE MANAGER</li>
            <li>INSERT, DELETE, and UPDATE on the tables SYS.INCVID, SYS.INCFIL, and SYS.INCEXP</li>
            </ul>
            </td>
        </tr>
        <tr>
            <td><strong>MP_FULL_DATABASE </strong></td>
            <td>
            Puede otorgar privilegios a roles y usuarios.
            </td>
        </tr>
        <tr>
            <td><strong>SELECT_CATALOG_ROL </strong></td>
            <td>
            Puede otorgar permisos a los usuarios de SELECT a algunos datos.
            </td>
        </tr>
        <tr>
            <td><strong>EXECUTE_CATALOG_ROLE  </strong></td>
            <td>
              Puede otorgar permisos a los usuarios para ejecutar procedimientos y paquetes.
            </td>
        </tr>
        <tr>
            <td><strong>DELETE_CATALOG_ROLE  </strong></td>
            <td>
              Permite a los usuarios eliminar registros de tablas
            </td>
        </tr>
    </tbody>
</table>

Para asignar privilegios a un rol, tenemos el siguiente formato: 
```
  GRANT <privilegios> ON <nombre_de_función|nombre_de_tabla> TO <nombre_de_rol>;
```

Se vería de la siguiente manera (este ejemplo es sin privilegios predefinidos):
```
GRANT SELECT, INSERT, UPDATE, DELETE
ON customers
TO <nombre_del_rol>;
```

También podemos eliminar privilegios:
```
  REVOKE <privilegio> ON <nombre_de_tabla> FROM <nombre_de_rol>;
```
#### Ejercicio 5
Crear Rol “programador” y asignar los siguientes privilegios:
- Poder conectarse a la base de datos (CONNECT)
- Poder crear recursos (RESOURCE)

Asignar el rol al usuario creado en el ejercicio 4.