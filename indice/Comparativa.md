# Comparativa con Proftpd

## Proftpd

  ProFTPd es un servidor FTP. Se promociona desde su página web como estable y seguro, cuando se configura correctamente. El servidor ProFTPd se promociona a sí mismo como un "Software servidor FTP altamente configurable con licencia GPL".

## Diferencias entre ellos

### Principales caracteristicas Proftpd:


- Tiene un archivo de configuración principal, que contiene directivas y grupos de directivas que son muy intuitivas si has utilizado el servidor web Apache, ya que se basaron en él para crear Proftpd.

- Dispone de un directorio llamado «.Ftpaccess» que es similar a «.htaccess» de Apache.

- Es muy fácil configurar múltiples servidores FTP virtuales y servicios FTP anónimos.

- Diseñado para ejecutarse como un servidor independiente o desde inetd / xinetd, dependiendo de la carga del sistema.

- Dispone de directorios raíz anónimos que no requieren ninguna estructura de directorio, archivos binarios del sistema u otros archivos del sistema.

 - No hay comando SITE EXEC,evitando así, los problemas que puede acarrear en seguridad.
 
 ### Principales caracteristicas vsftpd:

- Dispone de configuraciones de IP virtual

- Puedes crear usuarios virtuales

- Puede funcionar en modo operación independiente o inetd.

- Las opciones de configuración por parte del usuario son muy avanzadas.

- Dispone de un acelerador de ancho de banda para que funcionen las cargas y descargas aún mejor.

- Puedes establecer límites por IP.

- Es compatible con IPv6.
