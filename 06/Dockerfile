FROM nginx:latest
ARG USERNAME=nginx
USER root
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./start_nginx.sh /home/$USERNAME/start_nginx.sh
WORKDIR /home/$USERNAME
RUN usermod -aG adm, root, nginx $USERNAME ; \
    chown -R $USERNAME:$USERNAME /etc/nginx/nginx.conf; \
    chown -R $USERNAME:$USERNAME /var/cache/nginx ; \
    chown -R $USERNAME:$USERNAME /var/run/ ; \
    chown -R $USERNAME:$USERNAME /home/$USERNAME ; \
    chown -R $USERNAME:$USERNAME /var/lib/apt/lists/partial ; \
    touch /var/run/nginx.pid; \
    chown -R $USERNAME:$USERNAME /var/run/nginx.pid; \
    chmod +x /home/$USERNAME/start_nginx.sh
USER $USERNAME
HEALTHCHECK NONE
CMD ["./start_nginx.sh"]