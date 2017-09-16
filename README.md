# daloradius-sshmysql
Code modification to allow insert of SSH users. 
Requirements:
- SSH configured with nss-mysql

# HOW TO (For CENTOS):

yum install libnss-mysql pam_mysql mysql-server

create database user_db
 grant all privileges on user_db.* to 'sshdb'@'%' identified by '
 install DB
 CREATE TABLE `groups` (
`name` varchar(16) NOT NULL DEFAULT '',
`password` varchar(34) NOT NULL DEFAULT 'x',
`gid` int(11) NOT NULL AUTO_INCREMENT,
PRIMARY KEY (`gid`)
) ENGINE=InnoDB AUTO_INCREMENT=5001 DEFAULT CHARSET=latin1;

INSERT INTO `groups` VALUES ('ssh','x',5000);


CREATE TABLE `grouplist` (
`rowid` int(11) NOT NULL AUTO_INCREMENT,
`gid` int(11) NOT NULL DEFAULT '0',
`username` char(16) NOT NULL DEFAULT '',
PRIMARY KEY (`rowid`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=latin1;

INSERT INTO `grouplist` VALUES (1,5000,'tafora'); //boleh tidak diikutkan

CREATE TABLE `users` (
`username` varchar(16) NOT NULL DEFAULT '',
`uid` int(11) NOT NULL AUTO_INCREMENT,
`gid` int(11) NOT NULL DEFAULT '5000',
`gecos` varchar(128) NOT NULL DEFAULT 'SSH',
`homedir` varchar(255) NOT NULL DEFAULT '/home',
`shell` varchar(20) NOT NULL DEFAULT '/bin/bash',
`password` varchar(34) NOT NULL DEFAULT 'x',
`lstchg` bigint(20) NOT NULL DEFAULT '1',
`min` bigint(20) NOT NULL DEFAULT '0',
`max` bigint(20) NOT NULL DEFAULT '99999',
`warn` bigint(20) NOT NULL DEFAULT '0',
`inact` bigint(20) NOT NULL DEFAULT '0',
`expire` bigint(20) NOT NULL DEFAULT '-1',
`flag` bigint(20) unsigned NOT NULL DEFAULT '0',
PRIMARY KEY (`uid`),
UNIQUE KEY `username` (`username`),
KEY `uid` (`uid`)
) ENGINE=InnoDB AUTO_INCREMENT=5010 DEFAULT CHARSET=latin1;

INSERT INTO `users` VALUES ('johndoe',5000,5000,'John Doe','/home','/bin/false','/kyPT61xSTY0w',1,0,99999,0,0,-1,0);

Edit /etc/libnss-mysql.cfg
host        localhost
database    user_db
username    sshdb
password    DLJNDXHw
#socket      /var/lib/mysql/mysql.sock
#port        3306

Edit /etc/libnss-mysql-root.cfg
username    sshdb
password    DLJNDXHw

Edit /etc/pam.d/sshd
auth      sufficient   pam_unix.so
auth      required     pam_mysql.so    config_file=/etc/pam_mysql.conf
account   sufficient   pam_unix.so
account   required     pam_mysql.so    config_file=/etc/pam_mysql.conf

Setting up CHROOT
nano chroot.sh (paste chroot.sh file)

install
./chroot.sh /bin/{ping,ls,bash}

copy some libs
cp /lib64/libnss_dns* /var/chroot/lib64/
cp /lib64/libresolv* /var/chroot/lib64/

enable ping
chmod u+srwx,go=rx ping


assign group /etc/ssh/sshd_config
Match Group ssh
	ChrootDirectory /var/chroot

Daloradius setup
wget webtatic repo
yum install php56w php56w-mysql php56w-gd php56w-mbstring php56w-pear
pear install db
put the daloradius custom script from our files


Dropbear
yum install dropbear

/etc/init.d/dropbear
OPTIONS="-p 443 -p 80 -p 8080 -b /etc/dropbear/banner"

/etc/shells:
/bin/false

Set users shell to /bin/false for security reason
