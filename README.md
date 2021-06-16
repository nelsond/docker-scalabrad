# scalabrad docker image

This repository contains the Dockerfile to build an image for the [scalabrad 
manager](https://github.com/labrad/scalabrad).
Currently the following releases are supported:

* 0.8.3 (current release, tagged `latest`)

## Installation

All images are prebuilt on [hub.docker.com](hub.docker.com) and can be directly installed with `docker create`.

```shell
$ docker create nelsond/scalabrad
```

## Example usage

To start a labrad server with the password `secret` and tls not required, run

```shell
$ docker run -d -p 7682:7682 \
  -e LABRADPASSWORD=secret \
  -e LABRAD_TLS_REQUIRED=false \
  nelsond/scalabrad
```

For example, a python client can now connect to this server by using
```python
import labdrad

conn = labrad.connect(password='secret')
conn.servers # => auth ...
```
### docker-compose

First, save the password in an external `.env` file.

```shell
$ echo "LABRADPASSWORD=mysecretpassword" > .env
```

Then, create `docker-compose.yml` with the following content:

```yaml
version: "3.9"
services:
  manager:
    restart: always  
    image: nelsond/scalabrad:0.8.3
    environment:
      - LABRAD_TLS_REQUIRED=false
    volumes:
      - type: bind
        source: ./config
        target: /root/.labrad
    ports:
      - "7682:7682"
      - "7643:7643"
```

Finally, run `docker-compose` to start and configure the container

```shell
$ mkdir config && docker-compose up --d
```
