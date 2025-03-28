# Plex


## Create configuration folders

```bash
mkdir /root/plex
mkdir /root/plex/config
mkdir /mnt/vdb1/plex
mkdir /mnt/vdb1/plex/transcode
mkdir /mnt/vdb1/plex/data
```

## Docker compose file

docker-compose.yml

```yml
version: "3"

services:
  plex:
    container_name: plex
    image: plexinc/pms-docker
    # mem_limit: 4gb
    restart: unless-stopped
    environment:
      - TZ=Asia/Shanghai
      #- PLEX_CLAIM=<claimToken>
    network_mode: host
    volumes:
      - /root/plex/config:/config
      - /mnt/vdb1/plex/transcode:/transcode
      - /mnt/vdb1/plex/data:/data
```

