nginx-config
==============
## Installing NGINX(安装)
1. Open terminal window and open the sources.list file using the command
```
sudo nano /etc/apt/sources.list
```
2. You can add the Nginx repository links at the bottom of the file. Scroll down to
the very bottom of the file and add the two lines below :
```
deb http://nginx.org/packages/ubuntu/ trusty nginx
deb-src http://nginx.org/packages/ubuntu/ trusty nginx
```
3. Save the file.
4. Now you can download the package lists from the repositories and update
them with the information for the newest versions of the packages and their
dependencies. You can do that by typing the following command:
```
sudo apt-get update
```
5. You will get the following error regarding the missing signature key. It is
happening because gpg is trying to sign the nginx release and check its signature.
But the signing key is missing on the server and hence gpg is not able to validate
the nginx package:
Reading package lists... Done
W: GPG error: http://nginx.org trusty Release: The following signatures couldn't
be verified because the public key is not available: NO_PUBKEY ABF5BD827BD9BF62
6. Download and add the nginx signature key using the command below:
```
wget http://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key
```
7. Now try re-synchronizing the package index from the sources:
```
sudo apt-get update
```
8. Now that the package list is updated and indexed, you can install Nginx:
```
sudo apt-get install nginx
```
 9. You can verify Nginx installed the version:
```
nginx –v
```

You can get the complete list of all Nginx configuration details and its version by
using the –V command option with nginx . Here is a sample output of the command:
```
nginx –V
```

It has a mine.types and fastcgi_params file that contains all
the mime types that are enabled on the web server and fastcgi configuration details.
All these default configurations enable the Nginx server to start:

```
ls –F /etc/nginx/
```
# 视频

```bash
sudo apt-get install -y unzip build-essential libpcre3 libpcre3-dev libssl-dev
cd /opt
wget http://nginx.org/download/nginx-1.10.1.tar.gz
wget https://github.com/arut/nginx-rtmp-module/archive/master.zip
tar -zxvf nginx-1.10.1.tar.gz
unzip -o master.zip
cd nginx-1.10.1

./configure --with-http_ssl_module --add-module=../nginx-rtmp-module-master
make
make install
```
配置
```
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       8802;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}

rtmp {
    server {
        application myvideos {
            play /usr/local/nginx/html;
        }
        application mylive {
            live on;

            hls on;
            hls_path /usr/local/nginx/html/hls;
            hls_fragment 10s;

            dash on;
            dash_path /usr/local/nginx/html/dash;
            dash_fragment 10s;
        }
    }
}
```
