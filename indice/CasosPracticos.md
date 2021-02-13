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
 
 <img src=/capturas/vsftpd.PNG width=600px>

### 5.-Acceso al servidor FTP: anónimo tiene solo permisos de lectura en su directorio trabajo







### 6.-Control de Acceso: A la web1 se podrá acceder tanto desde la red externa como interna, a la web2, solo de la interna.

 * Editamos el fichero de configuracion de la web2 añadiendo las siguientes lineas:
   Fichero ----> ```/etc/nginx/sites-avaible/web2```
   
   ```location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ =404;
		allow 192.168.3.0/24;
		deny all;
	}```
 
  <img src=/capturas/ControlAcceso.png width=600px>
  
  * Reiniciamos el servicio
  * Comprobamos
     - Red Externa

 <img src=/capturas/Redexterna.png width=600px>
 
   -  Red Interna

 <img src=/capturas/Redinterna.PNG width=600px>
 
 
 ### 7.-Autentificacion Usuario
 
 **Creamos un directorio web1 que se llame privado y al cual solo pueden acceder usuarios validos**

 * Crearemos el directorio ```/var/www/web1/privado```
 
 ```mkdir privado```

<img src=/capturas/autentificacion.png width=600px>


* Añadimos las siguientes lineas en el fichero de configuracion de web1 ```/etc/nginx/sites-avaible/web1```
 
 
<img src=/capturas/autentificacion2.png width=600px>

* Instalamos el paquete apache2-utils

 ```apt install apache2-utils```
 
 * Creamos las credenciales en el fichero del usuario

 ```cd /etc/nginx```
 ```htpasswd -c -m .htpasswd usuario```
 
 **Nuestro usuario seria, usuario y el archivo que contiene todo es .htpasswd.
 -c ---> crea un fichero
 -m ---> obliga la encriptacion MD5**
 
  <img src=/capturas/autentificacion3.png width=600px>
 
 - Reiniciamos servicio
  ```systemctl restart nginx.service```
 
 * Comprobamos
 
 <img src=/capturas/autentificacion4.png width=600px>
 
### 8.-Red interna no pide autentificacion y externa si

* Añadimos las siguientes lineas al codigo y comprobamos:

<img src=/capturas/autentificacion5.png width=600px>

* Reiniciamos

```systemctl restart nginx.service```
 
 
 
 
 

 

  

