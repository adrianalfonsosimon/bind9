##Explición fichero docker-compose.yml

```
version: "3.3"
services:
  asir_bind9:
    container_name: asir_bind9
    image: internetsystemsconsortium/bind9:9.16
    networks:
      dc01:
        ipv4_address: 10.1.0.254
    ports:
      - 53:53
    volumes:
      - conf1:/etc/bind
      - conf2:/var/cache/bind
      - conf3:/var/lib/bind
      - conf4:/var/log
  asir_cliente:
    container_name: asir_cliente
    image: oadri/ubuntu20:comandos_instalados
    networks:
      - dc01
    stdin_open: true  # docker run -i
    tty: true         # docker run -t
    dns:
      - 10.1.0.254  # contenedor de dns server
volumes:
  conf1:
    external: true
  conf2:
    external: true
  conf3:
    external: true
  conf4:
    external: true
networks:
  dc01:
    ipam: 
      config:
        - subnet: 10.1.0.0/24
```
