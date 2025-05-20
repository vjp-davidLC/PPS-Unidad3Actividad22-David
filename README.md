# PPS-Unidad3Actividad22-David
###  Detectar intentos de ataque en los logs

Como estamos trabajando con un escenario multicontenedor docker, hay algunas peculiaridades. Por ejemplo, parte de los logs se guardan en el archivo other_vhosts_access.log en vez de access.log. Si las consultas no dan resultados prueba con access.log.

##### Buscar accesos a la página de administración

Detectar intentos de acceso a rutas sensibles como /admin:

![img1](img/img1.PNG)

![img1](img/img2.PNG)

##### Contar cuántas veces una IP ha intentado acceder

Para ver si una IP está haciendo muchas peticiones sospechosas:

![img1](img/img3.PNG)

##### Ver actividad en tiempo real y filtrar por palabra clave

Monitorear en vivo cualquier intento de acceso a /wp-admin (Página de administración de WordPress por defecto):

![img1](img/img4.PNG)

### Configurar alertas automáticas con Fail2Ban

Configuramos el archivo de configuración de fail2ban:

![img1](img/img5.PNG)

Esto bloqueará una IP por 1 hora si realiza más de 5 intentos fallidos.

### Configuración de Fail2Ban en un entorno LAMP con Docker

docker-compose.yml -->

![img1](img/img6.PNG)

Cremos las carpetas necesarias si no existen:

![img1](img/img7.PNG)

Tendremos que crear un dockerfile con las variables necesarias para el docker-compose.

![img1](img/img8.PNG)

Y por último configuramos el fail2ban:

![img1](img/img10.PNG)
![img1](img/img11.PNG)

Una vez levantado el docker.comose comprobamos el log de fail2ban.

![img1](img/img9.PNG)

Realizamos un ejemplo baneando una IP.

![img1](img/img13.PNG)
![img1](img/img14.PNG)

### Usar herramientas de SIEM (Security Information and Event Management)

#####  Agregar el repositorio de Elastic

![img1](img/img15.PNG)

![img1](img/img16.PNG)

Tendremos que configurar el fichero de /etc/elasticsearch/elasticsearch.yml.

``` # ======================== Elasticsearch Configuration =========================
# ----------------------------------- Paths ------------------------------------
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
# ---------------------------------- Network -----------------------------------
network.host: 0.0.0.0
http.port: 9200
# ---------------------------------- Security ----------------------------------
xpack.security.enabled: false
xpack.security.http.ssl.enabled: false
xpack.security.transport.ssl.enabled: false 
```

Verificar si Elasticsearch está funcionando.

![img1](img/img17.PNG)

##### Configurar Logstash para Apache

Añadir la configuración para procesar logs de Apache

![img1](img/img18.PNG)

Verificar que Logstash está procesando los logs

![img1](img/img19.PNG)

##### Configurar Kibana
Acceder a Kibana desde el navegador: http://localhost:5601
![img1](img/img20.PNG)

##### Instalar y configurar Filebeat para Apache
Instalar Filebeat.

![img1](img/igm20.PNG)


Editar la configuración de Filebeat

![img1](img/img21.PNG)

Verificar que Filebeat está enviando logs.

![img1](img/img22.PNG)

Verificar que los logs de Apache están en Kibana.

![img1](img/img23.PNG)

Si todo está configurado correctamente ir a la sección Analytics → Discover, buscar el índice filebeat-* y se debería ver los logs en tiempo real.


![img1](img/igm24.PNG)



FIN