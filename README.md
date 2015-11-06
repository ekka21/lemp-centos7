# lemp-centos7

## Housekeeping the "L"

- `sudo yum update`
- `sudo yum install epel-release tmux git`
- `vi ~/.bashrc`
```
alias lsa="ls -la"
alias ..="cd .."
alias www="cd /var/www/domains"
alias reloadngx="sudo systemctl reload nginx"
alias reloadphp="sudo systemctl reload php-fpm"
alias nx="cd /etc/nginx"
alias phpconfig="cd /etc/php-fpm.d/"
```

- `vi ~/.tmux.conf`
```
unbind C-b
set-option -g prefix C-a
bind C-a send-prefix

unbind %
bind s split-window -v
unbind '"'
bind v split-window -h
.
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# Make mouse useful in copy mode
setw -g mode-mouse on
set -g mode-mouse on

# Allow mouse to select which pane to use
set -g mouse-select-pane on

# Allow mouse to resize pane
set -g mouse-resize-pane on
```

## Install the "E" - Nginx

- `sudo yum install nginx`
- `sudo service nginx start`
- `sudo vi /etc/nginx/conf.d/example.conf`
```
server {
    listen       80;
    server_name  dev.example.com;

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
## Insatll the "M" MySql/MariaDB
- `sudo yum install mariadb-server mariadb`
- `sudo systemctl start mariadb`
- `sudo mysql_secure_installation`
- `mysql -uroot -proot`

## Install the "P" PHP-FPM
- `sudo yum install php php-mysql php-fpm`
- `sudo service php-fpm start`
- `php -v`

## Configure PHP-FPM
- `sudo vi /etc/php-fpm.d/www.conf`
- 
```
; Start a new pool named 'www'.
[www]

; The address on which to accept FastCGI requests.
; Valid syntaxes are:
;   'ip.add.re.ss:port'    - to listen on a TCP socket to a specific
address on
;                            a specific port;
;   'port'                 - to listen on a TCP socket to all addresses
on a
;                            specific port;
;   '/path/to/unix/socket' - to listen on a unix socket.
; Note: This value is mandatory.
listen = /var/run/php5-fpm.sock 

; Set listen(2) backlog. A value of '-1' means unlimited.
; Default Value: -1
;listen.backlog = -1

; List of ipv4 addresses of FastCGI clients which are allowed to
connect.
; Equivalent to the FCGI_WEB_SERVER_ADDRS environment variable in
the original
; PHP FCGI (5.2.2+). Makes sense only with a tcp listening
socket. Each address
; must be separated by a comma. If this value is left blank,
connections will be
; accepted from any ip address.
; Default Value: any
listen.allowed_clients = 127.0.0.1

; Set permissions for unix socket, if one is used. In Linux, read/write
; permissions must be set in order to allow connections from a web
server. Many
; BSD-derived systems allow connections regardless of permissions.
; Default Values: user and group are set as the running user
;                 mode is set to 0666
listen.owner = nginx 
listen.group = nginx 
;listen.mode = 0666

; Unix user/group of processes
; Note: The user is mandatory. If the group is not set, the default
user's group
;       will be used.
; RPM: apache Choosed to be able to access some dir as httpd
user = nginx
; RPM: Keep a group allowed to write in log dir.
group = nginx 
```
