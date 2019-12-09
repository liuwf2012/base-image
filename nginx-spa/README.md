# nginx-image-web

前端spa应用通用nginx容器

### 配置文件
> /etc/nginx/nginx.conf

```bash
# nginx服务的默认用户
user  nginx;
# 一般为CPU核个数
worker_processes  1;
# 默认错误日志记录路径和级别
error_log  /var/log/nginx/error.log warn;
# pid文件记录路径
pid        /var/run/nginx.pid;
# 事件处理模型优化
events {
  worker_connections  1024;
}

http {
  # 文件扩展名与文件类型映射表
  include       /etc/nginx/mime.types;
  # 默认文件类型
  default_type  application/octet-stream;
  # 默认日志格式
  log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

  # 访问日志
  # access_log  /var/log/nginx/access.log  main;

  # 开启文件的高效传输模式
  sendfile        on;
  #tcp_nopush     on;

  # 设置客户端连接保持会话的超时时间。
  keepalive_timeout  65;

  # 开启gzip压缩输出
  gzip               on;

  # Buffer pool count and their size.
  gzip_buffers       16 8k;

  # Disable Gzip if the HTTP version is less than this value.
  gzip_http_version  1.1;

  # Compression level from worst to best (1-9) compressed.
  # A higher value will require more CPU per request.
  gzip_comp_level    6;

  # Disable Gzip if the user-agent matches the regular expression.
  gzip_disable       "MSIE [1-6]\.(?!.*SV1)";

  # The required minimum size of the body (in bytes) for it to get compressed.
  gzip_min_length    1k;

  # Compress the following extensions:
  # css, exe, dll, gif, jpeg, jpg, json, js, png, so, ttf, txt, xml
  gzip_types        application/font-sfnt
                    application/json
                    application/octet-stream
                    application/vnd.ms-fontobject
                    application/x-font-ttf
                    application/xml
                    font/opentype
                    font/x-woff
                    image/gif
                    image/jpeg image/jpg
                    image/png
                    text/css
                    text/javascript
                    text/plain
  ;

  include /etc/nginx/conf.d/*.conf;
}

```

> /etc/nginx/conf.d/default.conf
```bash
server {
  listen       80 default_server;
  server_name  _;

  #charset koi8-r;
  #access_log  /var/log/nginx/host.access.log  main;

  location / {
      root   /usr/share/nginx/html;
      index  index.html index.htm;
      # for spa
      try_files $uri index.html;
  }

  #error_page  404              /404.html;

  # redirect server error pages to the static page /50x.html
  #
  error_page   500 502 503 504  /50x.html;
  location = /50x.html {
    root   /usr/share/nginx/html;
  }
}
```