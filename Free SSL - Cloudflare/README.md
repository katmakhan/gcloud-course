# CloudFlare SSL

## Adding cloudflare certificate Authority by using the command
- Use `sudo su -` to swtich to root user before running the following command.
```
wget https://developers.cloudflare.com/ssl/static/origin_ca_rsa_root.pem

mv origin_ca_rsa_root.pem origin_ca_rsa_root.crt

cp origin_ca_rsa_root.crt /usr/local/share/ca-certificates

update-ca-certificates
```
## SSL Origin Server
- Click create new certificate
- Default settings
- Copy the public and private key 
- Paste them into hestia control panel, Settings-> Configure-> SSL settings
- Then click save
- If you find, `authority error`, please install the cloudflare CA using the above commands.
