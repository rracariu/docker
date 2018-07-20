[![Docker pulls](https://img.shields.io/docker/pulls/rracariu/ldc-linux-x86-64.svg)](https://hub.docker.com/r/rracariu/ldc-linux-x86-64/)
[![Docker Build](https://img.shields.io/docker/automated/rracariu/ldc-linux-x86-64.svg)](https://hub.docker.com/r/rracariu/ldc-linux-x86-64/)

# docker-ldc-linux-x86-64

Docker Image for LLVM-based [D](http://dlang.org) Compiler. Default target Linux x86-64

## Usage

Place a `test.d` in your current directory.
Then execute
```
docker run --rm -ti -v $(pwd):/src rracariu/ldc-linux-x86-64 ldc2 test.d
```
