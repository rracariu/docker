FROM ubuntu:latest

LABEL maintainer="Radu Racariu <radu.racariu@gmail.com>"

ENV \
  COMPILER=ldc \
  COMPILER_VERSION=1.16.0

RUN apt-get update && apt-get install -y gpg gosu xz-utils curl gcc-arm-linux-gnueabi binutils-arm-linux-gnueabi cmake

RUN curl -fsS -o /tmp/install.sh https://dlang.org/install.sh \
 && bash /tmp/install.sh -p /dlang install "${COMPILER}-${COMPILER_VERSION}" \
 && rm /tmp/install.sh \
 && rm -rf /dlang/${COMPILER}-*/lib32

ENV \
  PATH=/dlang/${COMPILER}-${COMPILER_VERSION}/bin:${PATH} \
  LD_LIBRARY_PATH=/dlang/${COMPILER}-${COMPILER_VERSION}/lib \
  LIBRARY_PATH=/dlang/${COMPILER}-${COMPILER_VERSION}/lib \
  CC=arm-linux-gnueabi-gcc \
  LD=arm-linux-gnueabi-ld \
  AR=arm-linux-gnueabi-ar \
  PS1="(${COMPILER}-${COMPILER_VERSION}) \\u@\\h:\\w\$"

RUN echo 'ldc2 -mtriple=armv7-linux-gnueabi -gcc=arm-linux-gnueabi-gcc "$@"' > /dlang/${COMPILER}-${COMPILER_VERSION}/bin/ldc-arm \
 && chmod +x /dlang/${COMPILER}-${COMPILER_VERSION}/bin/ldc-arm


RUN cd /tmp \
&& rm -R "/dlang/${COMPILER}-${COMPILER_VERSION}/lib" \
&& "/dlang/${COMPILER}-${COMPILER_VERSION}/bin/ldc-build-runtime" --dFlags="-w;-mtriple=armv7-linux-gnueabi" --cFlags="-mfloat-abi=softfp -fPIC" --targetSystem="Linux;UNIX" \
&& mkdir "/dlang/${COMPILER}-${COMPILER_VERSION}/lib" \
&& cp ldc-build-runtime.tmp/lib/*.a "/dlang/${COMPILER}-${COMPILER_VERSION}/lib" \
&& cp ldc-build-runtime.tmp/lib/*.so "/dlang/${COMPILER}-${COMPILER_VERSION}/lib" \
&& rm -R ldc-build-runtime.tmp

 RUN apt-get autoremove -y gpg \
  && apt-get remove -y cmake cmake-data \
  && rm -rf /var/cache/apt

RUN cd /tmp \
 && echo 'void main() {import std.stdio; stdout.writeln("it works");}' > test.d \
 && ldc-arm test.d \
 && readelf -h /tmp/test| grep 'Machine:' | sed 's/:/\n/g' | tail -n 1 | grep ARM > /dev/null \
 && rm test*

WORKDIR /src

RUN gosu nobody true \
 && rm -rf /var/lib/apt/lists/* \
 && chmod 755 -R /dlang

COPY entrypoint.sh /
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["ldc-arm"]
