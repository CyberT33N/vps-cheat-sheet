# VPS Cheat Sheet
VPS Cheat Sheet with the most needes stuff...





# Debian

## Permission

#### Change user/group permission of folder and subfolders
```bash
sudo chown username:group -R * /var/www/html/sample
```

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

#### Create folder
```bash
mkdir foldername
```

#### Delete folder recursive
```bash
rm -rf lampp
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


# Gcloud

## Connect to VM Instance via Putty
https://www.youtube.com/watch?v=fmh94mNQHQc

If you get error **Disconnected: No supported authentication methods available (server sent: publickey)** then go to metadata and change enable-oslogin to **FALSE** (https://cloud.google.com/compute/docs/oslogin/manage-oslogin-in-an-org)



<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# Ubuntu


## Add user
```bash
sudo adduser yourusernamehere
```


## Change password of user
```bash
sudo passwd yourusernamehere
```



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

