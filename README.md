```shell
pkg update -y && pkg upgrade -y
pkg install php php-fpm mariadb golang python3 nodejs vim git

php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php
php -r "unlink('composer-setup.php');"
mv
```
default.conf
```
server {
	listen 8080;

	error_log /data/data/com.termux/files/home/webs/logs/api-error.log;
	server_name example.com;

	location / {
    default_type "text/plain";
    root /data/data/com.termux/files/home/webs/htdocs/prod/3kyu.net/api/public;
    try_files $uri =404;
	}

  #location / {
  #	return 301 https://$host$request_uri;
  #}

}

server {

	error_log /data/data/com.termux/files/home/webs/logs/example-error.log;
	listen 8443 ssl;

	http2 on;

	server_name example.com;

	root /data/data/com.termux/files/home/webs/htdocs/prod/website/api/public;

	ssl_certificate /data/data/com.termux/files/home/webs/certs/website/signed_chain.crt;
	ssl_certificate_key /data/data/com.termux/files/home/webs/certs/website/my.key;
	ssl_session_timeout 5m;
	ssl_protocols TLSv1.2;
	ssl_ciphers ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
	ssl_session_cache shared:SSL:50m;
	ssl_prefer_server_ciphers on;
	gzip on;
	gzip_types application/javascript application/json text/css;

	index index.php index.html;

	location / {
		try_files $uri $uri/ /index.php?$query_string;
	}

	location /favicon.ico { access_log off; log_not_found off; }
	location /robot.txt { access_log off; log_not_found off; }

	location ~\.php$ {
		fastcgi_pass unix:/data/data/com.termux/files/usr/var/run/php-fpm.sock;
		fastcgi_index index.php;
		include fastcgi_params;
		fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	}

	location ~ /\.(?!well-known).* {
		deny all;
	}
}
```
