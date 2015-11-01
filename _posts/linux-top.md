---
layout: page
title:	linux top命令简明教程
category: blog
description: 
---
# Preface
linux 下有很多用于查看高负载的命令，比如top 用来查看内存/cpu 消耗的，iotop 用来查看io 消耗的。

# top
top 是一个动态查看进程信息的工具

	top - 04:18:11 up 29 days,  2:38,  1 user,  load average: 0.00, 0.02, 0.07
	Tasks:  71 total,   1 running,  70 sleeping,   0 stopped,   0 zombie
	Cpu(s):  0.0%us,  0.3%sy,  0.7%ni, 99.0%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
	Mem:    502440k total,   159412k used,   343028k free,     7824k buffers
	Swap:  1048568k total,      248k used,  1048320k free,    76732k cached

	  PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
	27529 root       1 -19  402m 9024 1596 S  0.0  1.8   0:00.07 php-fpm
	27530 www        1 -19  402m 8356  912 S  0.0  1.7   0:00.00 php-fpm
	27531 www        1 -19  402m 8364  920 S  0.0  1.7   0:00.00 php-fpm
	27532 www        1 -19  402m 8356  912 S  0.0  1.7   0:00.00 php-fpm
	28332 xuehui1   25   5  400m  30m  23m S  0.0  6.3   0:00.52 php
	27538 www       20   0  400m  30m  23m S  0.0  6.3   0:00.02 php
	29134 xuehui1   25   5  168m 3176 2408 S  0.0  0.6   0:00.00 curl
	29011 xuehui1   20   0  123m 2976 1816 S  0.0  0.6   0:00.13 zsh
	 1082 root      20   0  105m  624  280 S  0.0  0.1   0:00.00 bash
	12760 root      20   0  105m  384  224 S  0.0  0.1   0:00.00 rsync
	29007 root      20   0 98156 4012 3060 S  0.0  0.8   0:00.01 sshd
	29010 xuehui1   20   0 98156 1760  788 S  0.0  0.4   0:00.00 sshd
	 1066 root      20   0 64116  976  304 S  0.0  0.2   0:01.53 sshd
	27539 www       20   0 57120 5752  532 S  0.0  1.1   0:00.00 nginx
	27535 root      20   0 53024 1552  348 S  0.0  0.3   0:00.00 nginx
	 4975 root      20   0 52444  688  144 S  0.0  0.1   0:00.27 vsftpd
		1 root      20   0 19352  632  412 S  0.0  0.1   0:01.06 init
	29047 xuehui1   25   5 15008 1236  968 R  0.0  0.2   0:00.04 top
	  473 root      16  -4 11044  856  176 S  0.0  0.2   0:00.04 udevd
	 1706 root      18  -2 10644  540  184 S  0.0  0.1   0:00.00 udevd
	 2487 root      18  -2 10644  540  184 S  0.0  0.1   0:00.00 udevd
	 1083 root      20   0  4060  312  232 S  0.0  0.1   0:00.01 mingetty
	 1085 root      20   0  4060  316  232 S  0.0  0.1   0:00.00 mingetty
	 1087 root      20   0  4060  316  232 S  0.0  0.1   0:00.00 mingetty
	 1090 root      20   0  4060  316  232 S  0.0  0.1   0:00.00 mingetty
	 1093 root      20   0  4060  312  232 S  0.0  0.1   0:00.00 mingetty
	 1095 root      20   0  4060  312  232 S  0.0  0.1   0:00.00 mingetty

> mac 下top -n3 查看使用率最高的的前3

## 开机信息
	top - 04:18:11 up 29 days,  2:38,  1 user,  load average: 0.00, 0.02, 0.07

第一行是开机信息： 开机了29天2小时38分, 一个用户， 近期的OS 负载：1分钟，5分钟，15分钟

## 进程信息
	Tasks:  71 total,   1 running,  70 sleeping,   0 stopped,   0 zombie

## cpu 信息
	Cpu(s):  0.0%us,  0.3%sy,  0.7%ni, 99.0%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st

	0.0%us user mode 用户空间占用CPU的百分比
	0.3%sy system mode 内核空间占用CPU的百分比
	0.7%ni 改变过优先级的进程占用CPU的百分比
	99.0%id 空闲CPU的百分比
	0.0%wa IO Waiting 占用CPU的百分比
	0.0%hi 硬中断(Hardware IRQs)占用CPU的百分比
	0.0%si 软中断(Software Interrupt Requests)占用CPU的百分比
	0.0%st steal (time given to other DomU instances)

## 内存信息
	Mem:    502440k total,   159412k used,   343028k free,     7824k buffers

	total 总
	used 使用
	free 未用
	buffers 缓冲交换区占用

## swap 信息
	Swap:  1048568k total,      248k used,  1048320k free,    76732k cached

> 一般可用内存包括：free + buffers + cached

## 进程信息

	  PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
	27529 root       1 -19  402m 9024 1596 S  0.0  1.8   0:00.07 php-fpm
	27538 www       20   0  400m  30m  23m S  0.0  6.3   0:00.02 php

PID — 进程id
USER — 进程所有者
PR — 进程优先级
NI — nice值。负值表示高优先级，正值表示低优先级
VIRT — 进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES
RES — 进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA
SHR — 共享内存大小，单位kb
S — 进程状态。D=不可中断的睡眠状态 R=运行 S=睡眠 T=跟踪/停止 Z=僵尸进程
%CPU — 上次更新到现在的CPU时间占用百分比
%MEM — 进程使用的物理内存百分比
TIME+ — 进程使用的CPU时间总计，单位1/100秒
COMMAND — 进程名称（命令名/命令行）


### Highlight高亮
	h 帮助

### sort
	x 排序序列高亮
	F/O 选择排序序列
	shift + </> 左右改变排序序列

### filed
	f/o add/remove field

### COMMAND
	c 切换显示命令名称和完整命令行
	top -c 启动时

### nice
	r 重新安排进程优先级
	

# 查看进程
	ps -ef | grep pid

rss       RSS    resident set size, the non-swapped physical memory that a task has used (in kiloBytes). (alias rssize, rsz).
vsz       VSZ    virtual memory size of the process in KiB (1024-byte units). Device mappings are currently excluded; this is subject to change. (alias vsize).
rss 是进程占用的物理内存，但是所有进程加起来的rss 会超过物理内存（因为进程间有很多内存是共享的）
可以用pmap 查看进程所用的非共享物理内存(writeable/private)

	➜  ~  pmap -d 15102
	15102:   redis-server
	Address           Kbytes Mode  Offset           Device    Mapping
	0000000000400000     424 r-x-- 0000000000000000 0fc:00001 redis-server
	00007fd1fc77f000    1576 r-x-- 0000000000000000 0fc:00001 libc-2.12.so
	00007fd1fd3d6000       4 r---- 000000000001f000 0fc:00001 ld-2.12.so
	mapped: 40048K    writeable/private: 29064K    shared: 0K

## Find parent process 
	$ ps aux -o ppid | grep 3027 | grep -v grep
	_www      3027   0.0  0.0  2438040    616   ??  S     8:01PM   0:00.00 /usr/sbin/httpd   3001

	$ ps -l 3027 | grep -v grep
	  UID   PID  PPID        F CPU PRI NI       SZ    RSS WCHAN     S     ADDR TTY           TIME CMD
	   70  3027  3001      104   0  31  0  2438040    616 -      S     eada7e0 ??         0:00.00 /usr/sbin/httpd -D FOREGROUND

	$ ps -l $$

or you can use pstree.  As you can see, 3001 is the parent process id of 3027.

	$ pstree -p 3027
	-+= 00001 root /sbin/launchd
	\-+= 03001 root /usr/sbin/httpd -D FOREGROUND
	\--- 03027 _www /usr/sbin/httpd -D FOREGROUND

# 查看io

	iotop -o

# Reference
- [top]
- [swap]

[top]: http://www.cnblogs.com/peida/archive/2012/12/24/2831353.html
[swap]: http://blog.csdn.net/xiangliangyu/article/details/8213127
[mysql swap]: http://blog.sina.com.cn/s/blog_4e46604d01016sp0.html
