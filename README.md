# multi-site-setup-nginx
Setup multiple site at single machine with nginx proxy

# Create docker network bridge
create docker network using:
```
docker network create nginx-proxy
```


then start nginx-proxy container using:
```
docker-compose up -d
```

# other separate container
it should be something like this:
```
version: "3"

services:

   nginx:
    image: nginx:alpine
    expose:
      - "80"
    env_file:
      - .env
    volumes:
      - ./docker/nginx/:/etc/nginx/conf.d/
    links:
      - wordpress
    depends_on:
      - wordpress
    restart: unless-stopped

   wordpress:
    build: .
    expose:
      - 80
    restart: always
    env_file:
      - .env
    volumes:
      - ./docker/wordpress/html:/var/www/html
      - ./docker/wordpress/.htaccess:/var/www/html/.htaccess:ro
    network_mode: nginx-proxy

networks:
  default:
    external:
      name: nginx-proxy
```
