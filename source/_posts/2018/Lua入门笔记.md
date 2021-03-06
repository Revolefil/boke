---
title: Lua入门笔记
date: 2018-1-12 14:00:00
categories: 
    - Lua
tags:
    - Lua
photos:
    - /uploads/photos/1518315679054.jpg
---

## <font color='#5CACEE'>简介</font>
> Lua是一门脚本语言 其设计目的是为了嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能。Lua由标准C编写而成，几乎在所有操作系统和平台上都可以编译，运行。Lua并没有提供强大的库，这是由它的定位决定的。所以Lua不适合作为开发独立应用程序的语言。Lua 有一个同时进行的JIT项目，提供在特定平台上的即时编译功能。

<!-- more -->


### <font color='#CDAA7D'>Lua特征</font>

 - 轻量级 使用标准C语言开发 编译后才100多K
 - 可拓展 lua没有庞大的标准库，但是可以灵活使用宿主语言的功能
 - 支持面向过程和函数式编程
 - 自动内存管理 闭包 提供多线程（更像是协程）


## <font color='#5CACEE'>应用场景</font>

 - 游戏开发
 - 独立应用脚本
 - Web应用脚本
 - 程序拓展或插件（Nginx, MySQL proxy）
 - 安全系统 如入侵检测系统
 
## <font color='#5CACEE'>Lua安装</font>

### <font color='#CDAA7D'>Linux编译安装lua</font>
依赖readline.h ubuntu系统安装libreadline-dev 

```
wget http://www.lua.org/ftp/lua-5.3.4.tar.gz
tar xf lua-5.3.4.tar.gz
cd lua-5.3.4
make linux
make install
```
    cd src && install -p -m 0755 lua luac /usr/local/bin
    cd src && install -p -m 0644 lua.h luaconf.h lualib.h lauxlib.h lua.hpp /usr/local/include
    cd src && install -p -m 0644 liblua.a /usr/local/lib
    cd doc && install -p -m 0644 lua.1 luac.1 /usr/local/man/man1

Lua向系统安装了这些文件 接下来就可以使用Lua命令了


## <font color='#5CACEE'>Hello World</font>
用hello world作为lua第一个脚本的开始

lua也有一个交互式解释器 也可以写为后缀为.lua的文件来执行代码

```
vim hello.lua
```
写入
    
    print("Hello world!")

执行
```
lua hello.lua
```

## <font color='#5CACEE'>Lua的注释</font>

### <font color='#CDAA7D'>单行注释</font>
单行注释是两个减号

    -- 单行注释

### <font color='#CDAA7D'>多行注释</font>

    --[[
    多行注释
    多行注释
    --]]


### <font color='#CDAA7D'><font color='#5CACEE'>标示符</font>

Lua中的标示符和其他语言的标示符都基本一致

## <font color='#5CACEE'>关键字</font>
|关键字|关键字|关键字|关键字|
|-|-|-|-|
|and   |	break|	do	 |  else|
|elseif	 |   end|	false|	for|
|function|	if	|in|	local|
|nil|	not	|or	|repeat|
|return|	then|	true|	until|
|while|

Lua中的约定：以下划线开头的大写字母 被保留用于Lua的内部全局变量

## <font color='#5CACEE'>数据类型</font>
Lua有8个基本类型

|数据类型|描述|
|-|-|
|nil|表示无效值 空值|
|boolean| 只有两个值：true和false|
|number|表示双精度类型的实浮点数|
|string|字符串 可以用单引号也可以双引号|
|function|由C或者Lua编写的函数|
|userdata|表示任意存储在变量中的C数据结构|
|thread|表示执行的独立线程，用于执行协同程序|
|table|Lua 中的表（table）其实是一个"关联数组"|

使用type函数测试给定的值是什么类型 比如：
```
print(type(1))
print(type("hello"))
print(type(false))
print(type({}))
```
### <font color='#CDAA7D'>nil (空)</font>
nil类型表示没有任何有效值 nil类型只有一个值:nil

将全局变量或table表里的变量赋值nil 相当于删除了这个变量

### <font color='#CDAA7D'>boolean (布尔)</font>
boolean型只有两个值：true 和 false

**Lua只把false和nil看作是假 其他都为真！！！就算是0也是真！！！**

### <font color='#CDAA7D'>number (数字)</font>
Lua的number只有一种类型：double类型 所有的数字都是double类型的

### <font color='#CDAA7D'>string (字符串)</font>
单行字符串可以用双引号或者单引号表示
多行字符串用 [[string]] 表示 相当于python的三引号

**如果一个数字型的字符串与数字进行运算 Lua会将字符串转为number！与JavaScript相反**

Lua的字符串拼接使用".." 两个点 比如：
```
print("str"..1)
print(2 ..3 ..4)
```
如果两个数字做字符串拼接 数字后面一定要带个空格 否则解释器会解释错

Lua使用#来计算字符串的长度 就像是python的len()一样
```
str = "Hello world!"
print(#str)
print(#"lua")
```


### <font color='#CDAA7D'>table (表)</font>
Lua的table类型比较有意思 像是python的字典，列表，集合的合体
table类型的索引可以是数字 也可以是字符串
**Lua的数字索引是从1开始的！！！**
```
tbl1 = {}
tbl2 = {"a","b","c"}
tbl3 = {a=1,b=2}
print(tbl2[1])
print(tbl3["a"])
```
```
a = {}
a["key"] = "value"
a[1] = 2
print(a["key"])
print(a[1])
```

Lua取table中的数据可以用[] 如果key是字符串类型 还可以用.
比如：
```
tbl1 = {key="a"}
print(tbl1.key)
```

### <font color='#CDAA7D'>function (函数)</font>
Lua的函数跟JavaScript的函数很像 用function声明 还可以当作参数传给其他函数

```
function map(fn, tab)
    count = 1
    ltab = {}
    for k,v in pairs(tab) do
            ltab[k] = fn(v)
    end
    return ltab
end

map(function(x)
        return x*x
    end,
    {2,3,4,5})
    
for k,v in pairs(rst) do
    print("new: "..k.."="..v)
end

```

### <font color='#CDAA7D'>thread (线程)</font>
Lua里的线程其实就是协程

### <font color='#CDAA7D'>userdata (自定义类型)</font>
userdata 是一种用户自定义数据，用于表示一种由应用程序或 C/C++ 语言库所创建的类型，可以将任意 C/C++ 的任意数据类型的数据（通常是 struct 和 指针）存储到 Lua 变量中调用。

## <font color='#5CACEE'>变量</font>
默认情况下 Lua的变量总是全局的 不需要声明就可以直接赋值。访问一个不存在的变量会返回nil，想删除一个变量 将其赋值为nil就可以了
<br>
Lua有三种变量：全局变量 本地变量 表中的域
Lua除非用local声明为局部变量 否则都为全局变量

<br>
应该尽量使用局部变量

 - 避免变量冲突
 - 访问局部变量的速度更快

```
o = 10              --> 全局变量
function say()
   local o = 100    --> 局部变量
   print(o)
end
print(o)
```

### <font color='#CDAA7D'>变量赋值</font>
Lua的变量赋值也比较灵活
```
a,b,c = 1,2,3   --> a=1,b=2,c=3
a,b = b,a       --> swap a for b
a,b = 1,2,3     --> a=1,b=2 3 was drop
a,b,c = 1,2     --> a=1,b=2,c=nil
print(a,b,c)
```

### <font color='#CDAA7D'><font color='#5CACEE'>Lua循环</font>

### <font color='#CDAA7D'>while 循环</font>

    while (condition)
    do
        statements
    end
    
### <font color='#CDAA7D'>for 循环</font>
#### <font color='#DDA0DD'>数值for循环</font>

    for var=exp1,exp2,exp3 do
        statements
    end
var从exp1变化到exp2，每次变化以exp3为步长递增var，并执行一次"执行体"。exp3是可选的，如果不指定，默认为1。
```
for i=10,1,-1 do
    print(i)
end
```
#### <font color='#DDA0DD'>泛型for循环</font>
泛型for循环通过一个迭代器函数来遍历所有值

    for k,v in pairs(table) do
        statements
    end

    for i,v in ipairs(list) do
        statements
    end

    在lua中pairs与ipairs两个迭代器的用法相近，但有一点是不一样的：
    
    pairs可以遍历表中所有的key，并且除了迭代器本身以及遍历表本身还可以返回nil;
    
    但是ipairs则不能返回nil,只能返回数字0，如果遇到nil则退出。它只能遍历到表中出现的第一个不是整数的key


### <font color='#CDAA7D'>repeat...until 循环</font>
类似于C的do while循环 先做后判断

    repeat
        statements
    until(condition)
    
### <font color='#CDAA7D'>break 跳出循环</font>
如果遇到break 就终止循环 很不幸的是 Lua没有continue，但是可以用嵌套循环模拟

```
for i = 1, 10 do
    repeat
        if i == 5 then
            break
        end
        print(i)
    until true
end
```

## <font color='#5CACEE'>流程控制</font>
### <font color='#CDAA7D'>if 语句</font>

    if (condition)
    then
        statements
    end

### <font color='#CDAA7D'>if...else 语句</font>

    if (condition)
    then
        statements
    else
        statements
    end

## <font color='#5CACEE'>函数</font>
Lua的函数定义格式如下

    scope function name(arg1,arg2,...)
        statements
        return result
        
    scope 定义函数作用域
    name  函数名
    arg   参数
    statements  函数内部语句
    return  返回值
    
### <font color='#CDAA7D'>多返回值</font>
Lua可以一次返回多个值 同样需要多个参数来接收

```
s,e = string.find("www.hinote.ga","hinote")
print(s,e)
```
 
### <font color='#CDAA7D'>可变参数</font>
将Lua的参数放入一个叫arg的表中 #arg计算有多少个arg的值
用"..."来表示函数有可变参数

```
function args(...)
    local arg = {...}
    print(#arg)
end
```
## <font color='#5CACEE'>Lua 运算符</font>
设定A=10, B=20
### <font color='#CDAA7D'>算数运算符</font>

|操作符|描述|实例|
|-|-|-|
|+|加法|A+B=30|
|-|减法|A-B=-10|
|*|乘法|A*B=200|
|/|除法|B/A=2|
|%|取余|B%A=0|
|^|乘幂|A^2=100|


### <font color='#CDAA7D'>关系运算符</font>

|操作符|描述|实例|
|-|-|-|
|==	|等于|(A == B) 为 false|
|~=	|不等于|(A ~= B) 为 true|
|>	|大于|(A > B) 为 false|
|<|	小于|(A < B) 为 true|
|\>=	|大于等于|(A >= B) 返回 false|
|<=	|小于等于|(A <= B) 返回 true|


### <font color='#CDAA7D'>逻辑运算符</font>
A=true, B=false

|操作符|描述|实例|
|-|-|-|
|and|	逻辑与操作符|(A and B) 为 false|
|or	|逻辑或操作符|(A or B) 为 true|
|not|	逻辑非操作符|not(A and B) 为 true|

### <font color='#CDAA7D'>其他运算符</font>
A="Hello", B="World"
|操作符|描述|实例|
|-|-|-|
|..|连接两个字符串|A..B 返回 "Hello World"|
|#|返回字符串或表的长度|#A 返回 5|

### <font color='#CDAA7D'>算数优先级</font>

    ^
    not    - (unary)
    *      /
    +      -
    ..
    <      >      <=     >=     ~=     ==
    and
    or


## <font color='#5CACEE'>字符串</font>

### <font color='#CDAA7D'>字符串常用方法</font>

|方法|用途|
|-|-|
|string.upper|字符串转为全大写|
|string.lower|字符串转为全小写|
|string.gsub|字符串替换|
|string.find|字符串查找|
|string.reverse|字符串反转|
|string.format|格式化字符串|
|string.char|将ascii转为字符串|
|string.byte|将字符串转为ascii|
|string.len|计算字符串长度|
|string.rep|返回字符串的N个拷贝|
|..|连接两个字符串|
|string.gmatch|迭代器的方式匹配正则表达式|
|string.match|只寻找源字符串的第一个匹配|

## <font color='#5CACEE'>数组</font>

### <font color='#CDAA7D'>一维数组</font>
```
arr = {"Hello", "World"}
```

### <font color='#CDAA7D'>多维数组</font>
```
arr = {"Hello",{"Hello","Lua"}}
```

## <font color='#5CACEE'>迭代器</font>

### <font color='#CDAA7D'>泛型for迭代器</font>
泛型 for 在自己内部保存迭代函数，实际上它保存三个值：迭代函数、状态常量、控制变量。

泛型 for 迭代器提供了集合的 key/value 对，语法格式如下：
```
for k, v in pairs(t) do
    print(k, v)
end
```

下面我们看看泛型 for 的执行过程：

 - 首先，初始化，计算in后面表达式的值，表达式应该返回泛型 for 需要的三个值：迭代函数、状态常量、控制变量；与多值赋值一样，如果表达式返回的结果个数不足三个会自动用nil补足，多出部分会被忽略。
 - 第二，将状态常量和控制变量作为参数调用迭代函数（注意：对于for结构来说，状态常量没有用处，仅仅在初始化时获取他的值并传递给迭代函数）。
 - 第三，将迭代函数返回的值赋给变量列表。
 - 第四，如果返回的第一个值为nil循环结束，否则执行循环体。
 - 第五，回到第二步再次调用迭代函数


### <font color='#CDAA7D'>无状态迭代器</font>
无状态的迭代器是指不保留任何状态的迭代器，因此在循环中我们可以利用无状态迭代器避免创建闭包花费额外的代价。

每一次迭代，迭代函数都是用两个变量（状态常量和控制变量）的值作为参数被调用，一个无状态的迭代器只利用这两个值可以获取下一个元素。
这种无状态迭代器的典型的简单的例子是ipairs，它遍历数组的每一个元素

### <font color='#CDAA7D'>多状态迭代器</font>
很多情况下，迭代器需要保存多个状态信息而不是简单的状态常量和控制变量，最简单的方法是使用闭包，还有一种方法就是将所有的状态信息封装到table内，将table作为迭代器的状态常量，因为这种情况下可以将所有的信息存放在table内，所以迭代函数通常不需要第二个参数。


## <font color='#5CACEE'>table (表)</font>

table是Lua的一种数据结构来帮助我们创建不同的数据类型
Lua也是通过table来解决模块、包、和对象的

### <font color='#CDAA7D'>表的常用操作</font>

|方法|用途|
|-|-|
|table.concat|类似于python字符串的join|
|table.insert|在数组的指定位置插入一个值|
|table.remove|移除数组中的一个值|
|table.sort|对给定的table升序排序|

### <font color='#CDAA7D'><font color='#5CACEE'>模块与包</font>

Lua5.1开始 加入了标准的模块管理机制
Lua的模块是由变量和函数等元素组成的table, 最后返回这个table就可以了
<br>
test.lua
```
module = {}
function module.max(x,y)
    if (x > y) then
        return x
    else
        return y
    end
end
return module
```
引入也很简单
```
require("test")
module.max(2,3)         --> 引入的是模块return的名字
test = require("test")  --> 也可以这样将模块重命名
test.max(2,3)
```

### <font color='#CDAA7D'>模块加载路径</font>
Lua内使用package.path可以看到默认的搜索路径
如果找不到lua模块 则会去调用C库 使用package.cpath可以查看

也可以使用环境变量LUA_PATH来设置lua的搜索路径
C库的环境变量是LUA_CPATH


## <font color='#5CACEE'>元表 (Matatable)</font>
感觉像是python类的魔法方法，可以自己修改table的各种行为，比如两个table相加，又有点像JavaScript的原型链

高级话题 以后再研究


## <font color='#5CACEE'>协程</font>

Lua 协同程序(coroutine)与线程比较类似：拥有独立的堆栈，独立的局部变量，独立的指令指针，同时又与其它协同程序共享全局变量和其它大部分东西。

### <font color='#CDAA7D'>基本语法</font>

|方法|描述|
|-|-|
|coroutine.create|创建coroutine,参数是一个函数|
|coroutine.resume|重启coroutine，和create配合使用|
|coroutine.yield|挂起coroutine，将coroutine设置为挂起状态|
|coroutine.status|查看coroutine的状态|
|coroutine.wrap|也是创建一个coroutine|
|coroutine.running|返回正在运行的coroutine|

高级话题 用到的时候再研究


## <font color='#5CACEE'>文件IO</font>
Lua的IO库用于读取和处理文件 分为简单模式和完全模式

 - 简单模式 拥有一个当前输入和输出文件，并且提供针对文件的相关操作
 - 它以一种面对对象的形式，将所有的文件操作定义为文件句柄的方法

<br>
打开文件操作语句如下
```
file = io.open (filename [, mode])
```
其中mode和python的文件打开方式一摸一样 不再多说

### <font color='#CDAA7D'>简单模式</font>

#### <font color='#DDA0DD'>读取文件内容</font>
```
file = io.open("/etc/passwd","r")
io.input(file)      --> 设置输入文件为file
io.read(10)         --> 读取10个字节 默认是读取一行
io.close(file)      --> 关闭文件
```

#### <font color='#DDA0DD'>写入文件内容</font>
```
file = io.open("/tmp/lua","a")  --> 如果是w模式 文件存在则会清空文件
io.output(file)
io.write("Hello\n")
io.close(file)
```

#### <font color='#DDA0DD'>io.read有多种参数</font>

|模式|描述|
|-|-|
|"*n"  |   读取一个数字并返回它|
| "*a"   |  读取全部内容|
|"*l"  |   读取下一行 默认行为|
|number |  按照字节读取|

#### <font color='#DDA0DD'>其他的io方法</font>

|方法|描述|
|-|-|
|io.tmpfile|返回一个临时文件 "a"模式打开|
|io.type(file)|检测obj是否是个可用的文件句柄|
|io.flush|向文件写入缓冲中的所有数据|
|io.lines|返回一个迭代函数 每次调用返回一行|

### <font color='#CDAA7D'>完全模式</font>
通常我们会在同一时间处理多个文件 简单模式就不够用了

#### <font color='#DDA0DD'>文件读写</font>
```
f1 = io.open("/etc/passwd","r")
f1:read()     --> 没看错 就是:  
f1:close()
f2 = io.open("/tmp/lua","w")
f2:write("Hi!\n")
f2:close()
```

#### <font color='#DDA0DD'>其他方法</font>

|模式|描述|
|-|-|
|file:seek|获取和设置当前文件指针|
|file:flush|将缓冲区输入写入到文件|


## <font color='#5CACEE'>错误处理</font>

### <font color='#CDAA7D'>assert 和 error</font>
```
function add(a,b)
   assert(type(a) == "number", "a 不是一个数字")
   assert(type(b) == "number", "b 不是一个数字")
   return a+b
end
add(10)
```
实例中assert首先检查第一个参数，若没问题，assert不做任何事情；否则，assert以第二个参数作为错误信息抛出。
```
function add(a,b)
   error("错误了")
   return a+b
end
add(10)
```
error更像是python的raise 主动抛出一个异常

### <font color='#CDAA7D'>pcall 和 xpcall</font>
Lua中处理错误，可以使用函数pcall来包装需要执行的代码。

pcall传入需要运行的函数和函数的参数，如果函数运行的有错误就返回false 否则返回true 所以可以用if进行判断和处理

```
pcall(function(i) print(i) end, 33)
pcall(function(i) print(i) error("error") end, 33)
```

xpcall可以接收一个错误处理函数来查看错误发生时的调用栈

```
xpcall(function(i) print(i) error('error..') end, function() print(debug.traceback()) end, 33)
```
xpacall的第二个参数是传入了一个错误处理函数 可以在这个函数中使用debug库获取错误的额外信息了

 - debug.debug 提供一个Lua提示符 让用户来检查错误的原因
 - debug.traceback 根据调用栈来构建一个扩展的错误信息

## <font color='#5CACEE'>debug 调试</font>
> 未完待续。。。

## <font color='#5CACEE'>垃圾回收</font>
> 未完待续。。。

## <font color='#5CACEE'>面向对象</font>
> 未完待续。。。
