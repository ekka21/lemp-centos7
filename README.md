# lemp-centos7

## Housekeeping the "L"

- `sudo yum update`

## Install and configure the "E" - Nginx

- `sudo yum install nginx` 
- `sudo service nginx start`
- `sudo vi /etc/nginx/conf.d/example.conf 
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

## Insatll and configure the "M" MySql/MariaDB
- `sudo yum install mariadb-server mariadb`
- `sudo service start mariadb`
- `sudo mysql_secure_installation`

## Install and configure the "P" PHP-FPM
- `sudo yum install php php-mysql php-fpm php-cli`
- `sudo service php-fpm start`

