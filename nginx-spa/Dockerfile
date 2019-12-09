FROM nginx:latest

# 创建www目录
RUN mkdir -p /usr/share/nginx/html

# 覆盖nginx配置
COPY nginx.conf /etc/nginx/nginx.conf
COPY conf.d/default.conf /etc/nginx/conf.d/default.conf

# COPY ./build/ /var/share/nginx/html/
CMD ["nginx", "-g", "daemon off;"]
