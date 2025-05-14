FROM nginx:1.10.1-alpine
#COPY listrace-v1.0/index.html /usr/share/nginx/html
COPY listrace-v1.0/ /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]