FROM bitnami/mongodb

LABEL maintainer="Radu Racariu <radu.racariu@gmail.com>"

EXPOSE 9095

RUN apt-get update && apt-get install -y netcat unzip

RUN gosu nobody true \
 && rm -rf /var/lib/apt/lists/*

RUN mkdir /opt/dub
WORKDIR /opt/dub
COPY dub-registry .
RUN chmod +x dub-registry
RUN curl -O -L https://github.com/dlang/dub-registry/archive/master.zip
RUN unzip master.zip
RUN cp dub-registry-master/categories.json .
RUN cp -R dub-registry-master/public/ public
RUN rm -fR dub-registry-master
RUN rm master.zip

RUN apt-get autoremove -y unzip

RUN > /entrypoint.sh \ 
&& echo "#!/bin/bash" >> /entrypoint.sh \
&& echo "/app-entrypoint.sh /run.sh &" >> /entrypoint.sh \
&& echo "while ! nc -z localhost 27017; do"  >> /entrypoint.sh \
&& echo " sleep 1"  >> /entrypoint.sh \
&& echo "done" >> /entrypoint.sh \
&& echo "echo Starting dub registry" >> /entrypoint.sh \
&& echo "export PATH=/opt/dub:/dub:\$PATH" >> /entrypoint.sh \
&& echo "cd /opt/dub"  >> /entrypoint.sh \
&& echo "[[ -e /dub/settings.json ]] && ln -s /dub/settings.json /opt/dub/settings.json"  >> /entrypoint.sh \
&& echo "./dub-registry --bind 0.0.0.0 --p 9095" >> /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]