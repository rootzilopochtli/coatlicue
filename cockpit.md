# cockpit

Using [cockpit](https://cockpit-project.org/) for monitoring performance and management via web-based interface.

## Instalation

- Install cockpit and the modules for the registration of the sessions through [tlog](https://github.com/Scribery/tlog) and [sssd](https://sssd.io/), dashboard and container management:

```
dnf -y install cockpit sssd tlog cockpit-session-recording cockpit-dashboard cockpit-podman
```

- Enable and start cockpit

```
systemctl enable cockpit.socket --now
```

## Setting Up

### Customizing login screen

- Backup and replace the `/usr/share/cockpit/branding/default/bg-plain.jpg` file with the design created for the login screen:

<img src="https://github.com/rootzilopochtli/coatlicue/blob/master/cockpit_files/bg-plain.jpg" alt="login screen" width="341" height="192">

### Set up tlog and ssd

- Create `guests-users` user group:

```
groupadd -g 1001 guests-users
```

- Create `/etc/sssd/conf.d/sssd-session-recording.conf` configuration file with content:

```
[session_recording]
scope = some
groups = guests-users
```

- Adds the user to be monitored to the monitored group:

```
groupmems -g guests-users -a user01
```

Use the Cockpit session-login panel to get the logged data back out.

### Proxying cockpit over nginx

WIP
