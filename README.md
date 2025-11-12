# qradarCE-101-español
Repositorio explicativo donde se aborda toda la interfaz de qradarCE 7.5 junto a todo su menú desde la descarga e instalación hasta la posterior utilización de toda la GUI.

QradarCE es la versión dedicada a la comunidad (community edition) de la plataforma de seguridad SIEM de IBM. Esta fue creada para tener una forma gratuita de desarrollar y probar las capacidades del sistema. Por obvias razones qradarCE esta limita en comparación a la versión Enterprise SIEM, aun así qradarCE permite tener un vistazo completo a casi todo lo que puede ofrecer el sistema y en eso se va a basar este repositorio.

### indice

* [Descarga, Prerrequisitos, Instalación y Ejecución](#Descarga-Prerrequisitos-Instalación-Ejecución)
  * [Prerrequisitos](#Prerrequisitos)
  * [Instalación](#Instalación)
* [Configuración](#Configuración)
  * [Licencia](#Licencia)
* [Qrdar UI](#Qrdar-UI)
* [Admin](#Admin)

# Descarga-Prerrequisitos-Instalación-Ejecución
La descarga de la versión más reciente de IBM qradarCE se hace a través de la página principal de IBM, para esto primero es necesario crearse una cuenta en la plataforma para poder descargar el sistema. Una vez la cuenta esté creada hay que dirigirse a la página de [IBM Security QRadar](https://www.ibm.com/community/101/qradar/ce) y descargarlo desde el botón azul.
Una vez en la página se debe descargar el archivos `.ISO` más el archivo para poder extender la licencia, el cual finaliza con la extensión `.key`.
Ya con esos dos archivos descargados se puede pasar a los prerrequisitos y la instalación.

### Prerrequisitos
QradarCE, al ser un SIEM todo en uno, tiene unos requisitos muy demandantes para una virtualización. Según la página oficial, los requisitos **mínimos** deberían de ser los siguientes:
- 24 GB de RAM
- 250 GB de espacio en disco
- 4-6 núcleos de procesador

</br>

Estos requisitos pueden ser muy abrumantes para una virtualización, pero es posible acotarlos y mantener el rendimiento de la VM. Estos requisitos son los que **YO** considero mínimos para una utilización de qradar básica sin llevar a los límites el sistema:
- 8-12 GB de RAM
- 100-150 GB de espacio en disco
- 4-6 núcleos de procesador

</br>

Estos requisitos son mucho más asequibles para el hardware. Aun así, si se dispone del hardware adecuado para cumplirlos, se recomienda seguir los que ofrece la página oficial.

### Instalación
Una vez definido los requisitos se puede proceder con la instalación. Para eso se debe elegir el virtualizado de preferencia, para esta guía se utilizó VMWare, pero es posible con cualquier virtualizador. 
QradarCE requiere configuración previa por parte del virtualizador, además de los requisitos previos, para que la instalación no tenga ningún fallo:
- Configurar el HardDisk en modo `SATA`, ya que la configuración default que utilizada discos virtualizados pueden generar errores.
- El adaptador de red debe estar en modo **"Puente"**, ya que qradar necesita una IP fija para poder comunicarse con los ortos dispositivos de la red.

</br>

Ya con todos los requisitos listos se puede iniciar con la instalación, esta toma aproximadamente unos 40-60 mins, ya que no es necesario preinstalar un sistema operativo previamente. QradarCE en la versión 7.5 y todo el SIEM está levantado sobre RedHat 8.10 como sistema operativo.

Durante la descarga, por lo que se mencionó anteriormente, la instalación se reinicia 3 veces. Aunque no lo parezca, es una buena señal porque el instalador necesita aplicar cambio, además de descargar el sistema operativo y el propio SIEM. 

 Llegado a un punto del tercer reinicio se va a solicitar un `LocalHost login:` donde hay que iniciar sesión con el usuario `root`. Pasado unos segundos después de ingresar con el usuario root aparecerá una página de términos y condiciones los cuales se deben aceptar.

Cuando se aceptan los términos y condiciones se va a visualizar el “appliance install”, en esta es necesario elegir la segunda opción, siendo esta la de `software install`, en la siguiente pestaña elegir `"all-in-one" console`, la tercer pestaña se deja en Normal setup, en la cuarta se elige la fecha, en la quinta y sexta se elige la región, en la séptima pestaña se deja la opción IPv4, en la octava se selecciona la interfaz (debería aparecer una sola), en la novena pestaña se debe fijar en que dirección se va a alijar la interfaz web, para esto, se debe elegir un hostname a nivel local para poder ingresar (Como `qradar.local.com` o `qradar.localhost`), la `IP Address` va a ser la IP fija que se va a utilizar, tené en cuenta que tiene que estar dentro del rango de IP de tu red local, luego llenar la máscara de sub-red, el gateway y un DNS primario. Con todos estos datos ya se puede dar a siguiente.

Después de que qradar finalicé la configuración de la network, te pedirá una contraseña para el usuario admin, esta tiene que tener como mínimo 5 caracteres **no especiales**. Con el usuario admin configurado el sistema tiene que hacer las últimas configuraciones (las cuales toma aproximadamente 10-15 min). 

Una vez finalizada la configuración te dará acceso a la CLI del usuario `root`, en esta para verificar si se instaló correctamente se puede utilziar el siguiente comando:

```bash
/opt/qradar/bin/myver -v
```

Este comando mostrará información de la versión instalad, si esta coincide con la del archivo .iso, la instalación se puede dar por finalizada.

## Configuración

Una vez terminada la instalación, es momento de ingresar a la interfaz web de qrdadarCE desde cualquier endpoint de la red interna. Esta interfaz se va a encontrar ingresando la dirección IPv4 (configurada anteriormente) en el buscador de preferencia, cuando se intente ingresar por primera vez, el buscador te va a informar que el certificado **SSL** no está validado y el buscador te va a avisar que la página puede no ser segura, aun así, debajo elija la opción de “ingresar igualmente”. 

Esto se debe a que qradar genera un certificado SSL propio para poder utilizar TLS dentro de la red local y el buscador te da un aviso de que la página no tiene una certificación SSL emitida por una agencia certificadora.

Dentro de la página de login se te va a solicitar que ingreses un usuario y contraseña, aquí se debe ingresar con el usuario `admin` y la contraseña debe ser la ingresada en la que se configuró en la instalación.

Luego de darle a login, qradar solicitara que cambies nuevamente la contraseña, esta contraseña va a ser la contraseña definitiva para el usuario admin. La siguiente pestaña que aparece es el license agreement, el cual hay que aceptar.

Una vez dentro te vas a encontrar con el **dashboard** junto con un mensaje que informa que la licencia temporal expira al cabo de unos días.

### Licencia

Para poder expandir la licencia hay que dirigirse a la pestaña “admin” y seleccionar la opción “system and license management”, dentro de esa pestaña va a aparecer la única licencia activa junto a la versión de qradar y la fecha de expiración. 

Una vez dentro, seleccione la opción “upload license” en la parte superior izquierda, dentro selecciona un archivo, este tiene que ser el archivo `.key` descargado anteriormente y luego darle a “upload file”.

Luego de subir el archivo hay que dirigirse al  centro de la pestaña y en el desplegable “displey” seleccionar la opción “licenses”. Ahora en la licencia va a aparecer un símbolo de más`(+)` en la parte derecha de la licencia que al darle clic se va a expandir mostrando todas las licencias junto a la que se agregó con un texto “license status: Unallocated”.

una vez vista la licencia sin asignar, hay que seleccionarla con clic derecho y darle al botón de “allocate system to license”.

Luego de asignar la licencia es necesario desplegarla, para esto vuelve a la psetaña de admin y donde aparece el cartel amarillo informando que hay opciones por implementar, expande los detalles para ver más información y luego dale al botón de “deply changes”. Después de que se apliquen los cambios hay que volver “system and license management” para modificar los pools de la licencia.

Al ser una licencia de la comunidad, está limitada en la cantidad de eventos por minuto y el volumen de datos por minutos y como el sistema no identifica que licencia se aplica en el sistema es necesario modificarlos manualmente para que la licencia no se bloquee.

Para esto dentro del panel de system and license management hay que cliquear la licencia que se activó anteriormente y dirigirse a “license pool management”, aquí aparecerá en rojo mostrando que está excedido del límite de la licencia. Para poder modificarlo, selecciona la licencia que está debajo dele gráfico y en la opción “allocated EPS” cámbialo a 100 y en “allocated FPM” a 5000, y darle a SAVE, con eso en el gráfico van a aparecer en ambos 0, esto significa que se configuró.

Para verificar que está bien configurado, diríjase a “log activity”, si esta pestaña aparecen logs significa que se configuro satisfactoriamente.

# Qrdar UI

### Dashboard

El dashboard de qradar es la primera pestaña que aparece y la más sencilla de configurar. Proporciona un entorno de trabajo con información resumida y detallada sobre el estado de la seguridad de la red. Esta viene con 7 pre sets, cada una con una temática diferente (Network, application, risk, etc.). También existe la posibilidad de crear tu propio dashboard y configurarlo a tu manera, con la posibilidad de borrar y renombrar el dashboard.

El mayor juego que tiene esta pestaña es la opción “add item” donde se pueden modificar los pre sets creados, en estos se puede agregar todo tipo de gráficos preseteados como Gráficos, diagramas, listas y monitores personalizables que muestran métricas clave.

Dato no menor, el idioma de UI de Qradar se ajusta al idioma que tenga de forma predeterminada el navegador que se utilize.

### Offenses

La pestaña Offenses muestra las alertas de seguridad generadas por el motor de correlación de QRadar cuando una regla detecta una actividad sospechosa o maliciosa. En Offenses se puede visualizar los incidentes que tomo tu usuario, como, todos los incidentes. Además de tener la posibilidad de filtrarlos por IP de origen o de destino, por red o por regla. 

Qradar integra este sistema de tiketing que permite investigar, asignar, comentar, cerrar y priorizar las alertas de seguridad. Cada "delito" es una colección de eventos y flujos relacionados.

### Log Activity

La pestaña Log Activity permite ver y buscar en tiempo real los logs que se envían a QRadar desde todos los orígenes de datos como: firewalls, servidores, aplicaciones, etc.

Esta es una lista de eventos brutos pero Qradar ofrece un potente motor de búsqueda y filtrado para investigar eventos específicos o patrones de eventos Incluyendo gráficos de series temporales configurables.

### Network Activity

La péstaña Network Activity es muy similar a **Log Activity**, pero se centra en los flujos de red. En esta pestaña se lista el flujos de red, permitiendo investigar el tráfico de red, buscar conexiones específicas, volúmenes de datos, protocolos utilizados y otra información de sesión.

### Assets

La pestaña Assets descubre automaticamente y mantiene una base de datos de los activos de la red local. Dentro de cada activo hay información detallada, incluyendo su sistema operativo, servicios y la actividad de red y eventos asociados.

### Reports

La pestaña de reportes sirve para la creación y gestión de informes**.** Se utiliza para generar, distribuir y gestionar informes periódicos sobre los datos en QRadar, estos dependiendo de la configuracion se generan diariamente o mensualmente.

La pestaña tiene un listado de informes y informes generados previamente, ademas de opciones para crear informes personalizados.

### Risk

Esta pestaña es un modulo que actua por separado,  el modulo “Risk Manager” que en Qradar la pestaña es llamada “risk” se utilzia para modelar, priorizar y gestionar los riesgos de seguridad y cumplimiento.

Este modulo es una herramienta para supervisar los dispositivos, simular cambios en la red, visualizar la topología y evaluar el riesgo. Esta pestaña es utilizada para hacer cumplir las politicas de seguridad. 

Este modulo ni viene instalado de forma predeterminada en Qradar.

### Vulnerabilities

La pestaña vulneravilities tambien es un modulo este llamado “QRadar Vulnerability Manager”, que, se enfoca en identificar y priorizar las vulnerabilidades en los activos.

Este es un sistema semiautomatico que explora la red para identificar vulnerabilidades en aplicaciones, sistemas y dispositivos, tambien, integra datos de escáneres de terceros.

### **Log Sources**

La pestaña log source es una de las mas importantes ya que en esta se agregan todos los assets y endpoints de los cuales salen los logs visualizados en log activity.
Log source permite elegir que tipo de “source” entre sistemas operativos y diferentes opcciones de de servicios.

Esta pestaña es una extención que se agrega desde “Extensions Management” en la pestaña Admin.

## Admin

La pestaña admin es donde se realizan las tareas de mantenimiento y gestion de qradar. Gran parte de toda la configuracion del SIEM viene de esta pestaña como: La configuración de orígenes de registro (Log Sources), gestión de usuarios y roles de seguridad, configuración del sistema, gestión de licencias, ajustes de red, copias de seguridad, y despliegue de cambios, entre otras muchas cosas. Dentro de esta pestaña tambien aparecen todas aquellas extensiones que se agreguen.

Dentro de esta pestaña se divide en diferentes secciones para la configuracion.

| Sección  | Función Principal  |  Funciónes que trae |
|----------|--------------------|---------------------|
| System Configuration | Gestión de la salud, rendimiento y mantenimiento de la Consola de QRadar. | Auto Update, Backup and Recovery, Index Management, Aggregated Data Management, Custom Offense Close Reasons, Store and Forward, Reference Set Management, Centralized Credentials, Email Server Management. |
| User Management | Control de acceso, roles y permisos de los usuarios de QRadar. | Users, User Roles, Security Profiles, Authentication, Autorized services, Tenant management. |
| Forensics | Configuración de las herramientas para la recolección y el análisis posterior a un incidente. | Server Management, Case management, Forensic user permissions, Schedule actions, Suspect content management. |
| Assets | Configuración de cómo QRadar mantiene los activos de la red. | Custom Asset Properties, Asset Profiler Configuration, Manage identify exclusion |
| Data Sources | Configuración y ajuste de los módulos que QRadar que se usa para recibir y analizar los datos de los logs. | --- |
| Events | Configuración del soporte para los logs. | DSM Editor, WinCollect, Log source(Extensions, Group, Parsing Ordering), Custom event properties, Event retention y Data obfuscation management |
| Flows | Configuración del soporte para los flujos de red. | Flow sources, Flow sources aliases, Custom flow propierties, Flow retention |
| Custom Actions | Definición de acciones de respuesta automatizadas que QRadar puede tomar cuando se activa una regla. | Define Actions (Crear scripts o llamadas a APIs) |
| Vulnerability | Configuración de los escáneres que detectan y reportan vulnerabilidades de la red. | VA Scanners y Schedule VA Scanners (Una sirve par configurar escáneres y la otra para programar ejecuciones periódicas) |
| Dynamic Search | Configuración de la funcionalidad de búsqueda dinámica avanzada, que permite búsquedas personalizadas sobre el contexto y metadatos de los datos. | Como su nombre indica funciona para la configuración de parámetros y vistas para la búsqueda dinámica |
| Remote Networks and Services Configuration | Definición de las redes y los servicios externos a la red principal para que QRadar los clasifique correctamente. | Esta seccion crear y gestionar las definiciones de redes externas y servicios conocidos. |
| Apps | Funcionalidades de importación relacionadas con los módulo importados | En esta seccion aparecen las extensiones de las funciones importadas. |

# Casos de uso

### Integración de log source para la ingesta de Datos de windows

Uno de los primeros casos de uso es integrar datos de un endpoint windows, qradar lo permite hacer de muchas maneras diferentes, pero en este caso solo utilizaremos una de las más sencillas que es el uso de un software de la suite de IBM para conectar el endpoint al SIEM.

Este software que se va a utilizar se llama WinCollect. Este se va a encargar de mandar los logs elegidos a qradar mediante una conexión TCP.

El primer paso para esto es necesario [descargarlo](https://www-ibm-com.translate.goog/community/101/qradar/wincollect10/?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=tc) desde la página principal, allí se va a elegir la arquitectura del sistema operativo y se va a descargar. Una vez descargado se va a instalar como administrador.

En la pestaña de instalación hay que aceptar los términos y condiciones, después elegir la opción de instalación rápida, en la pestaña donde se pide el hostname de Qradar hay que poner la dirección IP donde está alojado el SIEM.

Mientras se instala aparecerá una pestaña que pregunta si estás de acuerdo con que Wincollect pueda hacer cambios en el equipo, en esta pestaña hay que darle en el botón de aceptar y esperar a que se termine de instalar.

Una vez finalizado se puede ingresar a la consola usando la URL “localhost:3000”. En este lugar se van a crear los sources y conectar y verificar la conexión con Qradar.

 Dentro de la consola lo primero que hay es necesario hacer es verificar la conexión con Qradar, para esto hay que dirigirse a la pestaña “destinations” que se encuentra en la parte superior izquierda dentro del menú desplegable.

 Para esto hay que crear una destinación (si es que no hay) con la dirección IP de Qradar y utilizar un port 514 por el protocolo TCP, y, luego de configurarlo, utilizar el botón “test” y verificar la conexión.

Con la conexión realizada crear un Source con el botón azul en la parte superior. Aquí elegiremos una source local, en la sección “Group Source” vas a crear un nuevo grupo con el nombre que quieras, luego se van a elegir los logs a enviar que en este caso serias los de “Windows events”, y en este se van a seleccionar los siguientes 3 logtypes: application, system y security.

Luego elegir el “destination” configurado anteriormente y por último aplicar los cambios y confirmar.

Con esto los logs elegidos deberían estar llegando a Qradar directamente y los verás en la pestaña de “log activity”.

### Integración de log source para la ingesta de Datos de windows

La integración de los de Linux es un poco más sencillo. Para esto es necesario modificar el archivo rsyslog.conf para enviar todos los logs del sistema a Qradar. Para modificar el archivo hay que entrar directamente al sistema linux o directamente por ssh, luego modificar el archivo usando el comando `sudo nano /etc/rsyslog.conf`, luego en el final del archivo hay que ingresar la sintaxis “`*.* @<IP.DE.QRADAR>:514`”, luego guardar, salir y reiniciar el servicio rsyslog con el comando `systemctl restart rsyslog`. Con esto es necesario esperar aproximadamnetnte 15 minutos para que Qradar pueda detectar los logs de Linux.

Pasado un tiempo, si Qradar no detecta los logs de Linux es posible agregarlos usando los Logs Source. Para esto hay que ingresar un “simple log” y buscar Linux OS y luego Syslog. Aquí hay que rellenar las secciones obligatorias y luego ingresar la IP o el dominio de Endpoint donde está el sistema operativo Linux. 

Para poder verificar la llegada de logs es posible enviar un log de pureva desde el sistema operativo utilizando el comando “logger test”.


