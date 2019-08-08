[![Docker pulls](https://img.shields.io/docker/pulls/rracariu/ldc-linux-arm.svg)](https://hub.docker.com/r/rracariu/ldc-linux-arm/)
[![Docker Build](https://img.shields.io/docker/automated/rracariu/ldc-linux-arm.svg)](https://hub.docker.com/r/rracariu/ldc-linux-arm/)

# doker - ldc cross compile
Provides dlang LDC docker images for cross compile usage.

## about
Based on https://github.com/dockcross and https://github.com/lindt/docker-ldc

## usage
Considering that `some.d` file is located in the current dir and the target is Linux ARM with software floating point
```
docker run --rm -ti -v $(pwd):/src rracariu/ldc-linux-arm ldc-arm some.d
```
