FROM dockcross/linux-armv7:latest

LABEL maintainer="Radu Racariu <radu.racariu@gmail.com>"

ENV \
  COMPILER=ldc \
  COMPILER_VERSION=1.12.0

RUN apt-get update && apt-get install curl libcurl3 \
 && curl -fsS -o /tmp/install.sh https://dlang.org/install.sh \
 && bash /tmp/install.sh -p /dlang install "${COMPILER}-${COMPILER_VERSION}" \
 && rm /tmp/install.sh \
 && rm -rf /var/cache/apt \
 && rm -rf /dlang/${COMPILER}-*/lib32 \
 && rm -rf /dlang/dub-1.0.0/dub.tar.gz

ENV \
  PATH=/dlang/${COMPILER}-${COMPILER_VERSION}/bin:${PATH} \
  LD_LIBRARY_PATH=/dlang/${COMPILER}-${COMPILER_VERSION}/lib \
  LIBRARY_PATH=/dlang/${COMPILER}-${COMPILER_VERSION}/lib \
  PS1="(${COMPILER}-${COMPILER_VERSION}) \\u@\\h:\\w\$"

RUN echo 'ldc2 -mtriple=armv6-hardfloat-linux-gnueabihf -gcc=arm-linux-gnueabi-gcc "$@"' > /dlang/${COMPILER}-${COMPILER_VERSION}/bin/ldc-armhf \
 && chmod +x /dlang/${COMPILER}-${COMPILER_VERSION}/bin/ldc-armhf


RUN cd /tmp \
&& wget -q "https://github.com/ldc-developers/ldc/releases/download/v${COMPILER_VERSION}/${COMPILER}2-${COMPILER_VERSION}-linux-armhf.tar.xz" \
&& tar xf "${COMPILER}2-${COMPILER_VERSION}-linux-armhf.tar.xz" \
&& rm -R "/dlang/${COMPILER}-${COMPILER_VERSION}/lib" \
&& mv "${COMPILER}2-${COMPILER_VERSION}-linux-armhf/lib" "/dlang/${COMPILER}-${COMPILER_VERSION}/" \
&& rm "${COMPILER}2-${COMPILER_VERSION}-linux-armhf.tar.xz" \
&& rm -R "${COMPILER}2-${COMPILER_VERSION}-linux-armhf"

 RUN apt-get autoremove -y python \
 && apt-get autoremove -y python2.7-minimal \
 && apt-get autoremove -y qemu-user \
 && apt-get autoremove -y qemu-user-static \
 && apt-get autoremove -y -m gfortran \
 && apt-get autoremove -y git \
 && apt-get autoremove -y vim-runtime

RUN cd /tmp \
 && echo 'void main() {import std.stdio; stdout.writeln("it works");}' > test.d \
 && ldc-armhf test.d \
 && readelf -h /tmp/test| grep 'Machine:' | sed 's/:/\n/g' | tail -n 1 | grep ARM > /dev/null \
 && rm test*

WORKDIR /src

RUN gosu nobody true \
 && apt-get autoremove -y wget \
 && rm -rf /var/lib/apt/lists/* \
 && chmod 755 -R /dlang

COPY entrypoint.sh /
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["ldc-armhf"]
