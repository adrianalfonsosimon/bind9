## Explición fichero docker-compose.yml

```
version: "3.3"       #versión a utilizar de docker compose 
services:
  asir_bind9:        #creación de un servicio asignando un nombre
    container_name: asir_bind9          #nombre del contenedor que ejercerá de servidor dns
    image: internetsystemsconsortium/bind9:9.16       #imagen a partir de la cual se montará el contenedor anterior
    networks:             #se especificará la red en la que está el contenedor
      dc01:             #nombre de la red
        ipv4_address: 10.1.0.254    #IP del contenedor
    ports:    
      - 53:53   #relación entre el puerto anfitrión y el puerto del contenedor
    volumes:                  #se especifican las rutas del contenedor a las que harán referencia los volumenes creados a posteriori
      - conf1:/etc/bind
      - conf2:/var/cache/bind
      - conf3:/var/lib/bind
      - conf4:/var/log
  asir_cliente:   #creación del servicio para el contenedor cliente 
    container_name: asir_cliente      #nombre del contenedor cliente
    image: oadri/ubuntu20:comandos_instalados     #imagen a utlizar para la creación del contenedor, en este caso será una imagen propia con varias utilidades ya instaladas
    networks:       #especificación de la red en la que se encontrará el contenedor
      - dc01
    stdin_open: true  # docker run -i     #con esta opción el contenedor se mantendrá encendido 
    tty: true         # docker run -t     #con una terminal abierta
    dns:
      - 10.1.0.254  # se referencia al contenedor de dns para que sea este el dns del cliente
volumes:          #creación de los volumenes referenciados anteriormente
  conf1:
    external: true    #con esta opción no se crearán si ya existen
  conf2:
    external: true
  conf3:
    external: true
  conf4:
    external: true
networks:           #creación de la red asignada anteriormnete en los contenedores
  dc01:         #nombre de la red
    ipam:       #IP Address Management
      config:
        - subnet: 10.1.0.0/24     #subred en la que se encontrará
```

### /etc/resolve.conf / asir_cliente
```
nameserver 10.0.0.254
options ndots:0
```
### arranque y parada bind9
_esto se ejecutará en una maquina real con el servicio bind9 instalado

```
sudo service bind9 restart # para reiniciar el servicio 
sudo service bind9 start  #para iniciar el servicio
sudo service bind9 stop #para parar el servicio
sudo service bind9 status  #para ver el estado del servicio así como los posibles fallos
```
