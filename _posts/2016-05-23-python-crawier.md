---
layout: post
title: Python爬虫
date: 2016-05-23 18:00:00
tags: Python
categories: Python
---


爬虫, 数据说话的时代, 具有数据收集的手段, 我觉得很重要, 因为这些数据能帮助我们更好的分析趋势, 在解决问题时提供良好的依据, 以上就是我学习爬虫的动力所在

* [静觅](http://cuiqingcai.com/1052.html)

## Python爬虫入门
Python爬虫有一些基础库, 我们就从基础库的学习, 开始爬虫之路

### Urllib的使用
在使用之前, 需要导入Urllib库

```python
import urllib2
```

#### 获取网页

```python
# 标准写法 (推荐)
request = urllib2.Request("http://www.baidu.com")
response = urllib2.urlopen(request)
print response.read()

# 简写
response = urllib2.urlopen("http://www.baidu.com")
print response.read()
```

```python
urlopen(url, data, timeout)
```

- 第一个参数`url`即为`URL`，第二个参数`data`是访问`URL`时要传送的数据，第三个`timeout`是设置超时时间。

- 第二三个参数是可以不传送的，`data`默认为空`None`，`timeout`默认为 `socket._GLOBAL_DEFAULT_TIMEOUT`

- 第一个参数`URL`是必须要传送的，在这个例子里面我们传送了百度的`URL`，执行`urlopen`方法之后，返回一个`response`对象，返回信息便保存在这里面。

#### POST与GET数据传送
##### POST

```python
import urllib
import urllib2

params = {"username": "123", "password": "abc"}
data = urllib.urlencode(params)
url = "http://www.baidu.com/login"
request = urllib2.Request(url, data)
response = urllib2.urlopen(request)
print response.read()
```

##### GET

```python
import urllib
import urllib2

params = {"username": "123", "password": "abc"}
data = urllib.urlencode(params)
url = "http://www.baidu.com/login"
geturl = url + "?" + data
request = urllib2.Request(geturl, data)
response = urllib2.urlopen(request)
print response.read()
```

#### Urllib高级使用
##### 设置headers

```python
url = "http://www.baidu.com/login"
user_agent = "Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)"
headers = {"User-Agent": user_agent}
params = {"username": "123", "password": "abc"}
data = urllib.urlencode(params)
request = urllib2.Request(url. data, headers)
response = urllib2.urlopen(request)
print response.read()
```
利用设置`headers`可以用来对付`"反盗链"`

```python
headers = {'User-Agent': 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)', 'Referer':'http://www.zhihu.com/articles'}  
```

需要注意的`headers`属性

> - User-Agent : 有些服务器或 Proxy 会通过该值来判断是否是浏览器发出的请求
> - Content-Type : 在使用 REST 接口时，服务器会检查该值，用来确定 HTTP Body 中的内容该怎样解析。
> - application/xml ： 在 XML RPC，如 RESTful/SOAP 调用时使用
> - application/json ： 在 JSON RPC 调用时使用
> - application/x-www-form-urlencoded ： 浏览器提交 Web 表单时使用
> - 在使用服务器提供的 RESTful 或 SOAP 服务时， Content-Type 设置错误会导致服务器拒绝服务

##### 设置Porxy(代理)
假如一个网站它会检测某一段时间某个IP 的访问次数，如果访问次数过多，它会禁止你的访问. 所以你可以设置一些代理服务器来帮助你做工作，每隔一段时间换一个代理

`urllib2`默认会使用环境变量`http_proxy` 来设置`HTTP Proxy`

设置方法: 

```python
enable_proxy = True
proxy_handler = urllib2.ProxyHandler({"http": "http://some-proxy.com:8080"})
null_proxy_handler = urllib2.ProxyHandler({})
if enable_proxy:
	opener = urllib2.build_opener(proxy_handler)
else: 
	opener = urllib2.build_opener(null_proxy_handler)
urllib2.install_opener(opener)
```

##### 设置TimeOut

```python
# data参数空出未填
response = urllib2.urlopen("http://www.baidu.com", timeout = 10)
# 填写了data参数
response = urllib2.urlopen("http://www.baidu.com", data, 10)
```

##### 使用PUT和DELETE方法
`http`协议有六种请求方法，`get`,`head`,`put`,`delete`,`post`,`options`，我们有时候需要用到`PUT`方式或者`DELETE`方式请求。

```python
import urllib2
request = urllib2.Request(uri, data=data)
request.get_method = lambda: 'PUT' # or 'DELETE'
response = urllib2.urlopen(request)
```

### URLError异常处理
可能产生`URLError`的原因

- 网络无连接，即本机无法上网
- 连接不到特定的服务器
- 服务器不存在

使用`try...except`捕获异常

```python
request = urllib2.Request("http://www.xxxxxxxx.com")
try:
	response = urllib2.urlopen(request)
except Exception, e:
	print e.reason
```

#### HTTPError
服务器不能处理的请求, 会返回一个`HTTPError`, `HTTPError`是`URLError`的子类, 以下列举常见`HTTPError`错误码

> - 100：继续  客户端应当继续发送请求。客户端应当继续发送请求的剩余部分，或者如果请求已经完成，忽略这个响应。
> - 101： 转换协议  在发送完这个响应最后的空行后，服务器将会切换到在Upgrade 消息头中定义的那些协议。只有在切换新的协议更有好处的时候才应该采取类似措施。
> - 102：继续处理   由WebDAV（RFC 2518）扩展的状态码，代表处理将被继续执行。
> - 200：请求成功      处理方式：获得响应的内容，进行处理
> - 201：请求完成，结果是创建了新资源。新创建资源的URI可在响应的实体中得到    处理方式：爬虫中不会遇到
> - 202：请求被接受，但处理尚未完成    处理方式：阻塞等待
> - 204：服务器端已经实现了请求，但是没有返回新的信 息。如果客户是用户代理，则无须为此更新自身的文档视图。    处理方式：丢弃
> - 300：该状态码不被HTTP/1.0的应用程序直接使用， 只是作为3XX类型回应的默认解释。存在多个可用的被请求资源。    处理方式：若程序中能够处理，则进行进一步处理，如果程序中不能处理，则丢弃
> - 301：请求到的资源都会分配一个永久的URL，这样就可以在将来通过该URL来访问此资源    处理方式：重定向到分配的URL
> - 302：请求到的资源在一个不同的URL处临时保存     处理方式：重定向到临时的URL
> - 304：请求的资源未更新     处理方式：丢弃
> - 400：非法请求     处理方式：丢弃
> - 401：未授权     处理方式：丢弃
> - 403：禁止     处理方式：丢弃
> - 404：没有找到     处理方式：丢弃
> - 500：服务器内部错误  服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。一般来说，这个问题都会在服务器端的源代码出现错误时出现。
> - 501：服务器无法识别  服务器不支持当前请求所需要的某个功能。当服务器无法识别请求的方法，并且无法支持其对任何资源的请求。
> - 502：错误网关  作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应。
> - 503：服务出错   由于临时的服务器维护或者过载，服务器当前无法处理请求。这个状况是临时的，并且将在一段时间以后恢复。

在错误判断上我们预先判断子类错误, 如果判断不到, 再判断父类错误, 在输出错误信息之前还可以使用`hasattr`方法提前对属性进行判断

```python
request = urllib2.Request("http://www.xxxxxxxx.com")
try:
	response = urllib2.urlopen(request)
except urllib2.HTTPError, e:
	print e.code
except urllib2.URLError, e:
	if hasattr(e, "code"):
		print e.code
	if hasattr(e, "reason"):
		print e.reason
else:
	print "OK"
```

### Cookie的使用
Cookie，指某些网站为了辨别用户身份、进行session跟踪而储存在用户本地终端上的数据（通常经过加密）

#### Opener
`opener`是一个urllib2.OpenerDirector的实例, 获取一个`URL`时会使用一个`opener`, `urlopen`就是一个特殊的`opener`实例, 如果我们使用到了`Cookie`, 仅仅使用`urlopen`是不能达到目的的

#### Cookielib
`cookielib`模块的主要作用是提供可存储`cookie`的对象，以便于与`urllib2`模块配合使用来访问Internet资源。`Cookielib`模块非常强大，我们可以利用本模块的`CookieJar`类的对象来捕获`cookie`并在后续连接请求时重新发送，比如可以实现模拟登录功能。该模块主要的对象有`CookieJar`、`FileCookieJar`、`MozillaCookieJar`、`LWPCookieJar`。

##### 获取Cookie

```python
import urllib2
import cookielib
#声明一个CookieJar对象实例来保存cookie
cookie = cookielib.CookieJar()
#利用urllib2库的HTTPCookieProcessor对象来创建cookie处理器
handler=urllib2.HTTPCookieProcessor(cookie)
#通过handler来构建opener
opener = urllib2.build_opener(handler)
#此处的open方法同urllib2的urlopen方法，也可以传入request
response = opener.open('http://www.baidu.com')
for item in cookie:
    print 'Name = '+item.name
    print 'Value = '+item.value
```

##### 保存Cookie到文件

```python
import cookielib
import urllib2

#设置保存cookie的文件，同级目录下的cookie.txt
filename = 'cookie.txt'
#声明一个MozillaCookieJar对象实例来保存cookie，之后写入文件
cookie = cookielib.MozillaCookieJar(filename)
#利用urllib2库的HTTPCookieProcessor对象来创建cookie处理器
handler = urllib2.HTTPCookieProcessor(cookie)
#通过handler来构建opener
opener = urllib2.build_opener(handler)
#创建一个请求，原理同urllib2的urlopen
response = opener.open("http://www.baidu.com")
#保存cookie到文件
cookie.save(ignore_discard=True, ignore_expires=True)
```

- `ignore_discard`的意思是即使`cookies`将被丢弃也将它保存下来
- `ignore_expires`的意思是如果在该文件中`cookies`已经存在，则覆盖原文件写入

##### 从文件中读取Cookie

```python
import cookielib
import urllib2

#创建MozillaCookieJar实例对象
cookie = cookielib.MozillaCookieJar()
#从文件中读取cookie内容到变量
cookie.load('cookie.txt', ignore_discard=True, ignore_expires=True)
#创建请求的request
req = urllib2.Request("http://www.baidu.com")
#利用urllib2的build_opener方法创建一个opener
opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cookie))
response = opener.open(req)
print response.read()
```

##### 利用Cookie模拟网站登录

```python
import urllib
import urllib2
import cookielib

filename = 'cookie.txt'
#声明一个MozillaCookieJar对象实例来保存cookie，之后写入文件
cookie = cookielib.MozillaCookieJar(filename)
opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cookie))
postdata = urllib.urlencode({
			'stuid':'201200131012',
			'pwd':'23342321'
		})
#登录教务系统的URL
loginUrl = 'http://jwxt.sdu.edu.cn:7890/pls/wwwbks/bks_login2.login'
#模拟登录，并把cookie保存到变量
result = opener.open(loginUrl,postdata)
#保存cookie到cookie.txt中
cookie.save(ignore_discard=True, ignore_expires=True)
#利用cookie请求访问另一个网址，此网址是成绩查询网址
gradeUrl = 'http://jwxt.sdu.edu.cn:7890/pls/wwwbks/bkscjcx.curscopre'
#请求访问成绩查询网址
result = opener.open(gradeUrl)
print result.read()
```

### 正则表达式
> 正则表达式是对字符串操作的一种逻辑公式，就是用事先定义好的一些特定字符、及这些特定字符的组合，组成一个“规则字符串”，这个“规则字符串”用来表达对字符串的一种过滤逻辑。

![](http://qiniu.cuiqingcai.com/wp-content/uploads/2015/02/20130515113723855.png)

#### 相关注解
##### 数量词的贪婪模式与非贪婪模式
正则表达式通常用于在文本中查找匹配的字符串。`Python`里数量词默认是贪婪的（在少数语言里也可能是默认非贪婪），总是尝试匹配尽可能多的字符；非贪婪的则相反，总是尝试匹配尽可能少的字符。例如：正则表达式`”ab*”`如果用于查找`”abbbc”`，将找到`”abbb”`。而如果使用非贪婪的数量词`”ab*?”`，将找到`”a”`。

##### 反斜杠
与大多数编程语言相同，正则表达式里使用`”\”`作为转义字符，这就可能造成反斜杠困扰。假如你需要匹配文本中的字符`”\”`，那么使用编程语言表示的正则表达式里将需要4个反斜杠`”\\\\”`：前两个和后两个分别用于在编程语言里转义成反斜杠，转换成两个反斜杠后再在正则表达式里转义成一个反斜杠。

`Python`里的原生字符串很好地解决了这个问题，这个例子中的正则表达式可以使用`r”\\”`表示。同样，匹配一个数字的`”\\d”`可以写成`r”\d”`。有了原生字符串，妈妈也不用担心是不是漏写了反斜杠，写出来的表达式也更直观勒。

#### Re模块

```python
#返回pattern对象
re.compile(string[,flag])  
#以下为匹配所用函数
re.match(pattern, string[, flags])
re.search(pattern, string[, flags])
re.split(pattern, string[, maxsplit])
re.findall(pattern, string[, flags])
re.finditer(pattern, string[, flags])
re.sub(pattern, repl, string[, count])
re.subn(pattern, repl, string[, count])
```

`pattern`可以理解为一个匹配模式，我们需要利用`re.compile`方法获取`pattern`

在参数中我们传入了原生字符串对象，通过compile方法编译生成一个pattern对象，然后我们利用这个对象来进行进一步的匹配。

参数flag是匹配模式，取值可以使用按位或运算符’|’表示同时生效，比如re.I | re.M。

可选值有：

> - re.I(全拼：IGNORECASE): 忽略大小写（括号内是完整写法，下同）
> - re.M(全拼：MULTILINE): 多行模式，改变'^'和'$'的行为（参见上图）
> - re.S(全拼：DOTALL): 点任意匹配模式，改变'.'的行为
> - re.L(全拼：LOCALE): 使预定字符类 \w \W \b \B \s \S 取决于当前区域设定
> - re.U(全拼：UNICODE): 使预定字符类 \w \W \b \B \s \S \d \D 取决于unicode定义的字符属性
> - re.X(全拼：VERBOSE): 详细模式。这个模式下正则表达式可以是多行，忽略空白字符，并可以加入注释。 

*注：以下七个方法中的flags同样是代表匹配模式的意思，如果在pattern生成时已经指明了flags，那么在下面的方法中就不需要传入这个参数了。*

##### re.match(pattern, string[, flags])
这个方法将会从`string`（我们要匹配的字符串）的开头开始，尝试匹配`pattern`，一直向后匹配，如果遇到无法匹配的字符，立即返回`None`，如果匹配未结束已经到达`string`的末尾，也会返回`None`。两个结果均表示匹配失败，否则匹配`pattern`成功，同时匹配终止，不再对`string`向后匹配。

```python
__author__ = 'CQC'
# -*- coding: utf-8 -*-

#导入re模块
import re

# 将正则表达式编译成Pattern对象，注意hello前面的r的意思是“原生字符串”
pattern = re.compile(r'hello')

# 使用re.match匹配文本，获得匹配结果，无法匹配时将返回None
result1 = re.match(pattern,'hello')
result2 = re.match(pattern,'helloo CQC!')
result3 = re.match(pattern,'helo CQC!')
result4 = re.match(pattern,'hello CQC!')

#如果1匹配成功
if result1:
    # 使用Match获得分组信息
    print result1.group()
else:
    print '1匹配失败！'


#如果2匹配成功
if result2:
    # 使用Match获得分组信息
    print result2.group()
else:
    print '2匹配失败！'


#如果3匹配成功
if result3:
    # 使用Match获得分组信息
    print result3.group()
else:
    print '3匹配失败！'

#如果4匹配成功
if result4:
    # 使用Match获得分组信息
    print result4.group()
else:
    print '4匹配失败！'
```

匹配分析

1. 第一个匹配，`pattern`正则表达式为`’hello’`，我们匹配的目标字符串`string`也为`hello`，从头至尾完全匹配，匹配成功。

2. 第二个匹配，`string`为`helloo CQC`，从`string`头开始匹配`pattern`完全可以匹配，`pattern`匹配结束，同时匹配终止，后面的`o CQC`不再匹配，返回匹配成功的信息。

3. 第三个匹配，`string`为`helo CQC`，从`string`头开始匹配`pattern`，发现到 `‘o’` 时无法完成匹配，匹配终止，返回`None`

4. 第四个匹配，同第二个匹配原理，即使遇到了空格符也不会受影响。

我们还看到最后打印出了`result.group()`，这个是什么意思呢？下面我们说一下关于`match`对象的的属性和方法
`Match`对象是一次匹配的结果，包含了很多关于此次匹配的信息，可以使用`Match`提供的可读属性或方法来获取这些信息。

> 属性：

> 1. string: 匹配时使用的文本。
> 2. re: 匹配时使用的Pattern对象。 
> 3. pos: 文本中正则表达式开始搜索的索引。值与Pattern.match()和Pattern.seach()方法的同名参数相同。
> 4. endpos: 文本中正则表达式结束搜索的索引。值与Pattern.match()和Pattern.seach()方法的同名参数相同。
> 5. lastindex: 最后一个被捕获的分组在文本中的索引。如果没有被捕获的分组，将为None。
> 6. lastgroup: 最后一个被捕获的分组的别名。如果这个分组没有别名或者没有被捕获的分组，将为None。

> 方法：

> 1. group([group1, …]):
获得一个或多个分组截获的字符串；指定多个参数时将以元组形式返回。group1可以使用编号也可以使用别名；编号0代表整个匹配的子串；不填写参数时，返回group(0)；没有截获字符串的组返回None；截获了多次的组返回最后一次截获的子串。
> 2. groups([default]):
以元组形式返回全部分组截获的字符串。相当于调用group(1,2,…last)。default表示没有截获字符串的组以这个值替代，默认为None。
> 3. groupdict([default]):
返回以有别名的组的别名为键、以该组截获的子串为值的字典，没有别名的组不包含在内。default含义同上。
> 4. start([group]):
返回指定的组截获的子串在string中的起始索引（子串第一个字符的索引）。group默认值为0。
> 5. end([group]):
返回指定的组截获的子串在string中的结束索引（子串最后一个字符的索引+1）。group默认值为0。
> 6. span([group]):
返回(start(group), end(group))。
> 7. expand(template):
将匹配到的分组代入template中然后返回。template中可以使用\id或\g、\g引用分组，但不能使用编号0。\id与\g是等价的；但\10将被认为是第10个分组，如果你想表达\1之后是字符’0’，只能使用\g0。

```python
# -*- coding: utf-8 -*-
#一个简单的match实例

import re
# 匹配如下内容：单词+空格+单词+任意字符
m = re.match(r'(\w+) (\w+)(?P<sign>.*)', 'hello world!')

print "m.string:", m.string
print "m.re:", m.re
print "m.pos:", m.pos
print "m.endpos:", m.endpos
print "m.lastindex:", m.lastindex
print "m.lastgroup:", m.lastgroup
print "m.group():", m.group()
print "m.group(1,2):", m.group(1, 2)
print "m.groups():", m.groups()
print "m.groupdict():", m.groupdict()
print "m.start(2):", m.start(2)
print "m.end(2):", m.end(2)
print "m.span(2):", m.span(2)
print r"m.expand(r'\g \g\g'):", m.expand(r'\2 \1\3')
 
### output ###
# m.string: hello world!
# m.re: 
# m.pos: 0
# m.endpos: 12
# m.lastindex: 3
# m.lastgroup: sign
# m.group(1,2): ('hello', 'world')
# m.groups(): ('hello', 'world', '!')
# m.groupdict(): {'sign': '!'}
# m.start(2): 6
# m.end(2): 11
# m.span(2): (6, 11)
# m.expand(r'\2 \1\3'): world hello!
```

##### re.search(pattern, string[, flags])
`search`方法与`match`方法极其类似，区别在于`match()`函数只检测`re`是不是在`string`的开始位置匹配，`search()`会扫描整个`string`查找匹配，`match（）`只有在0位置匹配成功的话才有返回，如果不是开始位置匹配成功的话，`match()`就返回`None`。同样，`search`方法的返回对象同样`match()`返回对象的方法和属性。

```python
#导入re模块
import re

# 将正则表达式编译成Pattern对象
pattern = re.compile(r'world')
# 使用search()查找匹配的子串，不存在能匹配的子串时将返回None
# 这个例子中使用match()无法成功匹配
match = re.search(pattern,'hello world!')
if match:
    # 使用Match获得分组信息
    print match.group()
### 输出 ###
# world
```

##### re.split(pattern, string[, maxsplit])
按照能够匹配的子串将`string`分割后返回列表。`maxsplit`用于指定最大分割次数，不指定将全部分割

```python
import re

pattern = re.compile(r'\d+')
print re.split(pattern,'one1two2three3four4')

### 输出 ###
# ['one', 'two', 'three', 'four', '']
```

##### re.findall(pattern, string[, flags])
搜索string，以列表形式返回全部能匹配的子串

```python
import re

pattern = re.compile(r'\d+')
print re.findall(pattern,'one1two2three3four4')

### 输出 ###
# ['1', '2', '3', '4']
```

##### re.finditer(pattern, string[, flags])
搜索string，返回一个顺序访问每一个匹配结果（Match对象）的迭代器

```python
import re

pattern = re.compile(r'\d+')
for m in re.finditer(pattern,'one1two2three3four4'):
    print m.group(),

### 输出 ###
# 1 2 3 4
```

##### re.sub(pattern, repl, string[, count])
使用`repl`替换`string`中每一个匹配的子串后返回替换后的字符串。
当`repl`是一个字符串时，可以使用`\id`或`\g`、`\g`引用分组，但不能使用编号0。
当`repl`是一个方法时，这个方法应当只接受一个参数（`Match`对象），并返回一个字符串用于替换（返回的字符串中不能再引用分组）。
`count`用于指定最多替换次数，不指定时全部替换。

```python
import re

pattern = re.compile(r'(\w+) (\w+)')
s = 'i say, hello world!'

print re.sub(pattern,r'\2 \1', s)

def func(m):
    return m.group(1).title() + ' ' + m.group(2).title()

print re.sub(pattern,func, s)

### output ###
# say i, world hello!
# I Say, Hello World!
```

##### re.subn(pattern, repl, string[, count])
返回 (sub(repl, string[, count]), 替换次数)

```python
import re
 
pattern = re.compile(r'(\w+) (\w+)')
s = 'i say, hello world!'
 
print re.subn(pattern,r'\2 \1', s)
 
def func(m):
    return m.group(1).title() + ' ' + m.group(2).title()
 
print re.subn(pattern,func, s)
 
### output ###
# ('say i, world hello!', 2)
# ('I Say, Hello World!', 2)
```

在上面我们介绍了7个工具方法，例如`match`，`search`等等，不过调用方式都是 `re.match`，`re.search`的方式，其实还有另外一种调用方式，可以通过`pattern.match`，`pattern.search`调用，这样调用便不用将`pattern`作为第一个参数传入了，大家想怎样调用皆可。

函数API列表

> - match(string[, pos[, endpos]]) | re.match(pattern, string[, flags])
> - search(string[, pos[, endpos]]) | re.search(pattern, string[, flags])
> - split(string[, maxsplit]) | re.split(pattern, string[, maxsplit])
> - findall(string[, pos[, endpos]]) | re.findall(pattern, string[, flags])
> - finditer(string[, pos[, endpos]]) | re.finditer(pattern, string[, flags])
> - sub(repl, string[, count]) | re.sub(pattern, repl, string[, count])
> - subn(repl, string[, count]) |re.sub(pattern, repl, string[, count])

