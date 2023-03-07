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
- Enter the host address as <hcp.domain.com>
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
v-change-user-password admin <newpassword>
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