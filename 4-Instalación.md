<h1><p align=center> Instalación </p></h1>
Antes de nada, lo primero será actualizar la lista de paquetes.

``` apt update ```

Después de esto instalamos el paquete de vsftp.

``` apt -y install vsftpd ```

Ahora comprobamos que el servicio está iniciado y funciona correctamente.

``` systemctl status vsftpd.service ```