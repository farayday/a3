## Tutorial on how to serve static content

# Write a script for setup:

```
#!/bin/bash

# Install necessary software
sudo pacman -S nginx

# Create directories
sudo mkdir -p /etc/nginx/sites-available /etc/nginx/sites-enabled /srv/2420-files

# Create server block configuration using heredoc
sudo bash -c 'cat << EOF > /etc/nginx/sites-available/static-server
server {
    listen 80;
    server_name ray;

    location / {
        autoindex on;
        root /srv/2420-files;
    }
}
EOF'

# Enable the server block
sudo ln -s /etc/nginx/sites-available/static-server /etc/nginx/sites-enabled/

# Test Nginx configuration and restart
sudo nginx -t
sudo systemctl restart nginx
```

Make sure it's executable by running `sudo chmod +x setup`
Run the file by running `./setup`

Then go to a browser and enter the ip address of your virtual machine and you should see your files from the directory of `/srv/2420-files`
