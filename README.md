# RaspberryPlay

This is a simple project i made to use raspberry pi as multi-services server.

Services i installed : 

+ apache (to share my projects with my friends)
+ samba (i have a 64GB USB and i wanted to use it as online storage device)
+ vsftpd (i need it for wordpress to work)
+ tvheadend (to use my DVB-T dongle online and make it accessible on home network)
+ squid (use it as proxy cache)
+ NoIp (to have a static name to access my services outside)

## Installation

It's simple just follow this steps :

```bash
sudo su
apt-get update 
apt-get upgrade -y
apt install apache2 vsftpd samba tvheadend squid 
```
after that let's install NoIp 

```bash
sudo su
wget https://www.noip.com/client/linux/noip-duc-linux.tar.gz 
tar xvzf noip-duc-linux.tar.gz
cd noip-*
make
make install
```
```bash
root@raspberrypi:/media/noip-2.1.9-1# make install
if [ ! -d /usr/local/bin ]; then mkdir -p /usr/local/bin;fi
if [ ! -d /usr/local/etc ]; then mkdir -p /usr/local/etc;fi
cp noip2 /usr/local/bin/noip2
/usr/local/bin/noip2 -C -c /tmp/no-ip2.conf

Auto configuration for Linux client of no-ip.com.

Multiple network devices have been detected.

Please select the Internet interface from this list.

By typing the number associated with it.
0	eth0
1	wlan1
1 ==> (choose the interface that connected to internet for me it`s wifi)
Please enter the login/email string for no-ip.com  anas******@gmail.com
Please enter the password for user 'anas******@gmail.com'  **********

Only one host [raspberryanas***.ddns.net] is registered to this account.
It will be used.
Please enter an update interval:[30]  1
Do you wish to run something at successful update?[N] (y/N)  N

New configuration file '/tmp/no-ip2.conf' created.

mv /tmp/no-ip2.conf /usr/local/etc/no-ip2.conf

```

To test noip 

```bash
noip2 -S
```

To run it

```bash
noip2
```
## Configuration

now the hard part XD, let's start with apache:

+ check if the service is running by typing : service apache2 status, if it`s says **active (running)** u r good to go, just type [127.0.0.1](http://127.0.0.1) in your browser and it should bring you the apache home page, if it's not running try : service apache2 start.
+ apache directory : /var/www/html/
+ to change the page that shows in [127.0.0.1](http://127.0.0.1) just copy your files to this directory /var/www/html/ 

samba :
+ check service if it running by typing : service smbd status, if not type : service smbd start
+ now type this commands :

```bash
cd /etc/samba
nano smb.conf
```
+ now scroll to the end and add this :
```bash
[devices]  ==> choose a name, u need this when u want to mount this share point
Comment = Pi shared folder ==> a comment it`s unnecessary  
Path = /media  ==> the path you want to share, for me i need to share /media cuz it`s where usb device were mounted
Browseable = yes  ==> to let users see what you have in this share
Writeable = Yes  ==> to let users add files and edit them
only guest = no
create mask = 0777  ==> the new mask for files that add to that folder 
directory mask = 0777
Public = yes  
Guest ok = yes  ==> to allow users that not in your machine
```

+ u have to make that directory writable by everyone 
```bash
sudo su
chmod 777 /media 
```
+ let's add our users 
```bash
sudo su
useradd anas
passwd anas ==> add a password for the user anas
smbpasswd -a anas ==> add a passowrd for SMB 
```
+ now u just need to restart the service by : service smbd restart

squid :
+  configuration file it will be in reporistry and also the original file.

## Real Goal
after finishing everything i will make this as a system u just install it and u r good to go ;)
