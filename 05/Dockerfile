FROM nginx
USER root
RUN apt-get update && apt-get install -y gcc spawn-fcgi libfcgi-dev && rm -rf /var/lib/apt/lists
RUN useradd -s /bin/bash -m maximins
USER maximins
WORKDIR /home/maximins
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./mini_server.c .
COPY ./start_server.sh .
ENTRYPOINT ["sh", "./start_server.sh"]