FROM nginx:alpine

WORKDIR /usr/share/nginx/html

COPY ./templates/index.html config.json ./

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]