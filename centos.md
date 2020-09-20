# CentOS

After the first boot, the OS is updated:
```
dnf update
```

And install the necessary packages and tools:
```
dnf install podman buildah skopeo net-tools sos mlocate ps_mem
```

## ps&lowbar;mem

A utility to accurately report the core memory usage for a program:

<pre>
# ps&lowbar;mem
 Private  +   Shared  =  RAM used	Program

272.0 KiB + 123.0 KiB = 395.0 KiB	agetty (2)
416.0 KiB + 162.0 KiB = 578.0 KiB	sedispatch
540.0 KiB +  62.0 KiB = 602.0 KiB	rhsmcertd
696.0 KiB +  96.0 KiB = 792.0 KiB	gssproxy
760.0 KiB + 107.0 KiB = 867.0 KiB	chronyd
816.0 KiB + 112.5 KiB = 928.5 KiB	rpcbind
892.0 KiB + 118.5 KiB =   1.0 MiB	rngd
964.0 KiB +  70.5 KiB =   1.0 MiB	crond
892.0 KiB + 169.5 KiB =   1.0 MiB	auditd
  1.5 MiB + 208.0 KiB =   1.7 MiB	conmon (2)
  1.7 MiB + 118.5 KiB =   1.8 MiB	bash
  1.6 MiB + 245.0 KiB =   1.8 MiB	dbus-daemon
  1.3 MiB +   1.2 MiB =   2.5 MiB	systemd-journald
  2.3 MiB + 315.0 KiB =   2.6 MiB	sssd
  2.6 MiB + 227.0 KiB =   2.8 MiB	systemd-udevd
  2.5 MiB + 556.5 KiB =   3.0 MiB	systemd-logind
  2.8 MiB + 355.5 KiB =   3.1 MiB	systemd-resolved
  2.6 MiB +   1.1 MiB =   3.7 MiB	rsyslogd
  2.9 MiB + 947.5 KiB =   3.9 MiB	sssd&lowbar;be
  3.1 MiB + 988.0 KiB =   4.0 MiB	sssd&lowbar;nss
  2.5 MiB +   1.5 MiB =   4.0 MiB	(sd-pam)
  2.8 MiB +   1.5 MiB =   4.3 MiB	sshd (3)
  4.8 MiB + 897.5 KiB =   5.7 MiB	NetworkManager
  8.2 MiB + 849.5 KiB =   9.1 MiB	polkitd
  6.3 MiB +   2.9 MiB =   9.2 MiB	systemd (2)
 15.1 MiB + 918.5 KiB =  16.0 MiB	tuned
 29.5 MiB +   2.1 MiB =  31.6 MiB	rhsmd
---------------------------------
                        341.6 MiB
=================================
</pre>

### Reference

- [pixelb/ps&lowbar;mem](https://github.com/pixelb/ps_mem)

