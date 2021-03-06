# gitea-nginx-reverse-proxy
gitea and nginx reverse proxy sample config file

```
server {
    if ($host = git.contoh.com) {
        return 301 https://$host$request_uri;
    } 
    listen       80;
    server_name  git.contoh.com;
}

server {
    listen       443 ssl;
    server_name git.contoh.com;
    
    # SSL configuration
    ssl_certificate /etc/letsencrypt/live/git.contoh.com/fullchain.pem; 
    ssl_certificate_key /etc/letsencrypt/live/git.contoh.com/privkey.pem; 
    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout  10m;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
    
    # set max upload to 15 mb
    client_max_body_size 15M; 
    client_body_buffer_size 256K;

    location / { 
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_max_temp_file_size 0;
        proxy_pass http://git.contoh.com:3000;
    } 
}
```
