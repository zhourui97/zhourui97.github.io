---
layout: page
title:	c debug
category: blog
description: 
---
# Preface

- First, let excute file records Source file a.c via gcc parameter "-g"

	gcc -g a.c

- Second, start gdb 
	
	gdb a.out

- Set breakpoint

	gdb> b 9

- Run program

	gdb> run

# gdb & lldb
> On mac , gdb has been replaced by lldb. [lldb vs gdb](http://lldb.llvm.org/lldb-gdb.html)

## EXECUTION COMMANDS

### launch

#### launch with args

	% gdb --args a.out 1 2 3 
	% lldb -- a.out 1 2 3

or

	gdb> set args 1 2 3
	lldb> settings set target.run-args 1 2 3

* set env

	gdb> set env DEBUG 1
	(lldb) env DEBUG=1
	(lldb) set se target.env-vars DEBUG=1
	(lldb) settings set target.env-vars DEBUG=1

* unset env

	gdb> unset env DEBUG
	(lldb) settings remove target.env-vars DEBUG
	(lldb) set rem target.env-vars DEBUG

* show args

	gdb> show args
	lldb> settings show target.run-args

* set env and run
	
	lldb>  process launch -v DEBUG=1

## process

### pid  Attach to a process with pid 123
	
	gdb> attach 123
	(lldb) process attach --pid 123
	(lldb) attach -p 123

### pname Attach to a process named "a.out"
	gdb> attach a.out
	(lldb) process attach --name a.out
	(lldb) pro at -n a.out

### Attach to a remote gdb protocol server running on system "eorgadd", port 8000.
	(gdb) target remote eorgadd:8000	
	(lldb) gdb-remote eorgadd:8000

### Attach to a remote gdb protocol server running on the local system, port 8000.
	(gdb) target remote localhost:8000	
	(lldb) gdb-remote 8000

### Attach to a Darwin kernel in kdp mode on system "eorgadd".
	(gdb) kdp-reattach eorgadd	(lldb) kdp-remote eorgadd

## info & image

### info symbol
Look up information for a raw address in the executable or any shared libraries.

	gdb >  info symbol 0x1ec4
	gdb >  info symbol func
	lldb > image lookup --address 0x1ec4
	lldb > im loo -a 0x1ec4

## list source code

	# list source code from line 1
	gdb> list 1
	# list next 10 line of Source code
	gdb> list <or> <enter>
	gdb> list main

> `list` is usually  abbrevited to `l`

## step

	# step in( source level)
	> s <or> step
	# step over( source level)
	> n <or> <enter>
	# step in ( instruction level )
	> si
	# step over ( instruction level )
	> ni
	# Step out of the currently selected frame.
	> finish or fin

### Watchpoint

	# watch var when it is written to
	gdb> watch var
	lldb> watch set variable var
	lldb> wa s v var

	# delete
	watch del 1
	wa l

### breakpoint(stop)

	#break on line 9
	b 9
	# break on func main
	b main
	# on file
	b a.c:12
	# continue excute instead of step excute
	c
	# list
	gdb> i break
	lldb> breakpoint list
	lldb> br l
	lldb> b
	# delete
	gdb> delete 1
	lldb> br del 1

#### condition

	gdb> break foo if sum != 0
	(lldb) breakpoint set --name foo --condition '(int)sum != 0'
	(lldb) br s -n foo -c '(int) sum != 0'
	(lldb) b foo -c '(int) sum != 0'

### stop-hook

#### display var when stop
	gdb lldb > display var
	lldb >
		target stop-hook add -o "fr v var"
		ta st a -o "fr v var"
	> undisplay 

#### display var when func main stop

	lldb > ta st a -n main -o "fr v var"

### backtrace
View call stack:

	(gdb lldb) bt
	#0 add_range (low=1, high=10) at main.c:6
	#1 0x080483c1 in main () at main.c:14

There are 2 frames: 0, 1.

## Variable
gdb 参数：

	x/7db
	x/7w
	f, the display format
		`print`
		`s' (null-terminated string), or
		`i' (machine instruction). 
		`x' (hexadecimal) initially. 
		`d` decimal
	u, the unit size
		7 size 7
		b Bytes.
		h Halfwords (two bytes).
		w Words (four bytes). This is the initial default.
		g Giant words (eight bytes).

其中, lldb 参数 :

	-f format
		x hex(default)
		d decimal
		b binary

### show register

	gdb> p $rsp

#### 寄存器间接寻址

	# via register($) 20bytes
	gdb lldb> x/20 $rsp
	gdb lldb> x/20 $rsp+4

### show variable

	gdb> p var	"show var
	gdb> x/1b &var
	gdb> p &var	"show var's address

show var var pointer:

	gdb> p *pointer
	gdb> x/1b pointer

### show via address (x,memory read)
相当于show pointer

#### read memory

	### Read memory from address 0xbffff3c0 and show 4 hex uint32_t values. 推荐用gdb 语法
	(gdb) x/4xw 0xbffff3c0
	(lldb) x/4xw 0xbffff3c0
	(lldb) memory read --size 4 --format x --count 4 0xbffff3c0
	(lldb) me r -s4 -fx -c4 0xbffff3c0
	(lldb) x -s4 -fx -c4 0xbffff3c0

	### 对变量取地址input
	lldb> x/7xb &var
	lldb> x/7 &var
	lldb> x -fx -c7 &var


#### save results to a local file as text.

	(gdb) set logging on
	(gdb) set logging file /tmp/mem.txt
	(gdb) x/512bx 0xbffff3c0
	(gdb) set logging off
	(lldb) x/512bx -o/tmp/mem.txt 0xbffff3c0
	(lldb) memory read --outfile /tmp/mem.txt --count 512 0xbffff3c0
	(lldb) me r -o/tmp/mem.txt -c512 0xbffff3c0

### view local Variable(info)
info,i
frame,fr/f
print,p

> Mac OSX has no info, but frame has integrated info

View *all locals* in current frames

	gdb > i locals + i args 
	lldb > fr v

	gdb > i locals 
	lldb > fr v -a (frame variable --no-args)

View specified local variable:

	gdb lldb> p variable
	lldb> fr v variable

	gdb lldb> x/1x &variable

	# show contents of local variable "bar" formatted as hex.
	gdb lldb> p/x bar
	lldb> fr v -fx bar

#### view variable's address

	gdb lldb> p &var
	gdb lldb> p *&var
	gdb lldb> p 0x10

#### print result

	gdb lldb> p 11+3&~3
	gdb lldb> p sizeof(char *)

### Show the global/static variables

	gdb > N/A
	(lldb) target variable
	lldb > ta v

	# show contents of global variable "baz"
	gdb lldb> p paz
	(lldb) ta v baz 


### reassign Variable with new value(p)

	gdb> set var sum=0
	gdb> set var arr[1]=0
	gdb lldb> p arr[1]=0
	gdb lldb> p printf("The arr's first value is:%d", arr[1])
		The arr's first value is:0
		$6 = 26 //The printf will return length of string.


## Register
gdb 中register  前有`$`

### view register


	# via p($)
	gdb> p $rax (10进制)
	lldb> p $rax

	# via info/register read (hex)
	gdb > info registers 显示寄存器的值
	lldb > register read
	lldb > reg r 
	lldb > register read rax

### write
	gdb > p $rax = 123
	lldb >  register write rax 123
	gdb >  jump *$pc+8
	lldb > register write pc `$pc+8`

## disassemble

	gdb> disassemble
	lldb> disassemble
	lldb>  disassemble --frame
	lldb>  di -f

# assert
断言, 如果定义了NODEBUG (或者直接在编译时加上选项-DNDEBUG), 则assert函数则不被编译

	#define NDEBUG
	assert(111)

# objdump & otool
objdump 是linux 下的工具，用于将源代码、文件名、与汇编码对应显示
otool/gobjdump 是mac 下类似的工具， 与objdump -SI 相近的命令是otool -tV

	gcc -g a.c
	" -dS 与 -SI 好像是一样的
	objdump -dS a.out
	gobjdump -dS a.out
	otool -tV a.out

objdump:

	-d, --disassemble        Display assembler contents of executable sections
	-D, --disassemble-all    Display assembler contents of all sections
	-S, --source             Intermix source code with disassembly

otool

	- otool -tV <executable>
	-t   Display the contents of the (__TEXT,__text) section.  
		 With the -v flag, this disassembles the text.  
		 And with -V, it also symbolically disassembles the operands.

	-d     Display the contents of the (__DATA,__data) section.

> 除了用`objdump`, 我们还可以用gdb 的`disassemble` 反汇编，它后面跟函数名，则反汇编函数. 默认是反汇编当前的函数. 我们用以了：

# strace
> 最新的strace 有一个-k 参数, 显示调用栈. Mac 下没有strace 但是有很好用的: sample

strace常用来跟踪进程执行时的系统调用和所接收的信号。 在Linux世界，进程不能直接访问硬件设备，当进程需要访问硬件设备(比如读取磁盘文件，接收网络数据等等)时，必须由用户态模式切换至内核态模式，通 过系统调用访问硬件设备。strace可以跟踪到一个进程产生的系统调用,包括参数，返回值，执行消耗的时间。

	-c 统计每一系统调用的所执行的时间,次数和出错的次数等. 
	-d 输出strace关于标准错误的调试信息. 
	-f 跟踪由fork调用所产生的子进程. 
	-ff 如果提供-o filename,则所有进程的跟踪结果输出到相应的filename.pid中,pid是各进程的进程号. 
	-F 尝试跟踪vfork调用.在-f时,vfork不被跟踪. 
	-h 输出简要的帮助信息. 
	-i 输出系统调用的入口指针. 
	-q 禁止输出关于脱离的消息. 
	-r 打印出相对时间关于,,每一个系统调用. 
	-t 在输出中的每一行前加上时间信息. 
	-tt 在输出中的每一行前加上时间信息,微秒级. 
	-ttt 微秒级输出,以秒了表示时间. 
	-T 显示每一调用所耗的时间. 
	-v 输出所有的系统调用.一些调用关于环境变量,状态,输入输出等调用由于使用频繁,默认不输出. 
	-V 输出strace的版本信息. 
	-x 以十六进制形式输出非标准字符串 
	-xx 所有字符串以十六进制形式输出. 
	-a column 
	设置返回值的输出位置.默认 为40. 
	-e expr 
		指定一个表达式,用来控制如何跟踪.格式如下: 
		[qualifier=][!]value1[,value2]... 
		qualifier只能是 trace,abbrev,verbose,raw,signal,read,write其中之一.value是用来限定的符号或数字.默认的 qualifier是 trace.感叹号是否定符号.例如: 
	-eopen等价于 -e trace=open,表示只跟踪open调用.而-etrace!=open表示跟踪除了open以外的其他调用.有两个特殊的符号 all 和 none. 
	注意有些shell使用!来执行历史记录里的命令,所以要使用\\. 
	-e trace=set 
	只跟踪指定的系统 调用.例如:-e trace=open,close,rean,write表示只跟踪这四个系统调用.默认的为set=all. 
	-e trace=file 
	只跟踪有关文件操作的系统调用. 
	-e trace=process 
	只跟踪有关进程控制的系统调用. 
	-e trace=network 
	跟踪与网络有关的所有系统调用. 
	-e strace=signal 
	跟踪所有与系统信号有关的 系统调用 
	-e trace=ipc 
	跟踪所有与进程通讯有关的系统调用 
	-e abbrev=set 
	设定 strace输出的系统调用的结果集.-v 等与 abbrev=none.默认为abbrev=all. 
	-e raw=set 
	将指 定的系统调用的参数以十六进制显示. 
	-e signal=set 
	指定跟踪的系统信号.默认为all.如 signal=!SIGIO(或者signal=!io),表示不跟踪SIGIO信号. 
	-e read=set 
	输出从指定文件中读出 的数据.例如: 
	-e read=3,5 
	-e write=set 
	输出写入到指定文件中的数据. 
	-o filename 
	将strace的输出写入文件filename 
	-p pid 
	跟踪指定的进程pid. 
	-s strsize 
	指定输出的字符串的最大长度.默认为32.文件名一直全部输出. 
	-u username 
	以username 的UID和GID执行被跟踪的命令

Example：

	# 跟踪pid
	strace  -p 28979
	# 跟踪program
	strace whoami
	# 只跟踪网络相关的系统调用
	strace -e trace=network -p 213
	# 跟踪fork(-f) 或者 vfork(-F) 出的子进程
	strace -f -F httpd -k start

> 请参考: [strace 使用](http://www.cnblogs.com/ggjucheng/archive/2012/01/08/2316692.html)

## sample and dtruss

### dtruss

	ps aux| grep php ;#find php id
	sudo dtruss -p <pid>

# heap
Refer [c-debug-heap](/p/linux-c-debug-heap)
