# seafile-server-community_debian-jessie-amd64
This installer script offers a quick and easy way to set up the [Seafile Server Community Edition](http://seafile.com/en/home/) on a [Debian Jessie](https://www.debian.org/releases/stable/) (64bit), using MariaDB, Memcached and NGINX as a Reverse Proxy in under 5 minutes. We're also adding an init script that starts Seafile when starting the server.

### What's it for?
Installing the [Seafile Server Community Edition](http://seafile.com/en/home/) on a [Debian Jessie](https://www.debian.org/releases/stable/) (64bit) in a standard and more secure manner then the out of the box setup scripts offer. Instead of SQLite we're using MariaDB and instead of Gunicorn we're using NGINX.


### Why?
There are just too many ways to misconfigure a manual Seafile server installation. We've notices that many people don't realize that the default installation is unsecure. In other cases people get stuck during the installation or forget a step like changing "FILE_SERVER_ROOT" or use IP addresses instead of resolvable DNS names.
This script is derived from our [reference installation for Seafile Server  Professional](https://wiki.seafile.com.de/doku.php?id=debian_7_wheezy_64bit). It helps us to standardize the installation procedure and helps with identifying debugging errors easier.


### How long does the installation take?
The installation time will vary depending on your internet connection speed and hardware. On our SSD-based servers we were able to install a production-ready Seafile server in roughly 5 minutes.


### Operating system
It's meant to run on a [Debian Jessie minimal installation](https://www.youtube.com/watch?v=BCwz9oSSt8g). No desktop environment or other weird stuff like hosting panels (Plesk, ISPConfig, etc.)...


### Which components are used?
1. [Newest Seafile Server Community Edition](https://download.seafile.com.de/)
2. [NGINX](http://nginx.org/packages/mainline/debian/)
3. MariaDB
4. Memcached


### Which special features are enabled by default?
1. HTTPS-Proxy on port 443
2. Forced redirect from unenrypted port 80 to port 443
2. Seahub with FastCGI
3. [SeafDAV](http://manual.seafile.com/extension/webdav.html) with FastCGI
4. Memcached
5. Seafile autostart


### What does the installer do?
1. Update and upgrade Debian
2. Install NGINX from http://nginx.org/packages/mainline/debian/
3. Create Seafile init script and add it to systemd startup
4. Create unprivilieged system user "seafile" with home directory "/opt/seafile"
5. Download and extract latest Seafile server sources from https://download.seafile.com.de/seafile-server_latest_x86-64.tar.gz
6. Create "seafile" database user and Seafile databases (DB-Credentials in ~/.my.cnf)
7. Initialize various Seafile files
8. Enable SeafDAV, Memcached and MariaDB
9. Setup Seafile to listen to IP (hostname -i) instead of DNS (*Switch this to DNS before live deployment*)
10. Create Seafile admin with individual password
11. Display setup infos and what to do next.


### How do I run it?
<pre>
cd /tmp
wget --no-check-certificate https://raw.githubusercontent.com/alexanderjackson/seafile-server-community_debian-jessie-amd64/master/seafile-server-community_debian-jessie-amd64
time bash seafile-server-community_debian-jessie-amd64
</pre>


### Caution!
Never run the script on a production server. It's more or less a one trick pony and can seriously damage productions systems. So run it only one time and delete it afterwards. As a precaution I have added a few simple checks to abort installation if the unprivileged Seafile user or Seafile installation directory pre-exist:
1. Check if you're running this acript as root. If not, abort.
2. Check if the user "seafile" exists. If yes, abort.
3. Check if the directory "/opt/seafile" exists. If yes, abort.


### How do I update Seafile?
You will have to handle Seafile updates/upgrades manually after the initial installation. Consult http://manual.seafile.com/deploy/upgrade.html on how to upgrade Seafile. 


### What do I have to do after the installer has finished?
1. Delete installer script. You wont need it anymore and might even seriously damage your system if you ran it again.
2. Follow the suggested steps at the end of the installation to finalize your Seafile server installation. As a bare minimum you most definitely will want to change the listening IP to a valid DNS name. Run "seafile-server-change-address" as root to change the DNS name...
3. Install a firewall. TCP-Port 443 needs to be accessible. Optionally you can open TCP-Port 80 which redirects to HTTPS


### Where can I submit bugs or add suggestions?
Contact me at alexander.jackson@seafile.com.de or create an [Issue](https://github.com/alexanderjackson/seafile-server-community_debian-jessie-amd64/issues/new) on Github.
