# Proyecto DHCP

## Cofiguración docker-compose

Le pondremos nombre, la imagen, y el modo red en host para poder asignar IP a nuestra máquina.

Además creamos el volumen y en la carpeta data que crearemos a continuación.

~~~
version: "3.9"
services: 
  asir_DHCPD:
    container_name: asir_serverDHCP
    hostname: asir_server_dhcp
    image: networkboot/dhcpd
    network_mode: host
    volumes:
      - ./data:/data
~~~

## Configuración del DHCP

Para la configuración del DHCP crearemos la carpeta _data_ como pusimos en el volumen y dentro un archivo llamado _**dhcp.conf**_ donde pondremos la configuración.

Ahí crearemos la subnet donde pondremos el DNS de google (8.8.8.8) y el router que será el (10.0.9.254)

Además asignamos los tiempos de los leases y pondremos que es un servidor autoritativo.

Por último creamos un grupo donde meteremos la MAC de nuestra máquina virtual y la IP fija a asignar.

~~~
# subnet 10.0.9.0 netmask 255.255.255.0 {    
#     range 10.0.9.228 10.0.9.233;    
#     option routers 10.0.9.1;    
#     option domain-name-servers 8.8.8.8;    
# }

subnet 10.0.9.0 netmask 255.255.255.0 {

  option domain-name-servers 8.8.8.8;
  option domain-name "asir_dhcp.com";
  option routers 10.0.9.254;
  option subnet-mask 255.255.255.0;
  option broadcast-address 10.0.9.255;

  authoritative;
  default-lease-time 86400;
  max-lease-time 172800;

  group {

     default-lease-time 604800;
     max-lease-time 691200;

     host apache {
         hardware ethernet 08:00:27:EC:9C:DD;
         fixed-address 10.0.9.228;
     }

  }
}
~~~

## Comprobación

Para la comprobación una vez levantado el servicio, iremos a nuestra máquina virtual, nos iremos al _cmd_ y con _ipconfig /renew_ haremos que refresque y nos muestra la IP y la configuración.

Allí veremos que tiene la IP fija que asignamos anterirormente y demás configuración.

Para hacer la prueba hacemos un ping a 8.8.8.8 y vemos que funciona.

![imgMaquina](https://github.com/HugoCea/ProyactoDHCP/blob/master/imagenes/imgmaquina.png)



























