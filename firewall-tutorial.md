## Adding a firewall and reverse proxy

# Requirements:

Make sure you have ufw installed using `sudo pacman -S ufw`
Then `cd` into /etc/nginx/sites-enabled and add this into your server block location like so to configure the proxy settings:

```
location /hey {
        # Define the reverse proxy settings
        proxy_pass http://142.232.219.130;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

location /echo {
        # Define the reverse proxy settings
        proxy_pass http://142.232.219.130;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }


```

# Create a new service file with this as an example of content

```
[Unit]
Description=Backend Service
After=network.target

[Service]
Type=simple
ExecStart=/etc/systemd/system/hello-server
Restart=always

[Install]
WantedBy=multi-user.target
```

Then start and enable the service file with these commands

```
sudo systemctl start backend
sudo systemctl enable backend
```

Test out your connections to your backend with: `curl http://146.190.12.184`, `curl http://146.190.12.184/hey` and

````curl -X POST -H "Content-Type: application/json" \
  -d '{"message": "Hello from your server"}' \
  http://146.190.12.184/echo``` using your own ip address
````
