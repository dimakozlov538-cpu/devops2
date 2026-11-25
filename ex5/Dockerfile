FROM nginx:latest
ARG USERNAME=user_1

COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
COPY ./server/server.c /home/$USERNAME/app/server.c
COPY ./ex5/start.sh /home/$USERNAME/app/start.sh

WORKDIR /home/$USERNAME/app

RUN apt-get update \
    && apt-get install -y gcc spawn-fcgi libfcgi-dev \
    && gcc -o server server.c -lfcgi \
    && useradd -r -U -u 999 -s /bin/bash $USERNAME \
    && usermod -a -G $USERNAME nginx \
    && touch /var/run/nginx.pid \
    && chown -R $USERNAME:$USERNAME /var/run/nginx.pid /var/cache/nginx /var/log/nginx /home/$USERNAME \
    && chmod +x ./start.sh \
    && chmod u-s /usr/bin/umount /usr/bin/chfn /usr/bin/gpasswd /usr/bin/su /usr/bin/chsh /usr/bin/passwd /usr/bin/newgrp /usr/bin/mount \
    && chmod g-s /usr/bin/chage /usr/bin/expiry /sbin/unix_chkpwd /usr/bin/wall \
    && rm -rf /var/lib/apt/lists

HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost:81/ || exit 1

USER $USERNAME

ENTRYPOINT ["sh", "./start.sh"]
