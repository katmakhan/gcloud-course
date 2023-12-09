# Hesita Installation
[Hestia Control Panel](https://www.hestiacp.com/)
Alternative for CPanel, create multisite and mail domains using GCP.

## Installation Steps
- Create a FREE Instance in GCP with Ubuntu Image
- SSH Into the machine via browser
- Upgrade priveledge to super user
```
sudo -i
```
- Upgrade and update the packages
```
sudo apt-get update && sudo apt-get upgrade
```
- Download the installed script
```
wget https://raw.githubusercontent.com/hestiacp/hestiacp/release/install/hst-install.sh
```
- if you don't have wget
```
apt-get install wget
```

- Run the installer
```
bash hst-install.sh
```

---

## Creating the hestia panel and basic setting up
- Type `y` to install the dependencies
- Enter the email id for admin
- Enter the FQDN host address as <hcp.domain.com>
- Remember the admin password after the installation


### Setting up new firewall rule
- Choose for all instances
- tcp port `8083` 
- Use tcp port `2083` when using cloudflare
- Ip4 range should be `0.0.0.0/0`

### In cloudflare
- Setup new `A` record with `hcp` and enter the `static ip` you get from GCP, in the instance manager
- Don't use normal IP, that will change, so make sure to change the external IP to `reserve static IP` in `VPC Network` section in GCP

### Login to hestia at the specified port
- Specify the port and also the `login` path
```
https://hcp.<domain.com>:2083/login/
```

## Error Http to Https
If you get error like
```
"The plain HTTP request was sent to HTTPS port"
```
That is basically due to SSL configuration in cloudflare.

- Change the SSL/TLS certificate to `full` and not `flexible`
- Also make sure your port is changed to 2083
```
v-change-sys-port 2083
```
- Restart hestia 
```
service hestia restart
```
- Check if the config file is updated
```
cat /usr/local/hestia/nginx/conf/nginx.conf
```
- It should have
```
 # Vhost
        server {
                listen              8083 ssl;
                server_name         _;
                root                /usr/local/hestia/web;
                # Fix error "The plain HTTP request was sent to HTTPS port"
                error_page          497 https://$host:$server_port$request_uri;
                error_page          403 /error/404.html;
                error_page          404 /error/404.html;
                error_page          410 /error/410.html;
                error_page          500 501 502 503 504 505 /error/50x.html;
                ssl_certificate     /usr/local/hestia/ssl/certificate.crt;
                ssl_certificate_key /usr/local/hestia/ssl/certificate.key;

                location / {
                        expires off;
                        index   index.php;
                }
```
### Setting Up Hestia
- Login to hestia with admin or use the SSH terminal to reset the password
- After login, click on create web domain
- Then it will ask for `Add User` option to prevent global control
- Create a normal user, with some credentials for the `hestia account` with `user previledge` (lesser previledge)
- Then click on `login as <username>`, if you don't see that, logout and login again with `user previledge`
- Click on `Add Web Domain` and `<domain.com>`
- Then go `back`
- Then after that click on `Edit` button.
- Then check `Enable SSL for this domain`
- Also check `Use Lets Encrypt to obtain SSL certificate`
- Also check `Enable automatic HTTPS redirection`
- Then `save`
- If you find connection error, go to `cloudflare` and change the SSL from `flexible` to `strict` mode


### Go and add INGRESS Rule
- go to Compute Instance
```
Compute>Instances>Instance details
```
- go to Attached VNIC on Left side
```
Primary VNIC> subnet >Security List >Default security List
```
- Add Ingress Rules
- Set `Source` CIDR to
```
0.0.0.0/0
```
- Set `description` to
```
hestia
```
- Set IP Protocol to `TCP`
- Set DestinationPort Ranges
```
2083,80,443,143,993,110,995,25,465,587
```

### Install cloudflare origin certificate

- Install the origin certificate inside the server
```
sudo su -
wget https://developers.cloudflare.com/ssl/static/origin_ca_rsa_root.pem
mv origin_ca_rsa_root.pem origin_ca_rsa_root.crt
cp origin_ca_rsa_root.crt /usr/local/share/ca-certificates
update-ca-certificates
```
- Then go to `cloudflare`, to `SSL` section
- click on `origin server`
- click on `create certificate`
- Copy the 	`public` and `private` keys into the hestia dashboard.
- If there is any error, make sure, you installed the certificate installer first
- Then only change the `self signed` keys

### Install Wordpress
- In the Edit option under `Add web domain`, click on `Quick install app`
- Then install wordpress
- Assign the suitable `username` and `password` for the wordpress login

---

## To change the admin passowrd
```
sudo -i
v-change-user-password admin <newpassword>
```

## To change the port from default 8083 to 2083 when using cloudflare
- cloudflare doesn't allow port 8083, so change that port using the command
```
sudo -i
v-change-sys-port 2083
```

## Backup Hestia User
- Login via SSH
- Become root
```
sudo -i
```
- Move into Backups
```
cd backup
```
- Installed Users is in 
```
cd home
```
- Go directly into private folder
```
cd /home/kcymadmin/web/kcymdioceseofpalghat.in/private
```
- For example `/home/kcymadmin/web/kcymdioceseofpalghat.in/private/hestiabackups`
- Copy the files in `FTP` location to actual `backup`
```
cp hestiabackups /backup 
cp * /backup
mv * /backup
```
- If you wanna remove the hestiabackup folder then use `rm -d hestiabackup`

## Restore Hestia user
- Go inside `backup` folder
```
cd /backup
```
- Then restore user
```
sudo /usr/local/hestia/bin/v-restore-user  username username.date.tar
```

## Some things to note while copying and pasting in folders
If you moved, try copying from backup to private folder, that may have to change the file permission to be accesible by filezila or file manager

- To know the chmod
```
stat --format '%a' <file> 
```
- To make them downloadable 640 makes them readable but not downloadable
```
chmod 644 * 
```

## Wordpress 6.1 Customizer missing
go to the following url, to manually access the customizer page, the link is now missing in the new update
```
https://example.com/wp-admin/customize.php
```


## Do not use Updraft Backup, use All in One WP Migration Instead
This step didn't worked for me, always use `all in One WP Migration` to migrate single site GCP into Hestia
- Use Hestia File manager to upload the big files `Import` function inside the plugin doesn't let you upload the whole file, closes suddenly
- Also restoring backup is only on premium, so install the forked all in one project in the repostory that contains the old version


### Some keep points to know, if you are still not listening to me, and wants to take backup with Updraft
Using the backup will trigger the error in hestia, so change the sql MAX_ALLOWED_PACKET to make it work
- open SSH terminal
- sudo -i to get root permission
```
nano /etc/mysql/my.cnf
```

- Change the line: `max_allowed_packet=256M` (obviously adjust size for whatever you need) under the [MYSQLD] section.

- Then press `crtl` +`X` (exit file) and then `Y` then `Enter`

```
service mysql restart
```
### Sometimes "DATABASE PERMISSION ERROR"
- This is caused when mariadb is not working properly, you can login as root and then type 
```
systemctl status mysql
```

If there is an error while running the mariadb, the reason maybe due to insufficient space, all you have to do is clear the old backups and restart the db.

```
systemctl start mysql
```


### Sometimes there is no space in disk
- Check diskspace using
```
df -h
```

- Check details of folder size using
```
du -sh -- *
```


- Delete the temp folders and backups in `journal` and `backups` and `temps`.
- But first you need to login as root by
```
sudo -i
```
- Then delete the `journal`
- Donot delete the `journal`  or `journal/somefoldername`  but you can delete the content inside that `somenamedirector`.
- Donot delete the `directory` but delete the `content` inside

```
cd /var/log/journal
cd /somefoldernameinsidejournal
```
- Then delete the content inside the temp
```
cd /usr/local/hestia/web/fm/private/tmp
```

- Delete command
```
rm *
```
- Also delete the backup of each users than hestia automatically makes from the admin control panel in hestia.


## Adding FTP Clinet
- Under the advance settings
- Create `username` and `password`
- and click `save`
- The username will be `user_ftpusername`

## Add the ftp ports to the `oracle` network policies
- VCN -> networkname -> subnet -> securitylist -> add Ingress Rule
- Keep the protocol as `TCP`
- Then description as `FTP`
- CIDR
```
0.0.0.0/0
```
- Ports
```
20,21,12000-12100
```


## Connect via FileZila
- Host- the Ip address of the oracle machine/ gcp
- ftpusername - that you created (username_ftpusername)
- password
- Leave the rest as default
- You can transfer big files using this method
- Hesita limits upto 1GB with their default file manager
- The filezila protocol gets upto 7.2-10 mbps transfer speeds.
- Even if you try disabling proxing from `cloudflare` or increasing the `apache` config to `10GB` uploads.
- This method works without any hazzle.

  ## Increase upload size in filemanager
  - It defaults at 1GB, but you can change it
    ```shell
    nano /usr/local/hestia/web/fm/configuration.php
    ```
  - Change to make it into 2GB or More
   ```shell
    $dist_config["frontend_config"]["upload_max_size"] = 2* 1024 * 1024 * 1024;
    ```
  
