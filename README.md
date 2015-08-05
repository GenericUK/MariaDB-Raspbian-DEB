# MariaDB Raspbian DEB
Raspberry Pi Raspbian compiled DEB Debian Package of MariaDB MySQL replacement.

Compiled with checkinstall on Pi2 | Raspbian GNU/Linux 7 (wheezy) | Kernel 3.18.11-v7+

Compiling MariaDB was a bit of a trick on Raspian as it requires gcc & g++ packages to be upgraded to 4.8 which isn't available in wheezy/Raspian 7, it also takes several hours and requires about 2Gb on your wee Pi. Shame to waste my work so its here if you need it.

**PLEASE MAKE SURE YOU BACKUP YOU SYSTEM ESPECIALLY ANY EXISTING MYSQL DATABASES, THIS INSTALLED ON MY SYSTEM FINE BUT THAT DOESN'T MEAN IT WILL ON YOURS.**

# PRE INSTALL INSTRUCTIONS

1. mysqldump -u root -p --all-databases > mysqlbackup.sql
2. If your on a live production server you'll probably want to stop your web server now
3. sudo service mysql stop **[OR]** sudo /etc/init.d/mysql stop

**Uninstall MySQL (if installed)**<br>
Because MariaDB is a drop in replacement for MySQL most of the libraries should work, however apt-get did want to uninstall most of it so I let it and also ran auto-remove just to finish the job. 

Don't purge if you want to keep your existing MySQL config's and databases. I kept all mine.

1. sudo apt-get remove mysql-server mysql-client-5.5 mysql-common mysql-server-5.5 mysql-server-core-5.5
2. sudo apt-get autoremove **[optional]**

# INSTALL

1. sudo dpkg -i /DOWLOAD_LOCATION/mariadb_10.0.20-1_armhf.deb

# POST INSTALL

MariaDB default location is /usr/local/mysql and the executables are in /usr/local/mysql/bin. On Raspian/Debian they would have been in /usr/bin, so we need to update our MariaDB/MySQL config files if your replacing MySQL.

1. 


I'm writing them now...

