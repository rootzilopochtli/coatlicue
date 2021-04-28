# MariaDB

The database engine is built in a container, through podman, based in fedora, and managed as a service by the OS.
The database files are exported to the container via a bind mount.

## Setting up the container

- Pull the image to inspect it for variables for settings:

```
podman pull registry.fedoraproject.org/f31/mariadb
```

- Inspect the image

<pre>
# podman inspect registry.fedoraproject.org/f31/mariadb | egrep -wi "User|home"
            "User": "<b>27</b>",
                "HOME=<b>/var/lib/mysql</b>",
                "usage": "<i>docker run -d -e MYSQL_USER=user -e MYSQL_PASSWORD=pass \
                -e MYSQL_DATABASE=db -p 3306:3306 f31/mariadb</i>",
#
</pre>

- Create directory and grant permissions to user

```
mkdir -p /opt/var/lib/mysql/data && chown 27:27 /opt/var/lib/mysql/data
```

- Run the container

```
podman run -d --name mariadb-service -p 3306:3306 \
-v /opt/var/lib/mysql/data:/var/lib/mysql/data:Z \
-e MYSQL_USER=wordpress -e MYSQL_PASSWORD=secret \
-e MYSQL_DATABASE=wordpress --net host registry.fedoraproject.org/f31/mariadb
```

- [Install MariaDB client](https://docs.fedoraproject.org/en-US/quick-docs/installing-mysql-mariadb/) and test access

```
dnf module list mariadb
dnf module enable mariadb:10.3
dnf module install mariadb:10.3/client
```

```
# mysql -u wordpress -p -h ::1 
Enter password: 

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| test               |
| wordpress          |
+--------------------+
3 rows in set (0.006 sec)

MariaDB [(none)]> exit
Bye
#
```

- Create the service ([Example file](https://github.com/rootzilopochtli/coatlicue/blob/master/mariadb_files/mariadb-service.service))

```
vim /etc/systemd/system/mariadb-service.service
```

- Reload systemd and enable service

```
systemctl daemon-reload
systemctl enable --now mariadb-service
```
