# Traefik as reverse proxy for docker

by Julien Maumené https://www.maumene.fr

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
      - "traefik.enable=true"
      - "traefik.docker.network=traefik_webgateway"
      - "traefik.http.routers.web.rule=Host(`www.my-project.local`, `myproject-local`)"
      - "traefik.http.routers.web.entrypoints=http"
      - "traefik.http.routers.webs.rule=Host(`www.my-project.local`, `myproject.local`)"
      - "traefik.http.routers.webs.entrypoints=https"
      - "traefik.http.routers.webs.tls=true"

networks:
  traefik:
    external:
      name: traefik_webgateway
``` 

Add www.project.local to /etc/hosts :

```text
127.0.0.1	www.project.local
```

See https://github.com/jmaumene/docker-symfony/blob/master/docker-compose.yml for a example.
 
