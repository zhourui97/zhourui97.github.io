---
layout: page
title:	sql injection
category: blog
description: 
---
# Preface

# Blind SQL
不显示错误，盲注

# Wide Charset Injection
在GBK宽字节中

	%df'
	%df%27
	%df%5c%27 # %df%5c 是gbk 中是一个汉字

%5C 是\

