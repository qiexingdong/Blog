---
title: lua
date: 2023-03-13 20:53:00
tags: [lua,笔记]
categories : Lua
updated: 2023-03-13 20:53:00
index_img: /img/文章封面图/4.JPG
sticky: 
---
<!--more-->
# lua语法
1.变量
* 默认的变量都是全局变量，可以加关键词local进行修饰
``` lua
a = 10//全局变量
local b = 100//局部变量
```
* 任何一个没有被声明过的变量都是nil，相当于c中的NULL
* 可以同时赋值
``` lua
a,b = 10,20
a,b,c = 10,20//c为nil
```
* lua只有一种数值型number，支持16进制计数法、科学计数法等等
```lua
a = 2e6
b = 0x11
```
2.字符串
```lua
a = '字符串'
b = "字符串"
c = [[字符串]]//三者效果一样
d = a..b//连接a和b
e = toString(10)//转换成为字符串
f = toNumber("10")//转换成为数字，若转化失败返回值就是nil
print(#a)//可以查到字符串的长度
s = string.char(0x11,0x12,0x13,0x14)
r = string.byte(s,2)//获取s的第二个字符
```
3.函数
```lua
function 函数名()
函数体
end
```
4.数组
* 数字作为数组下标
```lua
a = {1,"abc",200}
print(a[1])//获取数组中第一个元素
table.insert(a,123)//在尾部插入元素
table.insert(a,2，123)//在数组第二个位置插入元素
table.remove(a,2)//移除数组第二个元素
print(#a)//获取数组中元素个数
```
* 字符串作为数组下标
``` lua
a = {
a = 123,
b = "你好",
c = function hello()
end
}
print(a["a"])
print(a.a)//这两种方法都能打印数组的第一个元素
```
* lua中所有的全局变量都在_G这个table中

5.布尔类型
* lua中只有nil和false代表假，0代表真
  
6.if语句
```lua
if 1>2 then
print("yes")
elseif 1<2 then
print("false")
else
print("equal")
end
```
7.循环
```lua
for i = 1,10,2 do
print(i)
end//相当于for(int i = 1;i <=10 ; i += 2)

n = 10
while n > 1 do
print(n)
n = n - 1//lua中没有自减和自增运算符
end
```