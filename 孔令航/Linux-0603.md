# 正则表达式基础  
****  
## 正则表达式  
> Regular Expression，在代码中常简写为 regex、regexp 或 RE  
> 简单的说形式和功能上正则表达式和我们前面讲的通配符很像，不过它们之间又有很大差别，特别在于一些特殊的匹配字符的含义上  
****  
## 基本语法    
> 一个正则表达式通常被称为一个模式（pattern），为用来描述或者匹配一系列符合某个句法规则的字符串  
### 一般格式  
1. \\：转义字符  
2. ^：匹配输入字符的开始位置  
3. $：匹配输入字符的结束位置  
4. {n}:o{2},n个
5. {n,}:至少n个
6. {n,m}:至少n个，至多m个
7. *：匹配0个或多个  
8. +：匹配1个或多个  
9. ?：匹配0个或1个  
10. \.:匹配除"\n"之外的所有任何单个字符 
11. (pattern)：匹配pattern并获取这一匹配的字符串  
12. x|y:匹配x或者y  
13. [xyz]：字符集合，匹配所包含的任意一个字符  
14. [^xyz]：排除型字符集合，匹配未包含的任意一个字符  
15. [a-z]：字符范围，匹配范围内的任意一个字符
16. [^a-z]：字符范围，匹配范围外的任意一个字符
### 优先级  
1. \:转义字符  
2. (),(?:),(?:),[]:括号们  
3. *,+,?,{n},{n,},{n,m}：限定符    
4. ^,$,\任何元字符：定位点和序列  
5. |:选择  

> 优先级从上到下，从左到右  

****  
## [grep指令](https://blog.csdn.net/wyqwilliam/article/details/83831947)  
> global regular regular expression print 
> Linux 下最强大的**文本搜索**命令  

### 命令格式  
	grep [option] "string_to_find" filename  
### 常见选项  
（1）-i：忽略搜索字符串的大小写

（2）-v：取反，即输出不匹配的那些文本行

（3）-n：输出行号

（4）-l：输出能够匹配模式的文件名，相反的选项为-L

（5）-q：静默输出  

（6）-c：计算匹配成功的行数

（7）-o：只输出匹配到的文本部分

（8）-r：grep的参数filename为目录时可以加上本选项表示递归搜索

（9）-e：该选项加上正则表达式就是一个需要匹配的模式

	shiyanlou:~/ $ export | grep ".*yanlou$"
	HOME=/home/shiyanlou
	LOGNAME=shiyanlou
	MAIL=/var/mail/shiyanlou
	USER=shiyanlou  
（10）-n：表示打印匹配行号  

	shiyanlou:~/ $ grep -rnI "shiyanlou" ~
	/home/shiyanlou/Desktop/xfce4-terminal.desktop:150:Path=/home/shiyanlou
	/home/shiyanlou/.zsh_history:30:: 1559194612:0;grep -rnI "shiyanlou" ~ 
	//'$'表示一行的末尾  
（11）-b：将二进制文件作为文本来匹配  
（12）-A n：A-After，表示出了列出匹配行之外，还列出后面的n行
（13）-B n：B-Before，表示出了列出匹配行之外，还列出前面的n行  
     
### 支持的三种正则表达式引擎  
1. -E:POSIX扩展正则表达式，ERE  
2. -G:POSIX扩展正则表达式，ERE  
3. -P:Perl正则表达式，PCRE  

### 特殊符号  
1. [:alnum:]	代表英文大小写字母及数字，亦即 0-9, A-Z, a-z  
2. [:alpha:]	代表任何英文大小写字母，亦即 A-Z, a-z  
3. [:blank:]	代表空白键与 [Tab] 按键两者  
4. [:cntrl:]	代表键盘上面的控制按键，亦即包括 CR, LF, Tab, Del.. 等等  
5. [:digit:]	代表数字而已，亦即 0-9  
6. [:graph:]	除了空白字节 (空白键与 [Tab] 按键) 外的其他所有按键  
7. [:lower:]	代表小写字母，亦即 a-z  
8. [:print:]	代表任何可以被列印出来的字符  
9. [:punct:]	代表标点符号 (punctuation symbol)，亦即：" ' ? ! ; : # $...  
10. [:upper:]	代表大写字母，亦即 A-Z  
11. [:space:]	任何会产生空白的字符，包括空白键, [Tab], CR 等等  
12. [:xdigit:]	代表 16 进位的数字类型，因此包括： 0-9, A-F, a-f 的数字与字节  
****  
	shiyanlou:~/ $ echo 'zero\nzo\nzoo' | grep 'z.*o'
	zero
	zo
	zoo
	shiyanlou:~/ $ echo '1234\nabcd' | grep '[a-z]'  
	abcd
	shiyanlou:~/ $ echo '1234\nabcd' | grep '[0-9]'
	1234
	shiyanlou:~/ $ echo '1234\nabcd' | grep '[[:digit:]]'
	1234
	shiyanlou:~/ $ echo '1234\nabcd' | grep '[A-Z]'      
	shiyanlou:~/ $ echo '1234\nabcd' | grep '[a-z]'
	abcd
	shiyanlou:~/ $ echo '1234\nabcd' | grep '[[:alnum:]]'
	1234
	abcd
	shiyanlou:~/ $ echo '1234\nabcd' | grep '[[:alpha:]]' 
	abcd

> 之所以要使用特殊符号，是因为[a-z]不是在所有情况下都管用，在使用时请确保当前语系的影响，使用[:lower:]则不会有这个问题  
****  
### "^"在括号内表示排除，不在括号内表示行首  
	shiyanlou:~/ $ echo 'geek\ngoog' | grep '[^o]'
	geek
	goog
	shiyanlou:~/ $ echo 'geek\ngoog' | grep '[^oe]'     
	geek
	goog  
### 使用扩展正则表达式，ERE  
	$ grep -E  
	$ egrep  
> 以上两种均可  
****  
	shiyanlou:~/ $ echo 'zero\nzo\nzoo' | grep -E 'zo{1}'[16:07:09]
	zo
	zoo
	shiyanlou:~/ $ echo 'zero\nzo\nzoo' | egrep 'zo{1}'  [16:12:00]
	zo
	zoo
	shiyanlou:~/ $ echo 'zero\nzo\nzoo' | egrep 'zo{1,}'[16:12:06]
	zo
	zoo  
****  
	shiyanlou:~/ $ echo 'www.shiyanlou.com\nwww.baidu.com\nwww.google.com' | egrep 'www\.(shiya
	nlou|google)\.com'
	www.shiyanlou.com
	www.google.com
	shiyanlou:~/ $ echo 'www.shiyanlou.com\nwww.baidu.com\nwww.google.com' | egrep -v 'www\.bai
	du\.com' 
	www.shiyanlou.com
	www.google.com  
****  
##　sed流编辑器  
> "sed - stream editor for filtering and transforming text "，意即，用于过滤和转换文本的流编辑器  

### 命令格式  
	$ sed [参数]... [执行命令] [输入文件]...
	e.g.  
	shiyanlou:~/ $ sed -i 's/sad/happy' test  
### 常用参数  
1. -n：安静模式，只打印受影响的行，默认打印输入数据的全部内容  
2. -e：用于在脚本中添加多个执行命令一次执行，在命令行中执行多个命令通常不需要加该参数  
3. -f filename：指定执行filename文件中的命令  
4. -r：使用扩展正则表达式，默认为标准正则表达式  
5. -i：将直接修改输入文件内容，而不是打印到标准输出设备  

### 常用选项  
	$[n1][,n2]command    
	$[n1][~step]command
> 其中,n1,n2表示输入内容的行号，他们之间的逗号','表示从n1到n2行，波浪线'~'表示从n1开始以step为步进的所有行  
> 常用的command如下：  
1. s：行内替换  
2. c：整行替换  
3. a：插入到指定行的后面  
4. i：插入到指定行的前面  
5. p：打印指定行，通常与-n参数配合使用  
6. d：删除指定行  

****  
	shiyanlou:~/ $ nl passwd| sed -n '2,5p'
     2daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
     3bin:x:2:2:bin:/bin:/usr/sbin/nologin
     4sys:x:3:3:sys:/dev:/usr/sbin/nologin
     5sync:x:4:65534:sync:/bin:/bin/sync  
	//打印2-5行  
	shiyanlou:~/ $ nl passwd| sed -n '1~2p'
     1root:x:0:0:root:/root:/bin/bash
     3bin:x:2:2:bin:/bin:/usr/sbin/nologin
     5sync:x:4:65534:sync:/bin:/bin/sync
     7man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
     9mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
    11uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
    13www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
    15list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
    17gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
    19systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
    21systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
    23_apt:x:104:65534::/nonexistent:/bin/false
    25colord:x:106:112:colord colour management daemon,,,:/var/lib/colord:/bin/false
    27pulse:x:108:113:PulseAudio daemon,,,:/var/run/pulse:/bin/false
    29usbmux:x:110:46:usbmux daemon,,,:/var/lib/usbmux:/bin/false
    31labex:x:6000:6000::/home/labex:/usr/bin/zsh
    33mongodb:x:112:65534::/home/mongodb:/bin/false  
	// 打印奇数行  
****  
	shiyanlou:~/ $ sed -n 's/shiyanlou/hehe/gp' passwd 
	hehe:x:5000:5000::/home/hehe:/usr/bin/zsh  
	//行内替换，s，全局替换文本中的"shiyanlou"替换为"hehe"，并只打印替换的那一行  
	shiyanlou:~/ $ nl passwd | grep "shiyanlou"
    30shiyanlou:x:5000:5000::/home/shiyanlou:/usr/bin/zsh
	shiyanlou:~/ $ sed -n "21c\www.shiyanlou.com" passwd 
	www.shiyanlou.com  
	//行间替换，c，删除第21行，并只打印替换的那一行  
****  
## 小结  
1. 初步学习了正则表达式，结合grep，sed等命令，练习了正则表达式的使用方法。 
2. 文中有许多部分讲述的不够详尽，需要补充许多相关的知识  
3. 近期事情比较多，会尽快结束Linux部分的联系   
