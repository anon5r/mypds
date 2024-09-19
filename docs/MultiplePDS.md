複数のPDSを実行する
======

1つの環境で複数のPDSを同居させます。

データディレクトリと環境変数をそれぞれ別々にすることで、同居させることができます。


### compose.yml

```compose.yml
services:
...
  pds1:
    container_name: pds1
    image: ghcr.io/bluesky-social/pds:latest
    restart: unless-stopped
    volumes:
      - type: bind
        source: ./data1
        target: /pds
    env_file:
      - ./envs/pds1.env
    networks:
      - frontend
      - backend
  pds2:
    container_name: pds2
    image: ghcr.io/bluesky-social/pds:latest
    restart: unless-stopped
    volumes:
      - type: bind
        source: ./data2
        target: /pds
    env_file:
      - ./envs/pds2.env
    networks:
      - frontend
      - backend
...
```
