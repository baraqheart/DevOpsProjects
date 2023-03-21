# Deploy  a wordpress site on ec2

# Install and configure WordPress

- WordPress is the most popular open-source blogging system and CMS on the Web. It is based on PHP and MySQL. Its features can be extended with thousands of free plugins and themes.

# 2. Install Dependencies

```
sudo apt update
sudo apt install apache2 \
                 ghostscript \
                 libapache2-mod-php \
                 mysql-server \
                 php \
                 php-bcmath \
                 php-curl \
                 php-imagick \
                 php-intl \
                 php-json \
                 php-mbstring \
                 php-mysql \
                 php-xml \
                 php-zip
```

# 3. Install WordPress

```
sudo mkdir -p /srv/www
sudo chown www-data: /srv/www
curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www
```

- Note that this sets the ownership to the user `www-data` , which is potentially insecure, 
- such as when your server hosts multiple sites with different maintainers.
- You should investigate using a user per website in such scenarios and make the files readable and writable to only those users. 
- This will require configuring PHP-FPM to launch a separate instance per site each running as the site’s user account. 
- In such setup the `wp-config.php` should (read: if you do it differently you need a good reason) be readonly to the site owner
- and group and other permissions set to no-access `(chmod 400)` .
- This is beyond the scope of this guide, however.

# 4. Configure Apache for WordPress

- Create Apache site for WordPress. Create `/etc/apache2/sites-available/wordpress.conf ` with following lines:

```
<VirtualHost *:80>
    DocumentRoot /srv/www/wordpress
    <Directory /srv/www/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Require all granted
    </Directory>
    <Directory /srv/www/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>

```

- Enable the site with:

```
sudo a2ensite wordpress
```

- Enable URL rewriting with:

```
sudo a2enmod rewrite
```

- Disable the default “It Works” site with:

```
sudo a2dissite 000-default
```

- Finally, reload apache2 to apply all these changes:
```
sudo service apache2 reload
```



