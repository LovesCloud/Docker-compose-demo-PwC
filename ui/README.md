# static-site
Static HTML site for Docker nginx

## How to Run this inside Docker?
* One Dockerfile for Docker build
* nginx.conf file with nginx configurations
* then:`docker build .-t <YOUR_BUILD_TAG>` to build the image
* finally to run ` docker run -t <YOUR_BUILD_TAG>`


* Create a Dockerfile with following content
_Dockerfile_
```
FROM nginx:latest

# Copy custom configuration file from the current directory

COPY nginx.conf /etc/nginx/nginx.conf

RUN mkdir -p /data/www

COPY index.html /data/www/index.html
COPY 404.html /data/www/404.html
COPY . /data/www
```

then created configuration file for Nginx:
_nginx.conf_
```
user  nginx;
worker_processes  1;
error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
    sendfile        on;
    #tcp_nopush     on;
    keepalive_timeout  1024;
    #gzip  on;

    server {
        listen 80 default_server;
        listen [::]:80 default_server;
        location /health {
            access_log off;
            return  200;
        }

        if ($http_x_forwarded_proto = 'http') {
           return 301 https://$host$request_uri;
        }

        location / {
            root   /data/www;
            index  index.html;
            try_files $uri $uri/ /index.html;
        }
    }
}
```

