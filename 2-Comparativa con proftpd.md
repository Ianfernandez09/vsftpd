<h1><p align=center>VSFTP vs PROTCP</p></h1>
<h2>Proftp</h2>
* Tiene un archivo de configuración principal, que contiene directivas y grupos de directivas que son muy intuitivas si has utilizado el servidor web Apache, ya que se basaron en él para crear Proftpd.
* Dispone de un directorio llamado «.Ftpaccess» que es similar a «.htaccess» de Apache.
* Es muy fácil configurar múltiples servidores FTP virtuales y servicios FTP anónimos.
* Diseñado para ejecutarse como un servidor independiente o desde inetd / xinetd, dependiendo de la carga del sistema.
* Dispone de directorios raíz anónimos que no requieren ninguna estructura de directorio, archivos binarios del sistema u otros archivos del sistema.
* No hay comando SITE EXEC,evitando así, los problemas que puede acarrear en seguridad.
* Los directorios y archivos ocultos están basados ​​en permisos de estilo Unix.
* Dispone de un servicio para administrar contraseñas ocultas y soporte para cuentas caducadas.
* Está diseñado de forma modular, eso quiere decir que permite ampliar fácilmente el servidor con módulos como por ejemplo módulos para bases de datos SQL, servidores LDAP, encriptación SSL / TLS, soporte RADIUS, etc.
<h2>Vsftp</h2>
* Dispone de configuraciones de IP virtual
* Puedes crear usuarios virtuales
* Puede funcionar en modo operación independiente o inetd.
* Las opciones de configuración por parte del usuario son muy avanzadas.
* Dispone de un acelerador de ancho de banda para que funcionen las cargas y descargas aún mejor.
* Puedes establecer límites por IP.
* Tiene soporte de cifrado a través de la integración con SSL.