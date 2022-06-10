# Ubuntu - NO MACHINE- RDC

------------------------------
VM INSTANCE WITH RDC in GCLOUD
------------------------------

windows server 2019 Datacenter (Not FREE)
ubuntu 18.04 (FREE)

Create VM instances 0:40
Connect with SSH 
sudo apt-get update
sudo apt-get upgrade

sudo apt-get install ubuntu-desktop (IF YOU FIND ERRROR DO THE FOLLOWING)


sudo su
apt-get clean
cd /var/lib/apt
mv lists lists.old
mkdir -p lists/partial
apt-get clean
apt-get update

WHEN DONE RUN AGAIN sudo apt-get install ubuntu-desktop


nomachine > Linux > X86> DEBIAN
https://download.nomachine.com/download/7.9/Linux/nomachine_7.9.2_1_amd64.deb
sudo dpkg -i nomachine_7.9.2_1_amd64.deb

sudo reboot

sudo -s (For login to root or super user)
useradd <username> (To add new user, Only root can add users)
usermod -a -G sudo,adm <username> (To give admin and sudo persmissions)

THEN VPC NETWORK -> FIREWALL RULE -> name and Target tag to nomachine-fw ,
Then couse IP RANGES 0.0.0.0/0
Then Specified Protocol tcp 4000

then Save

Then go to VM INSTANCE DETAILS> CHange allowed network tags , add "nomachine-fw"


INSTALL NOMACHINE IN YOUR SYSTEM