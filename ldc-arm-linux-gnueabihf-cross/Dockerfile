FROM dockcross/linux-armv7:latest

LABEL maintainer="Radu Racariu <radu.racariu@gmail.com>"

ENV \
  COMPILER=ldc \
  COMPILER_VERSION=1.14.0

RUN apt-get install -y curl libcurl3 binutils-arm-linux-gnueabihf
RUN rm -rf /var/cache/apt

RUN curl -fsS -o /tmp/install.sh https://dlang.org/install.sh \
 && bash /tmp/install.sh -p /dlang install "${COMPILER}-${COMPILER_VERSION}" \
 && rm /tmp/install.sh \
 && rm -rf /dlang/${COMPILER}-*/lib32

ENV \
  PATH=/dlang/${COMPILER}-${COMPILER_VERSION}/bin:${PATH} \
  LD_LIBRARY_PATH=/dlang/${COMPILER}-${COMPILER_VERSION}/lib \
  LIBRARY_PATH=/dlang/${COMPILER}-${COMPILER_VERSION}/lib \
  CC=arm-linux-gnueabihf-gcc \
  PS1="(${COMPILER}-${COMPILER_VERSION}) \\u@\\h:\\w\$"

RUN echo 'ldc2 -mtriple=armv6-hardfloat-linux-gnueabihf -gcc=arm-linux-gnueabihf-gcc "$@"' > /dlang/${COMPILER}-${COMPILER_VERSION}/bin/ldc-armhf \
 && chmod +x /dlang/${COMPILER}-${COMPILER_VERSION}/bin/ldc-armhf


RUN cd /tmp \
&& rm -R "/dlang/${COMPILER}-${COMPILER_VERSION}/lib" \
&& "/dlang/${COMPILER}-${COMPILER_VERSION}/bin/ldc-build-runtime" --dFlags="-w;-mtriple=armv6-hardfloat-linux-gnueabihf" --cFlags="-fPIC" --targetSystem="Linux;UNIX" \
&& mkdir "/dlang/${COMPILER}-${COMPILER_VERSION}/lib" \
&& cp ldc-build-runtime.tmp/lib/*.a "/dlang/${COMPILER}-${COMPILER_VERSION}/lib" \
&& cp ldc-build-runtime.tmp/lib/*.so "/dlang/${COMPILER}-${COMPILER_VERSION}/lib" \
&& rm -R ldc-build-runtime.tmp

 RUN apt-get autoremove -y python \
 && apt-get autoremove -y python2.7-minimal \
 && apt-get autoremove -y qemu-user \
 && apt-get autoremove -y qemu-user-static \
 && apt-get autoremove -y vim-runtime \
 && apt-get autoremove -y wget \
 && rm -rf /var/cache/apt

RUN cd /tmp \
 && echo 'void main() {import std.stdio; stdout.writeln("it works");}' > test.d \
 && ldc-armhf test.d \
 && readelf -h /tmp/test| grep 'Machine:' | sed 's/:/\n/g' | tail -n 1 | grep ARM > /dev/null \
 && rm test*

WORKDIR /src

RUN gosu nobody true \
 && rm -rf /var/lib/apt/lists/* \
 && chmod 755 -R /dlang

COPY entrypoint.sh /
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["ldc-armhf"]
