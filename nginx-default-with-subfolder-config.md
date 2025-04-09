## Nginx Default with subfolder config

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/project/public;
    index index.php index.htm index.nginx-debian.html;
    server_name _;

    location / {
        try_files $uri $uri/ /index.php;
    }

    location /blog/ {
        alias /var/www/blog/;
        index index.php index.html index.htm;

        location ~ \.php$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/run/php/php8.3-fpm.sock;
            fastcgi_param SCRIPT_FILENAME $request_filename;
            include fastcgi_params;
        }
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}
```