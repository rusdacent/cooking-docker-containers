# Dockle demo

## Install Hadolint

https://github.com/goodwithtech/dockle#installation

## Example

Запустим проверку

```
$ dockle rusdacent/dockle:example
```

или

```
$ docker run --rm goodwithtech/dockle:v0.4.14 rusdacent/dockle:example
```

Результаты
```
FATAL	- CIS-DI-0009: Use COPY instead of ADD in Dockerfile
	* Use COPY : /bin/sh -c #(nop) ADD file:81c0a803075715d1a6b4f75a29f8a01b21cc170cfc1bff6702317d1be2fe71a3 in /app/credentials.json 

FATAL	- CIS-DI-0010: Do not store credential in environment variables/files
	* Suspicious filename found : app/credentials.json (You can suppress it with "-af credentials.json")
	* Suspicious ENV key found : MYSQL_PASSWD on /bin/sh -c #(nop)  ENV MYSQL_PASSWD=password (You can suppress it with --accept-key)

FATAL	- DKL-DI-0005: Clear apt-get caches
	* Use 'rm -rf /var/lib/apt/lists' after 'apt-get install|update' : /bin/sh -c apt-get update && apt-get install -y git

FATAL	- DKL-LI-0001: Avoid empty password
	* No password user found! username : nopasswd

WARN	- CIS-DI-0001: Create a user for the container
	* Last user should not be root

INFO	- CIS-DI-0005: Enable Content trust for Docker
	* export DOCKER_CONTENT_TRUST=1 before docker pull/build

INFO	- CIS-DI-0006: Add HEALTHCHECK instruction to the container image
	* not found HEALTHCHECK statement

INFO	- CIS-DI-0008: Confirm safety of setuid/setgid files
	* setgid file: grwxr-xr-x usr/bin/chage
	* setuid file: urwxr-xr-x bin/ping
	* setgid file: grwxr-xr-x usr/bin/ssh-agent
	* setuid file: urwxr-xr-x bin/umount
	* setuid file: urwxr-xr-x usr/bin/chsh
	* setuid file: urwxr-xr-x usr/bin/passwd
	* setuid file: urwxr-xr-x usr/lib/openssh/ssh-keysign
	* setgid file: grwxr-xr-x sbin/unix_chkpwd
	* setuid file: urwxr-xr-x bin/mount
	* setuid file: urwxr-xr-x usr/bin/newgrp
	* setuid file: urwxr-xr-x usr/bin/chfn
	* setgid file: grwxr-xr-x usr/bin/expiry
	* setgid file: grwxr-xr-x usr/bin/wall
	* setuid file: urwxr-xr-x usr/bin/gpasswd
	* setuid file: urwxr-xr-x bin/su
```

Dockerfile соответствующий образу

```dockerfile
FROM debian:latest

# DKL-LI-0005
RUN apt-get update && apt-get install -y git
# DKL-LI-0001
RUN useradd nopasswd -p ""
RUN chmod u+s /etc/shadow
RUN chmod g+s /etc/passwd
# CIS-DI-0009
# CIS-DI-0010
ADD credentials.json /app/credentials.json
COPY sample.txt /app/sample.txt
RUN chmod u+s /app/sample.txt
RUN chmod g+s /app/sample.txt
# CIS-DI-0010
ENV MYSQL_PASSWD password
RUN chmod g-s /app/sample.txt
VOLUME /usr
```

via: [dockle-ci-test](https://github.com/goodwithtech/dockle-ci-test/blob/master/Dockerfile)
