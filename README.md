# qradarCE-101-español
Repositorio explicativo donde se aborda toda la interfaz de qradarCE 7.5 junto a todo su menú desde la descarga e instalación hasta la posterior utilización de toda la GUI.

QradarCE es la versión dedicada a la comunidad (community edition) de la plataforma de seguridad SIEM de IBM. Esta fue creada para tener una forma gratuita de desarrollar y probar las capacidades del sistema. Por obvias razones qradarCE esta limita en comparación a la versión Enterprise SIEM, aun así qradarCE permite tener un vistazo completo a casi todo lo que puede ofrecer el sistema y en eso se va a basar este repositorio.

### indice

* [Descarga, Prerrequisitos, Instalación y Ejecución](#Descarga-Prerrequisitos-Instalación-Ejecución)
  * [Prerrequisitos](#Prerrequisitos)
  * [Instalación](#Instalación)
* [Configuración](#Configuración)

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
