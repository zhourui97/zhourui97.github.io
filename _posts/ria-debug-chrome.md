---
layout: page
title:	
category: blog
description: 
---
# Preface
chrome://chrome-urls/

# todo
chrome 调试指南1,2,3 
http://web.jobbole.com/82558/

http://web.jobbole.com/84148/

# Network
chrome://chrome-urls
- Dns Cache
chrome://net-internals/#dns

- spdy
chrome://net-internals/#spdy

# shortcuts
建议使用vimium

# Element
- Dom Breakpoint

# inspect element
Select element and inspect it

	$0 current select 
	$1 last select 
	$2 the last 2'th select 
	$3 the last 3'th select 

# console

## console log

	console.log('%s-%d','abc', 123)

	//console color
	console.info()
	console.warn()
	console.debug()
	console.error()

console group 用于对log 分组:

	console.group('group1')
	console.groupEnd();

## console 性能
第一种是通过 console.time 函数计时

	console.time('a'); //用一个key标记计时开始
	...do sth....
	console.timeEnd('a'); //得到这个key 标识开始的代码的耗时

第二种是console.profile() 生成profile, 然后在profile tab 中观察代码耗时的细节

	console.profile()
	...do sth....
	console.profileEnd(); //生成profile1 用于

## trace
console.trace(); 会记录函数调用栈的信息，并打印到控制台（我还是不会用..）

## console.dir
console.dir(object) 用于查看对象的所有属性

# JS

## Breakpoint
- Cmd+P 打开source file

### JS Breakpoint
> 断点时，console 中的this 指向backbone 而不是window

1. Press `Pretty print`, if the code is not human readable.
2. Add Breakpoint or Watch Expression
3. Refresh

断点shortcuts:

	F10 step over
	F11 step in
	Shift+F11 step out
	F8	resume/stop script execution

### watch 
press `+`, 输入变量名, 比如`i` 就可以观察变量。不区分scope, 在当前scope 如果没有`i` 就显示`undefined`

### XHR Breakpoint
击+ 并输入 URL 包含的字符串即可监听该 URL 的 Ajax 请求，输入内容就相当于 URL 的过滤器。如果什么都不填，那么就监听所有 XHR 请求。一旦 XHR 调用触发时就会在 request.send() 的地方中断。

## snippets
关闭浏览器后不会消失. 浏览器刷新时也不会执行，除非手动执行

## edit js
1. edit js in sources
2. cmd+s to saved
3. Refresh to clear edit

即使在断点时，你也可以编辑代码，按ctrl+S保存之后，你会看到区域2的背景由白色变为浅色，而断点会重新开始执行。

## remote debug
打开：

	chrome://inspect

连接手机:
https://developer.chrome.com/devtools/docs/remote-debugging

# console
如果是自己的项目可以在发布的时候生成三份东西

1. 源代码,比如 app.js 
2. 压缩后的代码，比如 app.min.js
3. Sourcemap文件，比如app.js.map,这个文件保存了 `2->1` 映射

在Chrome等现代浏览器中你可以开启Sourcemap功能，这样你调试app.min.js的时候就像调试app.js一样方便

参考文章：
阮一峰的博文 http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html
英文	http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/

# charlesproxy
前面说的那些方法大多只能处理压缩的文件，无法处理混淆后的文件。

刚出炉的一片文章：使用 charlesproxy 的 map local 功能，这个功能可以在当前环境中用你的本地文件替代你的线上文件。
http://blog.icodeu.com/?p=709
