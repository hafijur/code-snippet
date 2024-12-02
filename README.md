### Apache proxy for nodejs and socket IO default config file 000-default.conf 
```
<VirtualHost *:80>

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html



    # Proxy configuration
    ProxyRequests Off
    ProxyPass /backend http://localhost:5000
    ProxyPassReverse /backend http://localhost:5000


    # Proxy configuration for WebSocket connections
    ProxyPass /socket.io/ ws://localhost:5000/socket.io/
    ProxyPassReverse /socket.io/ ws://localhost:5000/socket.io/


    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    # Optional: Allow traffic from specific IP addresses
    <Proxy *>
        Order deny,allow
        Allow from all
    </Proxy>


        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf

</VirtualHost>

```
### NVM node full install on single command

```
sudo git clone https://github.com/nvm-sh/nvm.git /usr/local/nvm \
&& sudo sh -c 'echo "source /usr/local/nvm/nvm.sh" >> /etc/profile' \
&& source /etc/profile \
&& nvm install --lts \
&& n=$(which node);n=${n%/bin/node}; chmod -R 755 $n/bin/*; sudo cp -r $n/{bin,lib,share} /usr/local
```

### Apache proxy for nextjs and subfolder exclude from nextjs route

```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html

    # Exclude /admin from being proxied to Next.js
    ProxyPass "/backend" !

    # Proxy all other requests to the Next.js app
    ProxyPass "/" "http://137.184.151.147:8001/"
    ProxyPassReverse "/" "http://137.184.151.147:8001/"

    # Serve /admin requests from the PHP application
    Alias /backend /var/www/html/backend
    <Directory "/var/www/html/backend">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

</VirtualHost>

```

# For laravel permission change and git untrack file issue do those steps

- Run this this command
    ```
    git config core.fileMode false
    ```
- Update .gitignore file
    ```
    /storage/*
    !/storage/.gitignore
    /bootstrap/cache/*
    !/bootstrap/cache/.gitignore
    ```
- Use this for web users
    ```
    sudo chmod -R 775 storage/ bootstrap/cache
    sudo chown -R www-data:www-data storage/ bootstrap/cache
    ```
- Or do this 
    ```
    sudo chmod -R 777 storage/ bootstrap/cache
    ```
# Check the MYSQL Authentication Method

    ```
    sudo mysql
    SELECT user, host, plugin FROM mysql.user
    ```
# Change the Authentication Method to Password

    ```
    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_password';
    ```