## Tutorial for nginx configuration in Arch Linux

# Requirements:

First we need to make sure that we have vim and nginx installed.
To install vim the command is `sudo pacman -S vim`
To install nginx first make sure you update your system to ensure that your kernel is up to date for packages with `sudo pacman -Syu`
To install nginx use `sudo pacman -S nginx`

# Check the nginx status to see if it's up and running

Use `systemctl status nginx.service` (The .service is optional because it is the default)
It should say inactive right now since we haven't enabled it yet.
To enable it use `sudo systemctl enable --now nginx.service`
This will create a symbolic link.
Then run the previous command to check the status of nginx.service again and this time it should say active and enabled.

# Change the nginx configuration file

Change directories into /etc/nginx by using `cd`
Then, to open the main configuration file use `sudo vim nginx.conf`
We're going to completely remove the contents so in vim you can enter insert mode, then command mode then enter :%d to delete all of the content.
We're going to use this example configuration and paste it into the file:

```
user http;
worker_processes auto;
worker_cpu_affinity auto;

events {
    multi_accept on;
    worker_connections 1024;
}

http {
    charset utf-8;
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    server_tokens off;
    log_not_found off;
    types_hash_max_size 4096;
    client_max_body_size 16M;

    # MIME
    include mime.types;
    default_type application/octet-stream;

    # logging
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log warn;

    # load configs
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

Run `sudo nginx -t` to test to see if there any any syntax errors.
If there are no errors, run `sudo systemctl reload nginx.service` to apply changes.
