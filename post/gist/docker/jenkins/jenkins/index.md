# Jenkins


The jenkins service run in docker

## Note

### User

Before I run docker service, I've already add user USER in host system.
So, USER is uid=1000 gid=1000 groups=1000, found by ```id USER```.
And docker group id is 990, run ```cat /etc/group | grep docker```.
I add USER to docker group.
To limit root permission for jenkins docker, only allow USER to run service:

```bash
# docker group user USER run jenkins service, add below into docker-compose.yml
user: 1000:990
```

## Problem

- Because jenkins service running by USER(id: 1000), it doesn't have permission to install package, running apt or mv binary file to root owned location.
  - I choose to expose host package package from host to docker container
  - in docker-compose.yml:

```bash
volumes:
  # required software
  - /usr/local/bin/hugo:/usr/local/bin/hugo
```

