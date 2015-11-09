---
layout: page
title:	
category: blog
description: 
---
# Preface

pythone 一切皆对象

# Multi paradigm，多范式
操作符其实是对象的方法, 

	'abc' + 'xyz' 
	'abc'.__add__('xyz')

	(1.8).__mul__(2.0)
	True.__or__(False)

Python的多范式依赖于Python对象中的特殊方法(special method, magic method):. Use `dir(1)` to list all magic method:

	> dir(1)
	 `__add__` `__init__`

内置函数也是映射magic method:

	len([1,2,3])
	[1,2,3].__len__()

list index

	li[3]
	li.__getitem__(3)
	
	li[3] = 0
	li.__setitem__(3, 0)
	{'a':1, 'b':2}.__delitem__('a')

function object

	class SampleMore(object):
    def __call__(self, a):
        return a + 5

	add = SampleMore()     # A function object
	print(add(2))          # Call function    

# Context Manager, 上下文管理器

	f = open("new.txt", "w")
	print(f.closed)               # whether the file is open
	f.write("Hello World!")
	f.close()
	print(f.closed)

有了CM 后，当代码进入`with .. as f` 定义的环境时，调用`f.__enter()`, 离开时，自动调用`f.__exit__()` (f.close() 的多范式)

	with open("a.txt", "w") as f:
		print(f.closed)
		f.write("hello world!")

	print(f.closed)

可以查看到f 的magic method

	> dir(f)

# Class and Object

	class MyStuff(object):

		def __init__(self):
			self.tangerine = "And now a thousand years between"

		def apple(self):
			print "I AM CLASSY APPLES!"

	obj = MyStuff();

## Inheritance

	class Parent(object):

		def altered(self):
			print "PARENT altered()"

	class Child(Parent):

		def altered(self):
			print "CHILD, BEFORE PARENT altered()"
			super(Child, self).altered()

	Child().altered()

super inherits init

	class Child(Parent):

		def __init__(self, stuff):
			self.stuff = stuff
			super(Child, self).__init__()

# Object Property
# todo
> http://python.jobbole.com/82622/
