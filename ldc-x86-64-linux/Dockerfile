FROM ubuntu:bionic

MAINTAINER Radu Racariu <radu.racariu@gmail.com>

ARG LDC_VER=1.16.0

ENV \
  COMPILER=ldc \
  COMPILER_VERSION=$LDC_VER

RUN apt-get update && apt-get install -y libssl1.0-dev gosu curl build-essential libxml2 libz-dev \
 && curl -fsS -o /tmp/install.sh https://dlang.org/install.sh \
 && bash /tmp/install.sh -p /dlang install "${COMPILER}-${COMPILER_VERSION}" \
 && rm /tmp/install.sh \
 && rm -rf /dlang/dub-*/dub.tar.gz

ENV \
  PATH=/dlang/${COMPILER}-${COMPILER_VERSION}/bin:${PATH} \
  LD_LIBRARY_PATH=/dlang/${COMPILER}-${COMPILER_VERSION}/lib \
  LIBRARY_PATH=/dlang/${COMPILER}-${COMPILER_VERSION}/lib \
  PS1="(${COMPILER}-${COMPILER_VERSION}) \\u@\\h:\\w\$"

RUN cd /tmp \
 && echo 'void main() {import std.stdio; stdout.writeln("it works"); }' > test.d \
 && ldc2 test.d \
 && ./test && rm test*

WORKDIR /src

RUN gosu nobody true \
 && rm -rf /var/lib/apt/lists/* \
 && rm -rf /var/cache/apt \
 && chmod 755 -R /dlang

COPY entrypoint.sh /
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["ldc2", "--version"]