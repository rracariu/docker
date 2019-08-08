FROM ubuntu:latest

MAINTAINER Radu Racariu <radu.racariu@gmail.com>

ENV \
  COMPILER=ldc \
  COMPILER_VERSION=1.16.0

RUN apt-get update && apt-get install -y gosu curl build-essential libssl-dev libxml2 libz-dev \
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

RUN apt-get update \
 && gosu nobody true \
 && rm -rf /var/lib/apt/lists/* \
 && rm -rf /var/cache/apt \
 && chmod 755 -R /dlang

COPY entrypoint.sh /
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["${COMPILER}2"]
