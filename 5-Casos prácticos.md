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

* Para que se ejecute vsftpd en modo independiente.

``` listen=YES ```

* No permitimos que se conecten usuarios anónimos.

``` anonymous_enable=NO ```

* Permitimos que los usuario locales se puedan conectar.

``` local_enable=YES ```

* Permitimos poder hacer modificaciones.

``` write_enable=YES ```

* Registra las conexiones y la información de transferencia, por defecto en /var/log/vsftpd.log

``` xferlog_enable=YES ```

* Se permite que el servidor vsftpd abra el puerto 20, para ponerse a la escucha de peticiones.

``` connect_from_port_20=YES ```

* Mensaje de bienvenida al conectarse mediante un cliente ftp

``` ftpd_banner=Bienvenidos al ftp de Redes de Area Local. ```

* Permitimos a los usuarios locales que puedan salir de su directorio.

``` chroot_local_user=NO ```

* Los usuarios locales que se encuentren en el fichero indicado por chroot_list_file estarán enjaulados en su directorio.

``` chroot_list_enable=YES ```

* Especifica el fichero que contiene los usuarios a enjaular.

``` chroot_list_file=/etc/vsftpd.chroot_list ```

* Esta opcion especifica el nombre de un directorio vacio. El directorio no tiene que tener privilegios para el usuario de ftp.Es un directorio usado como una jaula segura chroot.

``` secure_chroot_dir=/var/run/vsftpd ```

* Localización del certificado RSA para usar conexiones SSL. Esta opción viene por defecto.

``` rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem ```

* Esta opción especifica la localización de la clave privada para las conexiones SSL.

``` rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key ```

# Acceso al servidor FTP: usuarios del sistema
Creamos los directorios:

```mkdir /home/cristian ```

```mkdir /home/cristian/ftp```

Creo el usuario pertenciendo al grupo ftp que se crea con la instalación.

``` useradd -g ftp -d /home/cristian/ftp -c cristian cristian ```

* -g ftp : grupo al que pertenece.
* -d /home/cristian/ftp : directorio.
* -c cristian: el nombre completo del usuario.
* cristian: nombre de usuario.

Y le damos una contraseña al usuario.

``` passwd cristian ```

## Enjaular al usuario.

Copio la línea del usuario creado en ``` /etc/passwd ```

En mi caso queda así.

``` cristian:x:1001:126:cristian:/home/cristian/ftp:/bin/sh ```

Luego en el archivo ``` /etc/vsftpd.conf ``` tenemos que habilitar la línea de:

```LISTEN=YES```

```LISTEN_ipv6=NO```

``` chroot_local_user=YES ```

``` chroot_list_enable=YES ```

``` chroot_list_file=/etc/vsftpd.chroot_list ```

Luego pegamos la línea del usuario en el archivo ``` /etc/vsftpd.chroot_list ```, el cual estará vacío.

Y reiniciamos el servicio.

Para comprobar que funciona, creo un archivo llamado hola en el directorio ``` /home/cristian/ftp ```

Y accedo al servidor FTP por FileZilla:

**Añadir imagen 3**

# Acceso al servidor FTP: anónimo tiene solo permiso de lectura en su directorio de trabajo

Editamos el fichero ``` /etc/vsftpd.conf ```.

Cambiamos el valor de ``` anonymous_enable=NO ``` a ``` YES ```.

También podemos indicar que no necesiten contraseña para acceder con la siguiente directiva:

``` no_anon_password=YES ```

Creo 2 archivos en el directorio.

**Añadir imagen 4**

Reinicio el servicio de vsftp

``` systemctl restart vsftpd.service ```

Nos conectamos y nos deja descargar archivos:

**Añadir imagen 5**

Pero no deja subir:

**Añadir imagen 6**

# Acceso al servidor FTP: anónimo tiene permiso de escritura en el directorio sugerencias, que es un subdirectorio de su directorio raíz.

Primero, le damos permisos al usuario ftp al directorio ftp.

**Añadir imagen 7**

Creamos un directorio dentro de ftp llamado sugerencias.

**Añadir imagen 8**

Le damos permisos al usuario ftp sobre el directorio que hemos creado.

**Añadir imagen 9**

Y le quitamos los permisos de escritura sobre ftp.

**Añadir imagen 10**

Activamos las siguientes directivas:

``` anon_other_write_enable=YES ``` 

```write_enable=YES ```

```anon_upload_enable=YES ```

```chroot_list_enable=NO ```

Reiniciamos el servicio.

```systemctl restart vsftpd.service```

Nos deja subir archivos:

**Añadir imagen 11**

Pero no bajar:

**Añadir imagen 12**

En / tampoco nos deja subir nada:

**Añadir imagen 13**

# Acceso al servidor FTP: Creación de usuarios virtuales

Primero instalamos el siguiente paquete.

``` apt install libpam-pwdfile ```

Luego hacemos que el archivo de configuración quede de la siguiente forma:

**Añadir imagen 14**

Y dentro de una carpeta, creamos el archivo ftpd.passwd de la siguiente manera.

**Añadir imagen 15**

Me dirijo a /etc/pam.d y configuro el siguiente archivo. Solo dejamos esas líneas.

**Añadir imagen 16**

Creo el usuario virtual de la siguiente manera:

``` useradd --home /home/vsftpd --gid nogroup -m --shell /bin/false vsftpd ```

Creo el directorio y entro.

**Añadir imagen 17**

Creo el archivo para user1 y dentro escribo lo siguiente.

``` local_root=/srv/ftp/user1 ```

Creo la ruta donde podrá acceder el usuario.

**Añadir imagen 18**

Reinicio el servicio de vsftpd e intento acceder desde FileZilla.

# Acceso seguro a FTP

Primero instalo openssl para generar los certificados y claves.

``` apt install openssl ```

Generamos el certificado.

**Añadir imagen 19**

Configuramos las siguientes líneas del archivo /etc/vsftpd.conf:

```    rsa_cert_file=/etc/ssl/private/vsftpd.pem ```
```    rsa_private_key_file=/etc/ssl/private/vsftpd.pem ```
```    ssl_enable=yes ```

Reiniciamos el servicio.

Al intentar conectar desde FileZilla, nos sale el siguiente mensaje.

**Añadir imagen 20**
