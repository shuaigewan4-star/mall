FROM nginx:alpine
COPY mall-admin-web/dist/ /usr/share/nginx/html/
COPY docker/mall-admin-web/default.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
