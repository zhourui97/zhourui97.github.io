---
layout: page
title:	linux process tool
category: blog
description: 
---
# Preface


# pkill

	pkill <pattern>
		pattern
			ERE expression

Example:

	pkill '^rsync$'

See: man pgrep, man pkill for more.

# ps
This version of ps accepts several kinds of options:

1.   UNIX options, which may be grouped and must be preceded by a dash.
2.   BSD options, which may be grouped and must not be used with a dash.
3.   GNU long options, which are preceded by two dashes.

Options:

	x	To list all processes owned `by you` (same EUID as ps), or to list all processes when used together with the `a` option.
	a	To list all processes with a terminal (tty), or to list all processes when used together with the x option.
	u	display user-oriented format
	f	does full-format listing.

# htop

# strace

# strace
跟踪进程的系统调用
linux: strace
mac: dtruss

	dtruss ./a.out

example:

	sudo yum install strace -y
	sudo strace -p 23879
    	-f
		   Trace child processes as they are created by currently traced processes as a result of the fork(2) system cal
		-s 1024
			1024 strsize

trace php-fpm

	pgrep php-fpm -d ' -p '
		-d delim
			Specify a delimiter to be printed between each process ID.
	strace `pgrep -d ' -p ' php-fpm ` -s 1024 -f
