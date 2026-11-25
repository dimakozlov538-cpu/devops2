FROM nginx:latest

RUN apt-get update && apt-get install -y gcc spawn-fcgi libfcgi-dev

WORKDIR /app/

COPY ./nginx/nginx1.conf /etc/nginx/nginx.conf
COPY ./server/server.c .
COPY ./ex4/start.sh .

CMD ["bash", "./start.sh"]