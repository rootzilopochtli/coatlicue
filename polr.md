# Polr

Polr is a quick, modern, and open-source link shortener. It allows to host our own URL shortener: [rutil.io](http://rutil.io/).

The implementation was done through a podman pod, so that it would not affect the other installed applications.

The polr image was created and is maintained by [ajanvier](https://hub.docker.com/u/ajanvier) on [dockerhub](https://hub.docker.com/r/ajanvier/polr).

## Pre-requisites for the database

- Create directory and grant permissions to user for database store

```
mkdir -p /opt/var/lib/mysql/data2 && chown 27:27 /opt/var/lib/mysql/data2
```

## Setting up the Podman Pod

- Create pod

```
podman pod create -p 8000:8080 --name polr
```

- Add mariadb container to the pod

```
podman run -d --name polr-db \
-v /opt/var/lib/mysql/data2:/var/lib/mysql/data:Z \
-e MYSQL_USER=polr -e MYSQL_PASSWORD=polrpwd -e MYSQL_DATABASE=polr \
--pod polr registry.fedoraproject.org/f31/mariadb
```

- Add polr container to the pod

```
podman run -d --name polr-app -e "DB_HOST=polr" -e "DB_DATABASE=polr" \
-e "DB_USERNAME=polr" -e "DB_PASSWORD=polrpwd" \
-e "APP_ADDRESS=polr.demo" -e "ADMIN_USERNAME=admin" \
-e "ADMIN_PASSWORD=admin" -e "APP_NAME=polr" --pod polr ajanvier/polr
```

## Setting up the systemd service

- Create systemd service

```
podman generate systemd --name polr --files
```

- Move files to /etc/systemd/system/

```
mv *.service /etc/systemd/system/
```

- Enable and start the polr service

```
systemctl daemon-reload
systemctl enable --now pod-polr.service \
container-polr-db.service \
container-polr-app.service
```

## Setting up the reverse proxy

- Add to the /etc/nginx/nginx.conf file the following http block

```
    server {
        listen       80;
        server_name  polr.demo;

        location / {
	    proxy_pass http://test.rootzilopochtli.com:8000/;
            index  index.html index.htm index.php;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            } # end location
        } # end server
```

- Test changes

```
nginx -t
```

- Restart the reverse proxy service

```
systemctl restart nginx
```
