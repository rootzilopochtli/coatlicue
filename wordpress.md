# wordpress

For the wordpress service we use a php container image, which uses an [S2I](https://github.com/openshift/source-to-image) base image, based on CentOS. The wordpress files are exported to the container via a bind mount.

## Setting up the container

- Pull the image to inspect it for variables for settings:

```
podman pull docker.io/centos/php-73-centos7
```

- Inspect the image

<pre>
# podman inspect docker.io/centos/php-73-centos7 | egrep -w "User|ExposedPorts|APP_ROOT" -A2
            "User": "<b>1001</b>",
            "ExposedPorts": {
                "<b>8080/tcp</b>": {},
                "<b>8443/tcp</b>": {}
--
                "APP_ROOT=<b>/opt/app-root</b>",
                "HOME=<b>/opt/app-root/src</b>",
#
</pre>

- Create directory and grant permissions to user

```
mkdir -p /opt/app-root/src/ & chown 1001 -R /opt/app-root/src
```

- Download the latest package for wordpress service

```
cd /opt/app-root/src/
curl -o latest.tar.gz https://wordpress.org/latest.tar.gz
mv wordpress/* /opt/app-root/src/ 
chown 1001 -R /opt/app-root/src 
```

- Run the container

```
podman run --name php-73-service -p 8080:8080 \ 
-v /opt/app-root/src:/opt/app-root/src:Z  \
--net host docker.io/centos/php-73-centos7 \
/bin/sh -c /usr/libexec/s2i/run
```

- Create the service ([Example file](https://github.com/rootzilopochtli/coatlicue/blob/master/wordpress_files/php-73-service.service))

```
vim /etc/systemd/system/php-73-service.service
```

- Reload systemd and enable service

```
systemctl daemon-reload
systemctl enable --now php-73-service
```

- Open the 8080 port on system's firewall

```
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --reload
```

- Start wordpress installation

<img src="https://github.com/rootzilopochtli/coatlicue/blob/master/images/wordpress.png" alt="wordpress installation" width="384" height="216">
