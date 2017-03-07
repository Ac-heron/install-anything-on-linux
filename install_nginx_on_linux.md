# Install nginx

- site:http://nginx.org/
- download:wget http://nginx.org/download/nginx-1.11.10.tar.gz
- tar: tar -zxvf nginx-1.11.10.tar.gz
- cd : cd nginx-1.11.10
- 运行(默认安装到/usr/local/nginx(可加参数))：./configure --without-http_rewrite_module
- 编译：make
- 安装：make install
- 运行：sudo usr/local/nginx/sbin/nginx
- localhot:80

