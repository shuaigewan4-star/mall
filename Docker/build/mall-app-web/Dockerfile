FROM nginx:alpine

COPY mall-app-web/dist/build/h5/ /usr/share/nginx/html/
COPY docker/mall-app-web/default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
