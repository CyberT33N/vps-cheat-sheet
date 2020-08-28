# VPS Cheat Sheet
VPS Cheat Sheet with the most needes stuff...





# Debian

## Restart server
```bash
sudo shutdown -r now
```


## Utils

#### Check current disc storage
```bash
df -h
```
#### Swap Space
- https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04



#### Convert dos 2 unix
When you create files on windows there are different new lines than on mac/linux! You can convert these files by using dos2unix
```bash
# convert all files in folder recursive
#method 1
find . -type f -exec dos2unix {} ;
#method 2
find . -type f -print0 | xargs -0 dos2unix

# Or manually each file
dos2unix thescript.sh
```






## Monitor


#### screen (managa terminal windows)
- https://www.youtube.com/watch?v=Mw6QvsChxo4
- CTRL+a+c - New terminal window
- CTRL+a+n - Switch between open terminal windows
```bash
# Open again terminals inside of screen after you close your connection
screen -r
```








#### Create folder
```bash
mkdir foldername
```

#### Delete folder recursive
```bash
rm -rf lampp
```










## Permission



#### Change user/group permission of folder and subfolders
```bash
sudo chown username:group -R * /var/www/html/sample
```


#### Add user
```bash
sudo adduser yourusernamehere
```


#### Change password of user
```bash
sudo passwd yourusernamehere
```



<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />



# Domain

## Connect domain to server using Apache
Inside of your domain website settings change:
```bash
record: a
host: @
value: 123.45.67.89


record: a
host: www
value: 123.45.67.89
```

Next create virtual host
```bash
cd /etc/apache2/sites-available
touch cybert33n.com.conf
```

Inside of this file:
```bash

<VirtualHost *:80>


    DocumentRoot /var/www/html/cybert33n

    ServerName cybert33n.com
    ServerAlias www.cybert33n.com

    ErrorLog ${APACHE_LOG_DIR}/error_cybert33n.log
    CustomLog ${APACHE_LOG_DIR}/access_cybert33n.log combined

</VirtualHost>
```

Then enable the virtualhost file:
```bash
sudo a2ensite cybert33n.com.conf
```

to disable you can run:
```bash
sudo a2dissite cybert33n.com.conf
```

After this restart apache server:
```bash
sudo service apache2 restart
```




<br />
<br />


## Subdomains



Domain register create a record  with subdomain pointed to ip adress of server:
```bash
record: a
host: resume
value: 123.45.67.89
```


Create virtualhost file:
```bash
cd /etc/apache2/sites-available
touch resume.cybert33n.com.conf
```





Edit virtual host file:
```bash
<VirtualHost *:80>

    DocumentRoot /var/www/html/resume
    ServerName resume.cybert33n.com
    ServerAlias www.resume.cybert33n.com

    ErrorLog ${APACHE_LOG_DIR}/error_resume_cybert33n.log
    CustomLog ${APACHE_LOG_DIR}/access_resume_cybert33n.log combined

</VirtualHost>
```





Then enable the virtualhost file:
```bash
sudo a2ensite resume.cybert33n.com.conf
```

After this restart apache server:
```bash
sudo service apache2 restart
```



## SSL
You must have domains setup already of course with virtual hosts. Then run:
```bash
sudo apt-get update
sudo apt-get install python-letsencrypt-apache
sudo letsencrypt --apache -d cybert33n.com -d www.cybert33n.com

# test if auto renew is enabled
sudo certbot renew --dry-run
```



<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />


# PHP


## PHP not working on website but it´s installed


Edit this file **/etc/apache2/apache2.conf** with your favorite editor, and add at the bottom:
```bash
# enable executing php files
<FilesMatch \.php$>
SetHandler application/x-httpd-php
</FilesMatch>
```

Then run:
```bash
sudo a2dismod mpm_event && sudo a2enmod mpm_prefork && sudo a2enmod php7.2 (replace php7.0 with whatever version you has)
sudo systemctl restart apache2
```


<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# Apache

## Restart Apache
```bash
# method 1
systemctl reload apache2

# method 2
sudo service apache2 restart
```

<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />


# Google Cloud

## Socket hang up error at express
- In some cases you may Socket hang up errors when you use port 80 for your express server. Make sure to use a different port like as example 1337 (https://www.youtube.com/watch?v=JmjqPpQdtW8)

## Connect to VM Instance via Putty
https://www.youtube.com/watch?v=fmh94mNQHQc

If you get error **Disconnected: No supported authentication methods available (server sent: publickey)** then go to metadata and change enable-oslogin to **FALSE** (https://cloud.google.com/compute/docs/oslogin/manage-oslogin-in-an-org)


## Backup VM Instance
- https://www.cloudbooklet.com/how-to-backup-google-cloud-compute-engine/
- https://cloud.google.com/compute/docs/disks/create-snapshots?hl=en

<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />


# Webmin

## Build SMTP mail server
- https://www.youtube.com/watch?v=ayalsV9iPws


## Remove Google Auth
```bash
sudo sed -i 's/totp//g' /etc/webmin/miniserv.users
sudo sed -i '/twofactor_provider=totp/d' /etc/webmin/miniserv.conf
sudo /etc/init.d/webmin restart
```

## SSL on webmin login
Create a virtualhost on your server and ssl it with letsencrypt
<br />once its running  go to  webmin > webmin configuration > ssl encryption > lets encrypt and then select your domain
<br />webmin will download and use the already setup ssl certificate..


## SSL menu can not be found
Go to system settings > features and plugins and enable ssl on websites 









## Get better email score for own email from smtp 
- https://www.youtube.com/watch?v=N7BmgJWnztk
<br /><br />
SPF, DKIM, rDNS, MX is needed

<br /><br />
MX:
- https://www.namecheap.com/support/knowledgebase/article.aspx/322/2237/how-can-i-set-up-mx-records-required-for-mail-service

<br /><br />
go to mail settings and choose custom mx record:
<br />type: mx record
<br />host: @
<br />value: mail.cybert33n.com
<br />priority: 1
<br />ttl: automatic





<br /><br />
DKIM:
<br />virtualmin > email settings > domainkey identifier
<br />Signing of outgoing mail enabled?	 Yes
<br />add main domain and sub domain at domain field






<br /><br />
reverse dns:
<br />reverse dns can be done on digital ocean by change droplet name to the domain name.
















<br /><br />
SPF:
Server Configuration > DNS OPTIONS
<br />SPF record enabled?	 Yes
<br />allowed sender hostnames, domain and included domains  add domain and sub domain
<br />DMARC record enabled?	 Yes
<br />DNSSEC signature enabled?	 Yes
<br /><br />System settings > server template > default settings > BIND DNS DOMAIN
<br />Hostname for MX record	 Default 
<br />Master DNS server hostname	 Hostname(enter main domain which webmin is using)
<br />Add SPF DNS record?	  Yes, with server's IP address
<br />additioanal spf ips + addtional spf included domain add main domain and subdomain
<br />Does SPF record cover all senders? Yes, and deny other senders
<br />Add DMARC DNS record?	 Yes, with policy below
<br />Create DNSSEC key and sign new domains?	 yes
<br />DNSSEC cryptographic algorithm     rssha256



<br /><br />
DNSSEC: activate at domain hoster at dns section


<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# Node.js

## Install Node.js (Ubuntu/Linux Mint)
```bash
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
```

## Install express.js
```bash
npm i express
```

## Where to locate Node.js projects
The /opt directory is a good location for the program distribution files. The /srv directory is used for the programs run-time data. (Please see the Filesystem Hierarchy Standard.) Unlike the /etc directory where the standard indicates that the /opt/<pkg> configuration files should be placed in /etc/opt/<pkg>, there is no standardization that /srv/opt/<pkg> should be a parallel structure (although it's probably not a bad idea).
```bash
/opt/webserver/     (your node.js application)
    server.js
    package.json
    node_modules/
    ...

/etc/opt/webserver/
    config.json     (configuration file for your web server)

/srv/opt/webserver/ (opt subdirectory suggested, but not required)
    index.html
    images/
    css/
    ...

/var/opt/webserver
    error.log
    request.log
```





<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />


# .htaccess


## Allow specific IP
```bash
# method 1
order deny,allow
deny from all
allow from <your ip> 

# method 2
<RequireAll>
    Require ip xx.xx.xx.xx yy.yy.yy.yy
</RequireAll>
```

## Secure Wordpress
```bash
# disable directory browsing
Options All -Indexes

<files ~ "^.*\.([Hh][Tt][Aa])">
order allow,deny
deny from all
satisfy all
</files>


# Don't show errors which contain full path diclosure (FPD)
# Use that line only if PHP is installed as a module and not per CGI
# try using a php.ini in that case.
# Change mod_php5.c to mod_php7.c if you are running PHP7
<IfModule mod_php5.c>
  php_flag display_errors Off
</IfModule>

# Don't list directories
<IfModule mod_autoindex.c>
  Options -Indexes
</IfModule>

# Protect XMLRPC (needed for Apps, Offline-Blogging-Tools, Pingback, etc.)
# If you use that, these tools will not work anymore
<Files xmlrpc.php>
  Order Deny,Allow
  Deny from all
</Files>

# If you don't use the Database Optimizing and Post-by-Email features, turn off the access too:
<FilesMatch "(repair|wp-mail)\.php">
    Order Deny,Allow
    Deny from all
</FilesMatch>

# Prevent browser and search engines to request .log (e.g. WP DEBUG LOG) and .txt (e.g. plugins readme) files.
# Must be placed in /wp-content/.htaccess
<FilesMatch "\.(log|txt)$">
    Order Allow,Deny
    Deny from all
</FilesMatch>

# Hide WordPress, system & sensitive files
<FilesMatch "(^\.|wp-config(-sample)*\.php)">
    Order Deny,Allow
    Deny from all
</FilesMatch>

# Protect some other files
<FilesMatch "(liesmich.html|readme.html|(.*)\.ttf|(.*)\.bak)">
  Order Deny,Allow
  Deny from all
</FilesMatch>

# Block the include-only files.
# Do not use in Multisite without reading the note in Codex!
# See: https://codex.wordpress.org/Hardening_WordPress#WP-Includes
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^wp-admin/includes/ - [F,L]
  RewriteRule !^wp-includes/ - [S=3]
  # If you run multisite, comment the next line (see note above)
  RewriteRule ^wp-includes/[^/]+\.php$ - [F,L]
  RewriteRule ^wp-includes/js/tinymce/langs/.+\.php - [F,L]
  RewriteRule ^wp-includes/theme-compat/ - [F,L]
</IfModule>

# Set some security related headers
# See: http://de.slideshare.net/walterebert/die-htaccessrichtignutzenwchh2014 (GERMAN)
<IfModule mod_headers.c>
  Header set X-Content-Type-Options nosniff
  Header set X-XSS-Protection "1; mode=block"
  # The line below is an advanced method for a more secure configuration, please see documentation before usage!
  # Introduction: https://scotthelme.co.uk/content-security-policy-an-introduction/
  # http://www.heise.de/security/artikel/XSS-Bremse-Content-Security-Policy-1888522.html (German)
  # Documentation: https://content-security-policy.com/
  # Analysis: https://securityheaders.io/
  # Header set Content-Security-Policy "default-src 'self'; img-src 'self' http: https: *.gravatar.com;"
</IfModule>

# Allow WordPress Embed
# https://gist.github.com/sergejmueller/3c4351ec29576fb441fe
<IfModule mod_setenvif.c>
    SetEnvIf Request_URI "/embed/$" IS_embed
    <IfModule mod_headers.c>
    	Header set X-Frame-Options SAMEORIGIN env=!REDIRECT_IS_embed
    </IfModule>
</IfModule>

#Force secure cookies (uncomment for HTTPS)
<IfModule mod_headers.c>
  #Header edit Set-Cookie ^(.*)$ $1;HttpOnly;Secure
</IfModule>

#Unset headers revealing versions strings
<IfModule mod_headers.c>
  Header unset X-Powered-By
  Header unset X-Pingback
  Header unset SERVER
</IfModule>

# Filter Request Methods
# See: https://perishablepress.com/disable-trace-and-track-for-better-security/
<IfModule mod_rewrite.c>
  RewriteEngine on
  RewriteCond %{REQUEST_METHOD} ^(TRACE|DELETE|TRACK) [NC]
  RewriteRule ^(.*)$ - [F,L]
</IfModule>
# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>

# END WordPress

```
