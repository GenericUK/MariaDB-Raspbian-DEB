# MariaDB Raspbian DEB
**Details**<br>
Raspberry Pi Raspbian compiled DEB Debian Package of MariaDB MySQL replacement.
<br>Compiled with checkinstall on Pi2 | Raspbian GNU/Linux 7 (wheezy) | Kernel 3.18.11-v7+

**Introduction**<br>
Compiling MariaDB was a bit of a trick on Raspbian, as it requires gcc & g++ packages to be upgraded to 4.8 which isn't available in wheezy/Raspian 7, it also takes several hours and requires about 2Gb on your wee Pi. Shame to waste my work so its here if you need it.

**PLEASE MAKE SURE YOU BACKUP YOU SYSTEM ESPECIALLY ANY EXISTING MYSQL DATABASES, THIS INSTALLED ON MY SYSTEM FINE BUT THAT DOESN'T MEAN IT WILL ON YOURS.**

# PRE INSTALL INSTRUCTIONS

mysqldump -u root -p --all-databases > mysqlbackup.sql<br>
If your on a live production server you'll probably want to stop your web server now<br>
sudo service mysql stop **[OR]** sudo /etc/init.d/mysql stop

**Uninstall MySQL (if installed)**<br>
Because MariaDB is a drop in replacement for MySQL most of the libraries should work, however apt-get did want to uninstall most of it so I let it and also ran auto-remove just to finish the job. 

Don't purge if you want to keep your existing MySQL config's and databases. I kept all mine.

sudo apt-get remove mysql-server mysql-client-5.5 mysql-common mysql-server-5.5 mysql-server-core-5.5<br>
sudo apt-get autoremove **[optional]**

# INSTALL

sudo dpkg -i /DOWLOAD_LOCATION/mariadb_10.0.20-1_armhf.deb

# POST INSTALL

**MariaDB default location is /usr/local/mysql and the executables are in /usr/local/mysql/bin. On Raspbian/Debian they would have been in /usr/bin, so we need to update our MariaDB/MySQL config files if your replacing MySQL.**

**If your not replacing an existing MySQL install you will need to get conf/init files from https://github.com/allfs/mariadb/tree/master/debian/additions

sudo nano /etc/mysql/my.cnf
* basedir = /usr/local/mysql
* lc-messages-dir = /usr/local/mysql/share

sudo nano /etc/mysql/debian-start
* source /usr/local/mysql/debian-start.inc.sh
* MYSQL="/usr/local/mysql/bin/mysql --defaults-file=/etc/mysql/debian.cnf"
* MYADMIN="/usr/local/mysql/bin/mysqladmin --defaults-file=/etc/mysql/debian.cnf"
* MYUPGRADE="/usr/local/mysql/bin/mysql_upgrade --defaults-extra-file=/etc/mysql/debian.cnf"

**Now we need to add a replacement debian-start.inc.sh which will be missing after the uninstalling MySQL**

sudo wget --no-check-certificate<br> https://raw.githubusercontent.com/allfs/mariadb/master/debian/additions/debian-start.inc.sh -P /usr/local/mysql/

We now need to symlink to libmysqlclient if it was uninstalled

sudo ln -s /usr/local/mysql/lib/libmysqlclient.so.18 /usr/lib/libmysqlclient.so.18

**Right we also need to add the new bin location to bashrc**

nano .bashrc
* MYSQL_MARIADB_BIN=/usr/local/mysql
* PATH=$MYSQL_MARIADB_BIN/bin:$PATH
source .bashrc

**I also had to create a socket location, this may get done on a reboot but who wants to do that.**

sudo mkdir /var/run/mysqld<br>
chown -R mysql:root /var/run/mysqld

**Okay lets fire it up and do an upgrade**

sudo /etc/init.d/mysql start<br>
mysql_upgrade<br>
sudo /etc/init.d/mysql restart<br>

#NOTES
I only installed this as a replacement, and haven't tried as a fresh install. If you needs some more help please see the help pages on https://mariadb.com. There's also more details on tailoring your setup to take better advantage of the new MariaDB features.

https://mariadb.com/kb/en/mariadb/upgrading-from-mysql-to-mariadb/<br>
https://mariadb.com/kb/en/mariadb/mysql_upgrade/<br>
https://github.com/allfs/mariadb/tree/master/debian/additions<br>



