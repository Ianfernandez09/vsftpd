<h1><p align=center> Casos Prácticos </p></h1>

# Versión de Vsftpd Instalado

**Añadir imagen 1**

# Usuarios creados en la instalación

**Añadir imagen 2**

# Servicio asociado

``` systemctl status vsftpd.service ```

# Ficheros de configuración

Vsftpd usa 2 ficheros de configuración y son:

* /etc/vsftpd.conf -> Fichero de configuración del servidor vsftpd.

Para que se ejecute vsftpd en modo independiente.

``` listen=YES ```

No permitimos que se conecten usuarios anónimos.

``` anonymous_enable=NO ```

Permitimos que los usuario locales se puedan conectar.

``` local_enable=YES ```

Permitimos poder hacer modificaciones.

``` write_enable=YES ```

Registra las conexiones y la información de transferencia, por defecto en /var/log/vsftpd.log

``` xferlog_enable=YES ```

Se permite que el servidor vsftpd abra el puerto 20, para ponerse a la escucha de peticiones.

``` connect_from_port_20=YES ```

Mensaje de bienvenida al conectarse mediante un cliente ftp

``` ftpd_banner=Bienvenidos al ftp de Redes de Area Local. ```

Permitimos a los usuarios locales que puedan salir de su directorio.

``` chroot_local_user=NO ```

Los usuarios locales que se encuentren en el fichero indicado por chroot_list_file estarán enjaulados en su directorio.

``` chroot_list_enable=YES ```

Especifica el fichero que contiene los usuarios a enjaular.

``` chroot_list_file=/etc/vsftpd.chroot_list ```

Esta opcion especifica el nombre de un directorio vacio. El directorio no tiene que tener privilegios para el usuario de ftp.Es un directorio usado como una jaula segura chroot.

``` secure_chroot_dir=/var/run/vsftpd ```

Localización del certificado RSA para usar conexiones SSL. Esta opción viene por defecto.

``` rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem ```

Esta opción especifica la localización de la clave privada para las conexiones SSL.

``` rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key ```

# Acceso al servidor FTP: usuarios del sistema

## Creo el usuario

Primero creo un directorio dentro de ftp para el usuario que va a acceder.

``` mkdir /srv/ftp/cristian ```

Le doy permisos.

``` chmod -R 777 /srv/ftp/cristian ```

Luego creo el usuario pertenciendo al grupo ftp que se crea con la instalación.

``` useradd -g ftp -d /home/cristian/ftp -c cristian cristian ```

* -g ftp : grupo al que pertenece.
* -d /srv/ftp/cristian : directorio.
* -c cristian: el nombre completo del usuario.
* cristian: nombre de usuario.

Y le damos una contraseña al usuario.

``` passwd cristian ```

## Enjaular al usuario.

Copio la línea del usuario creado en ``` /etc/passwd ```

En mi caso queda así.

``` cristian:x:1001:126:cristian:/home/cristian/ftp:/bin/sh ```

Luego en el archivo ``` /etc/vsftpd.conf ``` tenemos que habilitar la línea de:

``` chroot_local_user=YES ```

``` chroot_list_enable=YES ```

``` chroot_list_file=/etc/vsftpd.chroot_list ```

Luego pegamos la línea del usuario en el archivo ``` /etc/vsftpd.chroot_list ```, el cual estará vacío.

Y reiniciamos el servicio.

Para comprobar que funciona, creo un archivo llamado hola en el directorio ``` /home/cristian/ftp ```

Y accedo al servidor FTP por navegador, ``` ftp://192.168.0.55 ``` y me sale lo siguiente:

**Añadir imagen 3**+

O por consola:

**Añadir imagen 4**

# Acceso al servidor FTP: anónimo tiene solo permiso de lectura en su directorio de trabajo

Primero creo un directorio dentro de ``` /srv/ftp ``` llamado pub.

``` mkdir /srv/ftp/pub ```

Editamos el fichero ``` /etc/vsftpd.conf ```.

Cambiamos el valor de ``` anonymous_enable=NO ``` a ``` YES ```.

También podemos indicar que no necesiten contraseña para acceder con la siguiente directiva:

``` no_anon_password=YES ```

Compruebo que puedo conectarme.

**Añadir imagen 5**

Para hacer que los usuarios anónimos tengan solo permiso de lectura, debemos poner la siguiente directiva en el archivo de configuración.

``` anon_world_readable_only=YES ```

Por lo que, por ejemplo si queremos crear un directorio como anonymous, no tendremos permiso.

**Añadir imagen 6**

