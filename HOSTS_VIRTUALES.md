

```
<VirtualHost *:80>
    ServerAdmin admin@lasucreña.com
    ServerName lasucreña.com
    DocumentRoot /var/www/lasucreña

    <Directory /var/www/lasucreña>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    <Directory /var/www/html/api-restful-basica/data>
        AllowOverride None
        Require all granted
    </Directory>

    <Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
