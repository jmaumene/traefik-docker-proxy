# Traefik as reverse proxy for docker

clone project
--------------

enter in directory and run command :
```shell script
docker network create traefik_webgateway
docker-compose up -d
```

Generate Certificates
----------------------
```shell script
sudo openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout certs/traefik.key -out certs/traefik.crt
```


Configure projects
------------------

Configure project, add in docker-compose.yml :

```yaml
services:
  my_service:
    ... Parameters ...
    networks:
      - traefik
    labels:
       - "traefik.frontend.rule=Host:www.project.local"

networks:
  traefik:
    external:
      name: traefik_webgateway
``` 

Add www.project.local to /etc/hosts :

```text
127.0.0.1	www.project.local
```
