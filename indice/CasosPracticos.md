# Nginx

 
## Casos practicos

*Vamos a mostrar acontinuación los casos practicos por apartados y estaran enlazados a una pagina aparte donde estara los codigos de cada apartado*

### 1.-Version vsftpd Instalada
 
 ```vsftpd -v```


<img src=/capturas/apartadoA.png width=600px>

### 2.-Usuarios creados en instalacion

```getent passwd```

<img src=/capturas/apartadoX.png width=600px>


### 3.-Servicio Asociado
 
 ```systemctl status vsftpd```


<img src=/capturas/serAsociado.png width=600px>


### 3.-Ficheros de configuracion
 
 Los ficheros de configuracion los podemos encontrar en ```/etc```, y el fichero el cual contiene la configuración basica y general es ```vsftpd.conf```


<img src=/capturas/ficheroconf.png width=600px>

* Los parámetros más importantes que debemos descomentar en el servidor FTP son los siguientes:

  - write_enable=YES –> Esta directiva nos permite poder escribir (copiar archivos y carpetas) al servidor FTP.
  -  local_umask=022 –> Esta directiva nos permite habilitar los permisos nuevos cuando copiemos datos al servidro FTP, por defecto el umask es 077 pero podremos modificarlo por el valor que nosotros queramos, 022 es el umask más utilizado en otros servidores FTP.
  -  ftpd_banner –> Esta directiva permite poner un banner de inicio de sesión.
  -  chroot_list_enable=YES –> Nos permite habilitar el chroot de los diferentes usuarios del sistema, para que solamente un usuario entre en su carpeta /home/usuario y en ninguna más, es una medida de seguridad, pero hay que usarla con mucho cuidado ya que si un usuario tiene permisos en directorios superiores tendrá acceso al resto.
  -  chroot_list_enable=YES –> Nos permite crear una lista con los usuarios en chroot, todos los que aparezcan aquí podrán conectarte.
  -  chroot_list_file=/etc/vsftpd.chroot_list –> Es la lista de usuarios con sus rutas predeterminadas.




### 4.-Acceso al servidor ftp: usuarios del sistema

* Primero descomentamos la siguiente linea del ficher ```/etc/vsftpd.conf```

<img src=/capturas/usuarioslocales.png width=600px>
 
* Un usuario enjaulado no puede tener permisos de escritura por lo que debemos modificarlo con el siguiente comando

```chmod a-w -R /home/```

- Despues ponemos ```chmod 777 -R /home/```

- Ponemos la siguiente linea para que pueda escribir
```allow_writeable_chroot=YES```

- reiniciamos
 ```systemctl restart vsftpd```
 
 <img src=/capturas/vsftpd1.PNG width=600px>

### 5.-Acceso al servidor FTP: anónimo tiene solo permisos de lectura en su directorio trabajo

* Activamos el usuario Anonymous en /etc/vsftpd.con

<img src=/capturas/vsftpd2.PNG width=600px>

* Creamos archivo en servidor para poder descargar

```touch /srv/ftp/ejemplo.txt```

* Accedemos mediante ftp y comprobamos que podemos bajarlo

<img src=/capturas/vsftpd3.PNG width=600px>

- Nos deja descargar pero no subir


### 6.-Acceso al servidor FTP: Anonimo tiene permisos de escritura en sugerencias

 * Primero cambiamos el dueño de /srv/ftp y creamos carpeta dentro
 
 ```chown ftp:nogroup /srv/ftp```
 ```mkdir /srv/ftp/sugerencias```
 
 * Cambiamos permisos para que solo pueda escribir
 
 ```chown ftp:nogroup /srv/ftp/sugerencias```
 ```chmod u-w /srv/ftp```
 
 * Configuramos el archivo vsftpd.conf de la siguiente manera: 
 
 <img src=/capturas/vsftpd4.PNG width=600px>
 
 * Comprobamos
 
  <img src=/capturas/vsftpd5.PNG width=600px>
  
  - Se puede subir pero nos deniega al descargar
 
 
 ### 7.-Creación usuarios virtuales
 
 * Instalamos los siguientes paquetes para poder crear usuarios
 
 ```apt install apache2-utils```
 
 * Añadimos las siguientes lineas al archivo de configuracion
 
  <img src=/capturas/vsftpd6.PNG width=600px>
  
 * Creamos la carpeta donde se guardalan la lista de usuarios virtuales y usuarios 
 ```mkdir vsftpd```
 ```httpasswd -cd ./ftpd.passwd user```
 
 * Nos dirigmos al siguiente archivo ```/etc/pam.d/vsftpd``` y escribimos lo siguiente
  
   <img src=/capturas/vsftpd7.PNG width=600px>
   
 * Creamos el usuario virtual
  useradd --home /home/vsftpd --gid nogroup -m --shell /bin/false vsftpd
  
  - Creamos directorio y entramos
  ```mkdir /etc/vsftpd_user_conf```
  ```cd/etc/vsftpd_user_conf```
   
 * Creamos el siguiente archivo de configuracion en ```/etc/vsftpd_user_conf```
   
    <img src=/capturas/vsftpd8.PNG width=600px>
 
 * Creamos la ruta donde podra accedir el usuario y comprobamos
 
 ```cd /srv/ftp```
 ```mkdir user```

### 8.-Seguridad

* Instalamos el paquete para generar certificado

```apt install openssl```

 
* Generamos certificado rellenando los siguientes datos

  <img src=/capturas/seguridad.PNG width=600px>
  
* Configuramos el /etc/vsftpd

<img src=/capturas/seguridad1.PNG width=600px>
 
 * Reiniciamos
 
 ```systemctl restart vsftpd.service```
 
 * Comprobamos
 
 <img src=/capturas/seguridad3.PNG width=600px>
 
 

 

  

