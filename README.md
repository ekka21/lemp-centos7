# lemp-centos7

## Housekeeping the "L"

- `sudo yum update`
- `sudo yum install epel-release`

## Install and configure the "E" - Nginx

- `sudo yum install nginx`
- `sudo service nginx start`
- `sudo vi /etc/nginx/conf.d/example.conf`
```
server {
    listen       80;
    server_name  local.example.com

    # note that these lines are originally from the "location /" block
    root   /var/www/domains/example.com/www/htdocs;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param
        SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```
- `sudo mkdir -p /var/www/domains/example.com/www/htdocs`
- `sudo vi /var/www/domains/example.com/www/htdocs/index.php`
```
<?php phpinfo();
```
## Insatll and configure the "M" MySql/MariaDB
- `sudo yum install mariadb-server mariadb`
- `sudo systemctl start mariadb`
- `sudo mysql_secure_installation`
- `mysql -uroot -proot`

## Install and configure the "P" PHP-FPM
- `sudo yum install php php-mysql php-fpm`
- `sudo service php-fpm start`
- `php -v`
