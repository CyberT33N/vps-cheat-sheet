# VPS Cheat Sheet
VPS Cheat Sheet with the most needes stuff...





# Debian

## Permission

#### screen (managa terminal windows)
- https://www.youtube.com/watch?v=Mw6QvsChxo4
- CTRL+a+c - New terminal window
- CTRL+a+n - Switch between open terminal windows
```bash
# Open again terminals inside of screen after you close your connection
screen -r
```


#### Swap Space
- https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04

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


# Gcloud

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

