# Bitnami Wordpress Installation 
It will be similar for AWS installation from market place also. Do the following steps to ensure that you can import your backup files and restoring them into the wordpress account.

- Connect the bitnami using SSH via Gcloud with a button
- Connect with aws with private key using
	Go the Specified private key folder in windows or linux,
	```bash
	chmod 400 <file_name>.pem
	```
- After changing the permission of the file 
	You need to use user name as "bitnami" as we installed bitnami.
	If you find the user name to be wrong kindly check with the pre-requesties: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html
	
	Get the default user name for the AMI that you used to launch your instance:
	- For Amazon Linux 2 or the Amazon Linux AMI, the user name is ec2-user.
	- For a CentOS AMI, the user name is centos or ec2-user.
	- For a Debian AMI, the user name is admin.
	- For a Fedora AMI, the user name is fedora or ec2-user.
	- For a RHEL AMI, the user name is ec2-user or root.
	- For a SUSE AMI, the user name is ec2-user or root.
	- For an Ubuntu AMI, the user name is ubuntu.
	- For an Oracle AMI, the user name is ec2-user.
	- For a Bitnami AMI, the user name is bitnami.
	- Otherwise, check with the AMI provider.
	
	```bash
	ssh -i "gt_website.pem" bitnami@ec2-<public_ip>.ap-south-1.compute.amazonaws.com
	```
- After Logged In , Go open the file php.in using nano or vim
	
	Editing in Nano
	```bash
	sudo nano /opt/bitnami/php/etc/php.ini
	```
	OR
	Editing in Vim
	```bash
	sudo vim /opt/bitnami/php/etc/php.ini
	```
	
- Modify the following options in the php.ini file to increase the allowed size for uploads

	- Search for the post max size
	` Ctrl`+`w` to search in nano
	- If you are using SSH Terminal in browser in GCP, that will close the terminal
	- So use the "send key combinations" icon on the top right of the SHH terminal
	
	- Search for
	```console
	post_max_size
	```
	- Change to something larger than your backup file / upload file you want
	```console
	post_max_size=600M
	```
	- Also change the Post size
	```console
	upload_max_filesize = 16M
	```
	
	- After changing in Nano Editor
	`Crtl`+`X` to Exit the nano editor and `Y` to save the edits and then `Enter` to save the file
	or
	
	- After changing in VIM Editor
	Press `Esc` and type `:wq` and then `Enter` to save the file
	
	
- Restart the servers for the changes to take effect.
	```bash
	sudo /opt/bitnami/ctlscript.sh restart
	```
	
- Install All-in-One WP Migration tool in wordpress
	- Upload the <filename>.wpress file
	- Max upload size will be 800MB after you change both `post_max_size` and `upload_max_filesize`
	- wait for the upload to finish
	- Restore the backup
	- For future dev, Take backup and store it in `Locally` or `Google drive`

- Disbale Wordpress using SSH
	- Connect to your server via SSH.
	- Navigate to your websites root folder.
	- Navigate to the /wp-content folder using the cd command.

		```console
		cd /opt/bitnami/wordpress/wp-content
		```
	- Rename the /plugin folder using the mv command.
	
		```console 
		sudo mv plugins plugins.disable
		```
	- All your plugins will now be disabled.
	- Alternatively you navigate inside of the /plugins folder and rename the plugins individually to disable them one at a time.

## Troubleshoooting when installing the backup from the instance

- To check status of bitnami
```bash
sudo /opt/bitnami/ctlscript.sh status
```
- To stop all services
```bash
sudo /opt/bitnami/ctlscript.sh stop
```
- To restart the scripts
```bash
sudo /opt/bitnami/ctlscript.sh restart
```

## Houzez Theme Requirements
Recommended PHP Configuration Limits
```
max_execution_time 600
memory_limit 128M
post_max_size 48M
upload_max_filesize 48M
```

## To troubleshoot wordpress plugin or theme installation
- Edit the file `wp-config.php`
- Change the debug to `true`
```
('WP_DEBUG',false);
```

