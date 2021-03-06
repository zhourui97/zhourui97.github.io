---
layout:	post
title:	
category: blog
description: 
---
# Preface

# init

## init
init指linux 系统运行级runlevel 0~6. 可在/etc/inittab中指定:

	id:3:initdefault:


### rc.d
运行级对应的要启动的脚本目录rcN.d(其下的脚本链接到../init.d/):

	/etc/rc.d/rcN.d

rcN.d下的文件是按启动顺序的, 18比10大, 于是在10后启动; S代表start K 代表Kill

	S10network
	K15httpd
	S18sshd
	...

runlevel含义是基于rcN.d的, 不同的OS对于rcN.d的定义是不同的, 对于Red Hat系来说[runlevel](http://en.wikipedia.org/wiki/Runlevel):

	0	Halt(Shutdown)
	1	Single-user Mode
	2	Multi-user Mode	
	3	Multi-user Mode with Networking
	4	Not used/User-definable	For special purposes.
	5	Start the system normally with appropriate display manager. ( with GUI )	Same as runlevel 3 + display manager.
	6	Reboot

### /etc/rc.d/{rc.local rc.sysinit}
在所有rcN.d初始化结束后会依次执行: rc.sysinit 、 rc.local、rc

> Mac OSX 应该用的是`/etc/rc.common`

## init.d
如果你执行以下命令, 你会发现: 
	
	ll -id /etc/init.d /etc/rc.d/init.d #它们是硬链接

init.d里面放置的是运行级脚本, 脚本文件写法：vi /etc/init.d/BLAH

	#!/bin/sh
	# SAMPLE BASIC INIT SCRIPT
	#
	# Below is the chkconfig syntax for auto startup at different run levels
	# Note runlevel 2 3 and 5 are level, 69 is the Start order and 68 is the Stop order
	# Make sure these are unique by looking into /etc/rc.d/*
	# Also below is the description which is necessary.
	#
	# chkconfig: 235 69 68
	# description: Description of the Service
	#
	# Below is the source function library
	. /etc/init.d/functions

	if [ -f /etc/sysconfig/BLAH ]; then
	. /etc/sysconfig/BLAH
	fi

	# Below is the Script Goodness controlling the service
	case "$1" in
		start)
			echo -n "Start service BLAH"
			/usr/sbin/BLAH start
			;;
		stop)
			echo -n "Stop service BLAH"
			/usr/sbin/BLAH stop
			;;
		restart)
			echo -n "Restart service BLAH"
			/usr/sbin/BLAH restart
			;;
		*)
			echo "Usage: $0 {start|stop|restart}"
			exit 1 ;;
	esac


## service
通过service 可以手动启动init.d下的服务脚本
在linux 中执行:

	service Name argv1 argv2 ...
	service sshd start

实际执行的是:
	
	/etc/init.d/Name argv1 argv2 ...
	/etc/init.d/sshd start

## chkconfig
chkconfig serviceName 与 service Name 不同，前者用于开机运行级(和init级相关，涉及/etc/rc.d/rcN.d). 而后者service Name 用于手动开启关闭某些服务(涉及/etc/init.d/). 不过它们的启动脚本都指向/etc/init.d/xxx (比如:

	# chkconfig --level 016 BLAH off (to turn off at run level 016)
	/etc/rc.d/rc[016].d/K68BLAH -> ../init.d/BLAH
	# chkconfig --level 235 BLAH on (to turn on at run level 235)
	/etc/rc.d/rc[235].d/S69BLAH -> ../init.d/BLAH

### 命令

	chkconfig --add name：增加一项新的服务。chkconfig确保每个运行级有一项启动(S)或者杀死(K)入口。如有缺少，则会从缺省的`init.d/`脚本自动建立, 比如(/etc/rc.d/rc3.d/S69BLAH -> ../init.d/BLAH)。
    chkconfig --del name：删除服务，并把相关符号连接从/etc/rc[0-6].d删除。
    chkconfig [--level levels] name <on|off|reset>：设置某一服务在指定的运行级是被启动，停止还是重置。on|off|reset 对应于运行级脚本中的start|stop|restart 方法

以ssh 为例子:

	#rpm -qa |grep ssh 检查是否装了SSH包
	# 没有的话yum install openssh-server
	chkconfig --list sshd 检查SSHD是否在本运行级别下设置为开机启动
	chkconfig --level 2345 sshd on  如果没设置启动就设置下.
	service sshd restart  重新启动
	netstat -antp |grep sshd  看是否启动了22端口.确认下.
	iptables -nL  看看是否放行了22口.
	setup---->防火墙设置   如果没放行就设置放行.

# systemd
linux 下的init(service/chkconfig) 这种系统服务管理器比较臃肿了，而systemd是一种更加优秀的服务管理器。 

## service list
systemctl list-unit-files

## systemctl
systemctl stop/status/reload/restart/is-enabled/help <service>

## 运行级
systemd 取消了init 数字运行级.而改用multi-user.target, graphical.target 文件命名的方式

	ln -s [/usr]/lib/systemd/system/multi-user.target /etc/systemd/default.target
	# 临时切换到 graphical
	systemctl isolate graphical.target

# launchd
launchd 是mac 下的系统服务管理器, 

	# 类似于chkconfig add mysqld; chkconfig --level 2345 mysqld on
	# 或者 systemctl enable mysqld
    ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents

	# 类似于service mysqld start  或者 systemctl start mysqld
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
	

	# 类似于service mysqld stop | systemctl stop mysqld
	launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist


	# 类似于chkconfig --level 2345 mysql off | systemctl disable mysqld
	rm -Rf ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist

launchctl管理OS X的启动脚本，控制启动计算机时需要开启的服务。也可以设置定时执行特定任务的脚本，就像Linux cron一样。

例如，开机时自动启动Apache服务器：

	$ sudo launchctl load -w /System/Library/LaunchDaemons/org.apache.httpd.plist

运行launchctl list显示当前的启动脚本。sudo launchctl unload [path/to/script]停止正在运行的启动脚本，再加上-w选项即可去除开机启动。用这个方法可以一次去除Adobe或Microsoft Office所附带的所有“自动更新”后台程序。

Launchd脚本存储在以下位置：

	~/Library/LaunchAgents    
	/Library/LaunchAgents          
	/Library/LaunchDaemons
	/System/Library/LaunchAgents
	/System/Library/LaunchDaemons

启动脚本的格式可以参考[这篇blog](http://paul.annesley.cc/2012/09/mac-os-x-launchd-is-cool/)，
或[苹果开发者中心的文章](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLaunchdJobs.html)。你也可以使用[Lingon应用](http://www.peterborgapps.com/lingon/)来完全取代命令行。

# profile(login shell)
/etc/profile 是启动初始化脚本(login shell 才会读), 它会执行以下脚本

	/etc/inputrc 输入键相关
	/etc/profile.d/ 子目录
	/etc/sysconfig/i18n 由/etc/profile.d/lang.sh呼叫

## ~/.bash_profile ~/.profile
这几个文件是次启动脚本(同样是login shell 才会读)，对于bash 来说，它会顺序遍历以下脚本并执行第一个：

	~/.bash_profile 
	~/.bash_login
	~/.profile

# .bashrc .zshrc
这两个文件是non-login 就会读：

	/etc/zshrc ~/.zshrc
	/etc/bashrc ~/.bashrc

默认的.bashrc 会调用/etc/bashrc:

	# ~/.bashrc
	if [-f /etc/bashrc ];then
		. /etc/bashrc
	fi

	# /etc/bashrc (/etc/zshrc 也如此)
	   . /etc/profile.d/*.sh;



.zshrc 与 .bashrc 在创建用户时，拷贝自/etc/skel

	 /etc/skel/.bashrc
	 /etc/skel/.zshrc


## *ctl
许多*ctl 实际是对一些服务的单独封装. 比如: apachectl 就是对httpd的独立封装
	
	vi `which apachectl`

在Mysql中mysql.service 也是对mysql_safe启动的单独封装

# crontab
Unix/Linux 下以用户身份编辑`crontab -e`, 在`/etc/crontab` 中编辑的话则需要在命令前指定用户身份

	> crontab -e
	# run at 10 pm on weekdays, annoy Joe
	5 4 * * sun  echo "run at 5 after 4 every sunday"
	# shutdown -h now at 11pm
	00 23 * * * /sbin/shutdown -h now

	> crontab -l

For log via `tail -f /var/log/cron.log`, for more see `man 5 crontab`

If you use mac OSX, you should disable backup when you edit crontab with vim [](http://calebthompson.io/crontab-and-vim-sitting-in-a-tree/)

	autocmd filetype crontab setlocal nobackup nowritebackup

Task: Start cron service

To start the cron service, use:

	/etc/init.d/crond start
	service crond start

	;OR RHEL/Centos Linux 7.x user:
	systemctl start crond.service

Task: Stop cron service

	/etc/init.d/crond stop
	service crond stop

	;OR RHEL/Centos Linux 7.x user:
	systemctl stop crond.service

Task: Restart cron service

	/etc/init.d/crond restart
	service crond restart

	# systemctl restart crond.service
