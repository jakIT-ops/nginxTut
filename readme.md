## NGINX 

### 1. Basic server block:

```
server {
    listen 80;
    server_name example.com;
    root /var/www/example.com;
    index index.html;
}
```

### 2. Redirect HTTP to HTTPS:

```
server {
    listen 80;
    server_name example.com;
    return 301 https://$host$request_uri;
}
```

### 3. SSL configuration:

```
server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;

    root /var/www/example.com;
    index index.html;
}
```

### 4. Proxy pass to a backend server: 

```
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend_server;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### 5. Load balancing:

```
http {
    upstream backend {
        server backend1.example.com;
        server backend2.example.com;
    }

    server {
        listen 80;
        server_name example.com;

        location / {
            proxy_pass http://backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }
}
```

### 6. Serving static files with cache control: 

```
server {
    listen 80;
    server_name example.com;
    root /var/www/example.com;

    location ~* \.(jpg|jpeg|gif|png|css|js)$ {
        expires 30d;
    }
}
```

### 7. Restrict access IP address: 

```
server {
    listen 80;
    server_name example.com;
    root /var/www/example.com;

    location /admin {
        allow 192.168.1.0/24;
        deny all;
    }
}
```

### 8. Basic authentication 


```
server {
    listen 80;
    server_name example.com;
    root /var/www/example.com;

    location /secure {
        auth_basic "Restricted";
        auth_basic_user_file /etc/nginx/.htpasswd;
    }
}
```

### 9. Rate limiting 

```
http {
    limit_req_zone $binary_remote_addr zone=mylimit:10m rate=1r/s;

    server {
        listen 80;
        server_name example.com;
        root /var/www/example.com;

        location / {
            limit_req zone=mylimit burst=5;
        }
    }
}
```



