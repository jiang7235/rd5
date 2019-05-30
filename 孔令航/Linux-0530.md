# 命令执行顺序控制与管道  
****
## 获得上次命令的返回结果  
	$ echo $?  
> 成功为0，不成功为1  

	shiyanlou:~/ $ [[ 1 -lt 3 ]]
	shiyanlou:~/ $ echo $?
	0
	shiyanlou:~/ $ [[ 1 -gt 3 ]]
	shiyanlou:~/ $ echo $?      
	1
## 竖线'|'及双竖线'||'指令   
###  '|'
> 管道，将竖线前的命令的输出(stdout)作为竖线后的输入(stdin)  

	shiyanlou:~/ $ du -h -d 0 *.zip ~ | sort
	8.0K	shiyanlou_1.zip
	8.0K	shiyanlou_9.zip
	8.0K	shiyanlou.zip
	92M/home/shiyanlou  

### '||'  
> 分隔多条命令  
> 前者为真，则不执行后者  
> 前者为假，则继续执行后者  

	shiyanlou:~/ $ [[ 1 -lt 3 ]] || echo hello
	shiyanlou:~/ $ [[ 1 -gt 3 ]] || echo hello
	hello
> 用来判断文件是否存在，不存在则创建  

	shiyanlou:~/ $ [[ -f test.txt ]] || touch test.txt 
	shiyanlou:~/ $ ls
	anaconda3  Desktop  home           shiyanlou.tar.gz  test.txt  

## 连接符'&'及双连接符'&&'指令   
###  '&'
> 同时执行多条指令，不论是否成功    

	shiyanlou:~/ $ [[ 1 -lt 3 ]] & echo hello          [13:31:18]
	hello

### '&&'  
> 同时执行多条指令，遇到错误时，不再执行后续指令  

	shiyanlou:~/ $ [[ 1 -lt 3 ]] && echo hello
	hello
	shiyanlou:~/ $ [[ 1 -gt 3 ]] && echo hello
## 联合使用'&&'and'||'  
	shiyanlou:~/ $ [[ 1 -gt 3 ]] && [[ 1 -lt 4 ]] || echo hello                 
	hello
	shiyanlou:~/ $ [[ 1 -lt 3 ]] && [[ 1 -gt 4 ]] || echo hello
	hello
	shiyanlou:~/ $ [[ 1 -lt 3 ]] && [[ 1 -lt 4 ]] || echo hello
	shiyanlou:~/ $ echo $? 
	0	
****  
## [cut命令](https://blog.csdn.net/appke846/article/details/80367395)  
> 打印某一行的某一字段  
### cut一般格式为：
> cut [options] file1 file2
### 下面介绍其可用选项：
> -c list 指定剪切字符数  
> -f field 指定剪切域数  
> -d指定与空格和tab键不同的域分隔符 

	shiyanlou:~/ $ cut /etc/passwd -d ':' -f 1,6
	root:/root
	daemon:/usr/sbin
	...
	//打印以冒号分隔的第一个和第六个字段

> -c用来指定剪切范围  
> -c 1，5-7剪切第1个字符，然后是第5到第7个字符  
> -c1-50 剪切前50个字符  
> -f 格式与-c相同  
> -f 15剪切第1域，第5域  
> -f 1,10-12剪切第1域，第10域到第12域  

	shiyanlou:~/ $ cut /etc/passwd -c -2
	ro
	//前两个，包括第二个
	shiyanlou:~/ $ cut /etc/passwd -c 2-
	oot:x:0:0:root:/root:/bin/bash
	//第二个之后的，包括第二个  
	shiyanlou:~/ $ cut /etc/passwd -c 2
	o  
	//第二个
	shiyanlou:~/ $ cut /etc/passwd -c 2-5
	oot:  
	//第2到第5个，包括第5个  
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
（9）-n：表示打印匹配行号  

	shiyanlou:~/ $ grep -rnI "shiyanlou" ~
	/home/shiyanlou/Desktop/xfce4-terminal.desktop:150:Path=/home/shiyanlou
	/home/shiyanlou/.zsh_history:30:: 1559194612:0;grep -rnI "shiyanlou" ~ 
	//'$'表示一行的末尾

### 补充：指定/排除文件
（1）--include：指定需要搜索的文件

（2）--exclude：排除需要搜索的文件

（3）--exclude-dir：排除需要搜索的目录
****  
## wc命令  
> 简单的计数工具  

	shiyanlou:~/ $ wc -w /etc/passwd
	52 /etc/passwd
	//'word'单词数
	shiyanlou:~/ $ wc -c /etc/passwd
	1816 /etc/passwd
	//'c'字节数
	shiyanlou:~/ $ wc -m /etc/passwd
	1816 /etc/passwd
	//'m'字符数	
	shiyanlou:~/ $ wc -l /etc/passwd
	34 /etc/passwd
	//'l'行数	
	shiyanlou:~/ $ wc -L /etc/passwd
	85 /etc/passwd
	//'L'最长行字节数  
> 结合管道
  
	shiyanlou:~/ $ ls -dl /etc/*/ | wc -l[13:43:50]
	102
****
## sort命令  
> 对File参数指定的文件中的行排序，并将结果写到标准输出  
### 命令格式  
	$ sort [-option] [file or stdin]  
### 常用选项  
1. -f  ：忽略大小写的差异，例如 A 与 a 视为编码相同；
2. -b  ：忽略最前面的空格符部分；
3. -M  ：以月份的名字来排序，例如 JAN, DEC 等等的排序方法；
4. -n  ：使用『纯数字』进行排序(默认是以字典来排序的，即字母)；
5. -r  ：反向排序；
6. -u  ：就是 uniq ，相同的数据中，仅出现一行代表；
7. -t  ：分隔符，默认是用 [tab] 键来分隔；
8. -k  ：以那个区间 (field) 来进行排序的意思

	`shiyanlou:~/ $ cat /etc/passwd | sort -t':' -k 3 -n`  
	`root:x:0:0:root:/root:/bin/bash`  
****  
## uniq命令  
> 用于过滤或者输出重复行  
	shiyanlou:~/ $ history | cut -c 8- | cut -d ' ' -f 1 | uniq  
	history  
****  
## 小结  
> 指令的基本功能介绍的很简单  
> 需要补充很多没有提到的知识  
> 要多多练习来提高命令的熟练度  
> 练习和补充知识有点拖慢进度，不过也还好  
