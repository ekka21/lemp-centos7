# lemp-centos7

## Housekeeping the "L"

- `sudo yum update`
- `sudo yum install epel-release tmux git`
- `vi ~/.bashrc` #optional
```
alias lsa="ls -la"
alias ..="cd .."
alias www="cd /var/www/domains"
alias reloadngx="sudo systemctl reload nginx"
alias reloadphp="sudo systemctl reload php-fpm"
alias nx="cd /etc/nginx"
alias phpconfig="cd /etc/php-fpm.d/"
```

- `vi ~/.tmux.conf` #optional
```
unbind C-b
set-option -g prefix C-a
bind C-a send-prefix

unbind %
bind s split-window -v
unbind '"'
bind v split-window -h

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
- `sudo systemctl start nginx`
- `sudo systemctl status nginx`
- you should be able to go to http://IP-address

## Install the "P" PHP-FPM
- `sudo yum install php php-mysql php-fpm`
- `sudo systemctl start php-fpm`
- `sudo systemctl status php-fpm`

### Adding a new vhost

- `sudo mkdir -p /var/www/domains/example.com/www/htdocs`
- `sudo vi /var/www/domains/example.com/www/htdocs/index.php`
- `sudo vi /etc/nginx/conf.d/example.conf`

```
server {
    listen       80;
    server_name  example.com www.example.com;

    # note that these lines are originally from the "location /" block
    root   /var/www/domains/example.com/www/htdocs;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php-fpm-example.sock;
        fastcgi_index index.php;
        fastcgi_param
        SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

## Configure PHP-FPM
- `sudo cp /etc/php-fpm.d/www.conf /etc/php-fpm.d/example.conf`
- `sudo vi /etc/php-fpm.d/example.conf`

```
[example-com]

listen = /var/run/php-fpm-example.sock

listen.owner = example-com
listen.group = example-com

user = example-com
group = example-com
```

- `sudo adduser example-com`
- `sudo usermod -aG example-com USER` #appendGroup example-com to USER
- `sudo systemctl reload php-fpm`
- `ps aux | grep php` to check if the socket is running
- `ls -la /var/run/ | grep php`

## Insatll the "M" MySql/MariaDB
- `sudo yum install mariadb-server mariadb`
- `sudo systemctl start mariadb`
- `sudo mysql_secure_installation`
- `mysql -uroot -proot`


