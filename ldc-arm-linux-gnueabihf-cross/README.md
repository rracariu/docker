[![Docker pulls](https://img.shields.io/docker/pulls/rracariu/ldc-linux-armhf.svg)](https://hub.docker.com/r/rracariu/ldc-linux-armhf/)
[![Docker Build](https://img.shields.io/docker/automated/rracariu/ldc-linux-armhf.svg)](https://hub.docker.com/r/rracariu/ldc-linux-armhf/)

# doker - ldc cross compile
Provides dlang LDC docker images for cross compile usage.

## about
Based on https://github.com/dockcross and https://github.com/lindt/docker-ldc

## usage
Considering that `some.d` file is located in the current dir and the target is Linux ARM with hardware floating point
```
docker run --rm -ti -v $(pwd):/src rracariu/ldc-linux-armhf ldc-armhf some.d
```
