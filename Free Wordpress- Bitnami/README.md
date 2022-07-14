#GCLOUD

Modify the following options in the 
/opt/bitnami/php/etc/php.ini file to increase the allowed size for uploads:

Maximum size of POST data that PHP will accept.
post_max_size = 16M

Maximum allowed size for uploaded files.
upload_max_filesize = 16M
  
If the default Web server configuration limits are too low for the file sizes you plan to upload, you can also increase those limits in the /opt/bitnami/apache2/conf/httpd.conf file, by setting the LimitRequestBody parameter to a new value in bytes, as shown below:

LimitRequestBody 16384

Restart the servers for the changes to take effect.
sudo /opt/bitnami/ctlscript.sh restart


Commands
sudo nano /opt/bitnami/php/etc/php.ini
crt+w for search
upload_max_filesize= 16M
Change it
crt+X to Exit
Y to yes


sudo /opt/bitnami/ctlscript.sh restart

