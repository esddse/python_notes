
[参考博客](http://www.cnblogs.com/tina-python/p/5508402.html)

## 普通字符和11个元字符

|符号|含义|正则表达式示例|可匹配字符串示例|
|:--:|:-:|:-----------:|:------------:|
|普通字符|匹配自身|abc|abc|
|.|匹配任意除换行符"\n"外的字符(在DOTALL模式中也能匹配换行符|a.c|abc|
|\\|转义字符，使后一个字符改变原来的意思|a\.c;a\\c|a.c;a\\c|
|\*|匹配前一个字符0或多次|abc\*|ab;abccc|
|+|匹配前一个字符1次或无限次|abc+|abc;abccc|
|?|匹配一个字符0次或1次|abc?|ab;abc|
|^|匹配字符串开头。在多行模式中匹配每一行的开头|^abc|abc|
|$|匹配字符串末尾，在多行模式中匹配每一行的末尾|abc$|abc|
|\||或。匹配\|左右表达式任意一个，从左到右匹配，如果\|没有包括在()中，则它的范围是整个正则表达式|abc\|def|abc;def|
|\{\}|{m}匹配前一个字符m次，{m,n}匹配前一个字符m至n次，若省略n，则匹配m至无限次|ab{1,2}c|abc;abbc|
|\[\]|字符集。对应的位置可以是字符集中任意字符。字符集中的字符可以逐个列出，也可以给出范围，如[abc]或[a-c]。[^abc]表示取反，即非abc。所有特殊字符在字符集中都失去其原有的特殊含义。用\反斜杠转义恢复特殊字符的特殊含义。|a[bcd]e|abe;ace;ade|
|()|被括起来的表达式将作为分组，从表达式左边开始没遇到一个分组的左括号“（”，编号+1.分组表达式作为一个整体，可以后接数量词。表达式中的\|仅在该组中有效。|(abc){2};a(123\|456)c|abcabc;a456c|

## 预定义字符集(可写在字符集[...]中)

|符号|含义|正则表达式示例|可匹配字符串示例|
|:--:|:--:|:----------:|:-------------:|
|\d|数字:[0-9]|a\dc|a1c|
|\D|非数字:[^\d]|a\Dc|abc|
|\s|匹配任何空白字符:[<空格>\t\r\n\f\v]|a\sc|a c|
|\S|非空白字符:[^\s]|a\Sc|abc|
|\w|匹配包括下划线在内的任何字字符:[A-Za-z0-9_]|a\wc|abc|
|\W|匹配非字母字符，即匹配特殊字符|a\Wc|a c|
|\A|仅匹配字符串开头,同^|\Aabc|abc|
|\Z|仅匹配字符串结尾，同$|abc\Z|abc|
|\b|匹配\w和\W之间，即匹配单词边界匹配一个单词边界，也就是指单词和空格间的位置。例如， 'er\b' 可以匹配"never" 中的 'er'，但不能匹配 "verb" 中的 'er'。|\babc\b;a\b!bc|空格abc空格;a!bc|
|\B|[^\b]|a\Bbc|abc|

## 贪婪匹配与惰性匹配

贪婪匹配会尽可能长地匹配目标字符串

惰性匹配一旦发现匹配成功则停止匹配

re默认是贪婪匹配

转换成惰性匹配只需要在其后加``?``

例子：
```
贪婪匹配: *, +, ?, {m,n}
惰性匹配: *?, +?, ??, {m,n}?
```

## 常用功能函数

### compile

编译正则表达式模式，返回一个对象的模式

```
re.compile(pattern, flags=0)
```

例子:
```
import re
tt = "Tina is a good girl, she is cool, clever, and so on..."
rr = re.compile(r'\w*oo\w*')
print(rr.findall(tt))   #查找所有包含'oo'的单词
执行结果如下：
['good', 'cool']
```

### match

决定RE是否在字符串刚开始的位置匹配。//注：这个方法并不是完全匹配。当pattern结束时若string还有剩余字符，仍然视为成功。想要完全匹配，可以在表达式末尾加上边界匹配符'$'

```
re,match(pattern, string, flags=0)
```

例子：
```
print(re.match('com','comwww.runcomoob').group())
print(re.match('com','Comwww.runcomoob',re.I).group())
执行结果如下：
com
com
```

### search

re.search函数会在字符串内查找模式匹配,只要找到第一个匹配然后返回，如果字符串没有匹配，则返回None

```
re.search(pattern, string, flags=0)
```

例子:
```
print(re.search('\dcom','www.4comrunoob.5com').group())
执行结果如下：
4com
```

** 注意 ** match和search一旦匹配成功，就是一个match object对象，而match object对象有以下方法：

* group() 返回被 RE 匹配的字符串
* start() 返回匹配开始的位置
* end() 返回匹配结束的位置
* span() 返回一个元组包含匹配 (开始,结束) 的位置
* group() 返回re整体匹配的字符串，可以一次输入多个组号，对应组号匹配的字符串

例子:
```
import re
a = "123abc456"
 print(re.search("([0-9]*)([a-z]*)([0-9]*)",a).group(0))   #123abc456,返回整体
 print(re.search("([0-9]*)([a-z]*)([0-9]*)",a).group(1))   #123
 print(re.search("([0-9]*)([a-z]*)([0-9]*)",a).group(2))   #abc
 print(re.search("([0-9]*)([a-z]*)([0-9]*)",a).group(3))   #456
###group(1) 列出第一个括号匹配部分，group(2) 列出第二个括号匹配部分，group(3) 列出第三个括号匹配部分。###
``` 

### findall

re.findall遍历匹配，可以获取字符串中所有匹配的字符串，返回一个列表

```
re.findall(pattern, string, flags=0)
```

例子：
```
import re
tt = "Tina is a good girl, she is cool, clever, and so on..."
rr = re.compile(r'\w*oo\w*')
print(rr.findall(tt))
print(re.findall(r'(\w)*oo(\w)',tt))#()表示子表达式 
执行结果如下：
['good', 'cool']
[('g', 'd'), ('c', 'l')]
```

### split

按照能够匹配的子串将string分割后返回列表

```
re.split(pattern, string[, maxsplit])
maxsplit用于指定最大分割次数，不指定将全部分割
```

例子:
```
print(re.split('\d+','one1two2three3four4five5'))
执行结果如下：
['one', 'two', 'three', 'four', 'five', '']
```

### sub

使用re替换string中每一个匹配的子串后返回替换后的字符串

```
re.sub(pattern, repl, string, count)
```

例子：
```
import re
text = "JGood is a handsome boy, he is cool, clever, and so on..."
print(re.sub(r'\s+', '-', text))
执行结果如下：
JGood-is-a-handsome-boy,-he-is-cool,-clever,-and-so-on...
其中第二个函数是替换后的字符串；本例中为'-'

第四个参数指替换个数。默认为0，表示每个匹配项都替换。
```
