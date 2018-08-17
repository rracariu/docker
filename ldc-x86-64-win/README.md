# docker-ldc-win-x86-64

Docker Image for LLVM-based [D](http://dlang.org) Compiler. Default target Windows x86-64

Useful for cross-compiling binaries for Win32 X86-64.

Original work https://github.com/jacob-carlborg/docker-ldc-windows

## Usage

Build local image

```
docker build --build-arg MSVC_LIBS_URL=http://your-path-to-msvc-libs -t ldc-win-x86-64
```

Mind that this image uses you **onw** copy of the MS VC libraries. Make sure you understant the legal jargon for the propietary license they are distributed under.

Place a `test.d` in your current directory.
Then execute
```
docker run --rm -ti -v $(pwd):/src ldc-win-x86-64 ldc2 test.d
```
