# nginx

The reverse proxy service uses nginx installed on the system OS.

## Installation

- Install and enable nginx

```
dnf install nginx
systemctl enable --now nginx
```

## Setting Up

### Firewall

- Close all ports and only allow access by nginx ports (80 and 443)

```
firewall-cmd --zone=public --list-services --permanent
```

### HTTP

- Granting access in SELinux

```
setsebool httpd_can_network_connect on -P
```

- Adding settings for http request forwarding on /etc/nginx/nginx.conf ([Example File](https://github.com/rootzilopochtli/coatlicue/blob/master/nginx_files/nginx.conf))

<pre>
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       <b>80</b>;
        server_name  <b>test.rootzilopochtli.com</b>;

        location / {
	    <i>proxy_pass http://test.rootzilopochtli.com:8080/;
            index  index.html index.htm index.php;
            proxy_set_header Host      $host;
            proxy_set_header X-Real-IP $remote_addr;</i>
            } # end location
        } # end server
    } # end http
</pre>

- Test changes

```
nginx -t
```

### Auto SSL Generation

- *TODO*
