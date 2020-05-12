# 简单的文本处理  
****  
## tr命令  
> 用来删除一段文本信息中的某些文字
> 将一段文本信息进行转换  
### 一般格式  
	tr [option]...SET1 [SET2]
### 常用选项  
#### -d:删除和set1匹配的字符，注意不是全词匹配也不是按字符顺序匹配  

	shiyanlou:~/ $ echo "hello shiyanlou" | tr -d 'olh'
	e siyanu
  
#### -s:去除set1指定的在输入文本中连续并重复的字符  

	shiyanlou:~/ $ echo "hello shiyanlou" | tr -s 'l'  
	helo shiyanlou  
  
#### 大小写转换：[:lower:];[:upper:];or[a-z];[A-Z]  

	shiyanlou:~/ $ echo "hello shiyanlou" | tr 'a-z' 'A-Z'
	HELLO SHIYANLOU
	shiyanlou:~/ $ echo "HellO ShiYanLou" | tr 'A-Z' 'a-z'
	hello shiyanlou		
****
## col命令  
> 将"Tab"转换为空格
> 将空格转换为"Tab"        
### 一般格式  
	col [option] 
### 常用选项  
#### -x:将Tab转换为空格  

	shiyanlou:~/ $ cat -A /etc/protocols| tail -n 5
	manet^I138^I^I^I# MANET Protocols [RFC5498]$
	hip^I139^IHIP^I^I# Host Identity Protocol$
	shim6^I140^IShim6^I^I# Shim6 Protocol [RFC5533]$
	wesp^I141^IWESP^I^I# Wrapped Encapsulating Security Payload$
	rohc^I142^IROHC^I^I# Robust Header Compression$
	
	//^I为Tab键的转义字符  
  
	shiyanlou:~/ $ cat /etc/protocols| col -x | cat -A | tail -n 5
	manet   138                     # MANET Protocols [RFC5498]$
	hip     139     HIP             # Host Identity Protocol$
	shim6   140     Shim6           # Shim6 Protocol [RFC5533]$
	wesp    141     WESP            # Wrapped Encapsulating Security Payload$
	rohc    142     ROHC            # Robust Header Compression$

#### -h:将空格转换为Tab（默认选项）  

	shiyanlou:~/ $ cat /etc/protocols| col -h | cat -A | tail -n 5
	manet^I138^I^I^I# MANET Protocols [RFC5498]$
	hip^I139^IHIP^I^I# Host Identity Protocol$
	shim6^I140^IShim6^I^I# Shim6 Protocol [RFC5533]$
	wesp^I141^IWESP^I^I# Wrapped Encapsulating Security Payload$
	rohc^I142^IROHC^I^I# Robust Header Compression$  

****  
## join命令  
> 用于将两个文件中包含相同内容的那一行合并在一起  

	shiyanlou:~/ $ echo '1 hello' > file1
	shiyanlou:~/ $ echo '1 shiyanlou' > file2
	shiyanlou:~/ $ join file1 file2
	1 hello shiyanlou  

### 一般格式  
	join [option]... file1 file2  
### 常用选项  
1. -t：指定分隔符，默认为空格  
2. -i：忽略大小写的差异  
3. -1：指明第一个文件要用哪个字段来对比，默认对比第一个字段  
4. -2：指明第二个文件要用哪个字段来对比，默认对比第一个字段  
****
#### 例1
	shiyanlou:~/ $ sudo join -t':' /etc/passwd /etc/shadow
	root:x:0:0:root:/root:/bin/bash:*:17590:0:99999:7:::
	daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin:*:17590:0:99999:7:::
	...
#### 例2  
	shiyanlou:~/ $ sudo join -t':' -1 4 /etc/passwd -2 3 /etc/group
	0:root:x:0:root:/root:/bin/bash:root:x:
	1:daemon:x:1:daemon:/usr/sbin:/usr/sbin/nologin:daemon:x:  
	...  
> 将两个文件合并，指定以':'作为分隔符, 分别比对第4和第3个字段
****  
## paste命令  
> 在不对比数据的情况下，简单地将多个文件合并一起，以Tab隔开  
### 一般格式  
	paste [option] file...
### 常用选项  
> -d:指定合并的分隔符，默认为Tab  
> -s	:不合并到一行，每个文件为一行  

	shiyanlou:~/ $ echo 'hello' file1 
	hello file1
	shiyanlou:~/ $ echo 'shiyanlou' file2
	shiyanlou file2
	shiyanlou:~/ $ echo www.shiyanlou.com > file3
	shiyanlou:~/ $ paste -d ':' file1 file2 file3
	1 hello:1 shiyanlou:www.shiyanlou.com
	shiyanlou:~/ $ paste -s file1 file2 file3    
	1 hello
	1 shiyanlou
	www.shiyanlou.com  
****    
## 小结  
1. 命令不是十分常用，但熟练掌握之后很实用，在特定情况下可以减轻很多工作量
2. 经过这些天的学习，逐步感受到了Linux系统中的操作逻辑，随着熟练度的提高，逐渐明确命令的区别和联系，可以提高操作的效率以及体会到windows平台所没有的优势和乐趣  
3. 毕业事情有点多，尽量不请假，保持学习  
