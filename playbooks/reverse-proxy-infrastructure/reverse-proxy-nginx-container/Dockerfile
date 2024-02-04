FROM nginx:latest

RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y \
        net-tools \
        neovim \
    && apt-get autoremove -y \
    && apt-get clean \
    && apt-get install certbot -y \
    && apt-get install python3-certbot-nginx -y \
    && rm -rf /var/lib/apt/lists/*

COPY config/development-nginx-container.conf /etc/nginx/conf.d/
COPY config/production-nginx-container.conf /etc/nginx/conf.d/

EXPOSE 80 443 8080 3000 9090

CMD ["nginx", "-g", "daemon off;"]