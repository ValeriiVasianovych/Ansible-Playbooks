FROM nginx:latest

RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y \
        net-tools \
        neovim \
    && rm -rf /var/lib/apt/lists/*

COPY ./index.html /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
