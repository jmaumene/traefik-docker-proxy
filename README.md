# Traefik as reverse proxy for docker

clone project
enter in directory and run command :
```
docker network create traefik_webgateway
docker-compose up -d
```

Configure project, add in docker-compose.yml :

```
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
```
127.0.0.1	www.project.local
```
