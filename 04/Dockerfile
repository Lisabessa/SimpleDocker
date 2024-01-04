FROM nginx
WORKDIR /home/
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./mini_server.c .
COPY ./start_server.sh .
RUN apt-get update
RUN apt-get install -y gcc spawn-fcgi libfcgi-dev
ENTRYPOINT ["sh", "./start_server.sh"]