# qradarCE-101-español
Repositorio explicativo donde se aborda toda la interfaz de qradarCE 7.5 junto a todo su menú desde la descarga e instalación hasta la posterior utilización de toda la GUI.

QradarCE es la versión dedicada a la comunidad (community edition) de la plataforma de seguridad SIEM de IBM. Esta fue creada para tener una forma gratuita de desarrollar y probar las capacidades del sistema. Por obvias razones qradarCE esta limita en comparación a la versión Enterprise SIEM, aun así qradarCE permite tener un vistazo completo a casi todo lo que puede ofrecer el sistema y en eso se va a basar este repositorio.

# Descarga, Prerrequisitos, Instalación y Ejecución
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
- 200 GB de espacio en disco
- 4-6 núcleos de procesador

</br>

Estos requisitos son mucho más asequibles para el hardware. Aun así, si se dispone del hardware adecuado para cumplirlos, se recomienda seguir los que ofrece la página oficial.

### Instalación
Una vez definido los requisitos se puede proceder con la instalación. Para eso se debe elegir el virtualizado de preferencia, para esta guía se utilizó VMWare, pero es posible con cualquier virtualizador. 
QradarCE requiere configuración previa por parte del virtualizador, además de los requisitos previos, para que la instalación no tenga ningún fallo:
- Configurar el HardDisk en modo `SATA`, ya que la configuración default que utilizada discos virtualizados pueden generar errores.
- El adaptador de red debe estar en modo **"Puente"**, ya que qradar necesita una IP fija para poder comunicarse con los ortos dispositivos de la red.

</br>

Ya con todos los requisito listos se puede iniciar con la instalación
