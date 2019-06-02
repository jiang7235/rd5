# 数据流重定向  
****  
## 简单的重定向  
	shiyanlou:~/ $ mkdir Documents
	shiyanlou:~/ $ cat > Documents/test.c <<EOF
	heredoc> #include <stdio.h>
	heredoc> {
	heredoc>        printf("hello,world\n");
	heredoc>        return 0;
	heredoc> }
	heredoc> EOF
	shiyanlou:~/ $ cd Documents 
	shiyanlou:Documents/ $ less test.c 
	#include <stdio.h>
	{
	        printf("hello,world\n");
	        return 0;
	}
	shiyanlou:Documents/ $ cd .. ;cat Documents/test.c
	#include <stdio.h>
	{
		printf("hello,world\n");
		return 0;
	}
	shiyanlou:~/ $ echo "hi" | cat
	hi
	shiyanlou:~/ $ echo "hello shiyanlou" > redirect[15:45:46]
	shiyanlou:~/ $ cat redirect 
	hello shiyanlou  
****  
## 标准的错误重定向  
> 隐藏某些错误或警告    

	shiyanlou:~/ $ cat Documents/test.c hello.c[15:46:20]
	#include <stdio.h>
	{
		printf("hello,world\n");
		return 0;
	}  
	cat: hello.c: 没有那个文件或目录  

### 方法1  
	shiyanlou:~/ $ cat Documents/test.c hello.c >somefile 2>&1
	shiyanlou:~/ $ cat somefile 
	#include <stdio.h>
	{
		printf("hello,world\n");
		return 0;
	}
	cat: hello.c: 没有那个文件或目录  
> 注意，将标准错误stderr重定向到标准输出stdout，再将标准输出重定向到文件，注意将重定向到文件写到前面  
> 注意，要在在>前加上&  
### 方法2  
	shiyanlou:~/ $ cat Documents/test.c hello.c &>somefilehell
	shiyanlou:~/ $ cat somefilehell 
	#include <stdio.h>
	{
		printf("hello,world\n");
		return 0;
	}
	cat: hello.c: 没有那个文件或目录  
> 只用bash提供的特殊的重定向符号"&"将标准错误和标准输出同时重定向到文件  
****  
## tee命令  
> 同时重定向到多个文件  

	shiyanlou:~/ $ echo 'hello shiyanlou' | tee hello
	hello shiyanlou
	shiyanlou:~/ $ cat hello
	hello shiyanlou 
****  
## exec命令  
> 实现永久重定向  
> 使用指定的命令替换当前的Shell
> 即使用一个进程替换当前进程，或指定新的重定向  

	shiyanlou:~/ $ zsh
	shiyanlou:~/ $ exec 1>somefile
	//使用exec替换当前进程的重定向，将标准输出重定向到一个文件  
	//后面你执行的命令的输出都将被重定向到文件中,直到
	//你退出当前子shell，或取消exec的重定向  
	shiyanlou:~/ $ ls 
	shiyanlou:~/ $ exit
	shiyanlou:~/ $ cat somefile
	anaconda3
	Code
	Desktop
	Documents
	hello
	redirect
	somefile
	somefilehell  

****  
## 创建输出文件描述符  
> shell中共有9个文件描述符    
> 0-stdin  
> 1-stdout  
> 2-stderr  
> 3-8默认没有开启    
> 查看当前Shell进程中打开的文件描述符  

	shiyanlou:~/ $ cd /dev/fd ; ls -Al
	总用量 0
	lrwx------ 1 shiyanlou shiyanlou 64 6月   2 15:40 0 -> /dev/pts/0
	lrwx------ 1 shiyanlou shiyanlou 64 6月   2 15:40 1 -> /dev/pts/0
	lrwx------ 1 shiyanlou shiyanlou 64 6月   2 16:08 10 -> /dev/pts/0
	lr-x------ 1 shiyanlou shiyanlou 64 6月   2 16:08 12 -> /usr/share/zsh/functions/Completion
	.zwc
	lrwx------ 1 shiyanlou shiyanlou 64 6月   2 15:40 2 -> /dev/pts/0  

> 使用exec命令创建新的文件描述符  

	shiyanlou:fd/ $ zsh
	shiyanlou:fd/ $ exec 3>somefile
	zsh: 没有那个文件或目录: somefile
	shiyanlou:/proc/ $ cd ~
	shiyanlou:~/ $ zsh
	e%                                                                                         
	shiyanlou:~/ $ exec 3>somefile
	shiyanlou:~/ $ cd /dev/fd ; ls -Al ; cd -
	总用量 0
	lrwx------ 1 shiyanlou shiyanlou 64 6月   2 16:14 0 -> /dev/pts/0
	lrwx------ 1 shiyanlou shiyanlou 64 6月   2 16:14 1 -> /dev/pts/0
	lrwx------ 1 shiyanlou shiyanlou 64 6月   2 16:15 10 -> /dev/pts/0
	lr-x------ 1 shiyanlou shiyanlou 64 6月   2 16:15 12 -> /usr/share/zsh/functions/Completion
	.zwc
	lrwx------ 1 shiyanlou shiyanlou 64 6月   2 16:14 2 -> /dev/pts/0
	l-wx------ 1 shiyanlou shiyanlou 64 6月   2 16:15 3 -> /home/shiyanlou/somefile
	~
	shiyanlou:~/ $ echo "this is test" >&3
	shiyanlou:~/ $ cat somefile
	this is test
	shiyanlou:~/ $ exit  

> 关闭文件描述符  

	shiyanlou:~/ $ exec 3>&-
	shiyanlou:~/ $ cd /dev/fd ; ls -Al ; cd -
	总用量 0
	lrwx------ 1 shiyanlou shiyanlou 64 6月   2 16:14 0 -> /dev/pts/0
	lrwx------ 1 shiyanlou shiyanlou 64 6月   2 16:21 1 -> /dev/pts/0
	lrwx------ 1 shiyanlou shiyanlou 64 6月   2 16:21 10 -> /dev/pts/0
	lr-x------ 1 shiyanlou shiyanlou 64 6月   2 16:21 12 -> /usr/share/zsh/functions/Completion
	.zwc
	lrwx------ 1 shiyanlou shiyanlou 64 6月   2 16:21 2 -> /dev/pts/0
	~  

****  
## 完全屏蔽命令的输出  
> Linux中，有一个被称为“黑洞”的设备文件  
> 所有导入`/dev/null`（空设备）的数据都将被“吞噬”   
> 读取它则会立即得到一个EOF  

	shiyanlou:~/ $ cat Documents/test.c nefile 1>/dev/null 2>&1  

****  
## xargs命令  
> 将参数列表转换成小块分段传递给其他命令，以避免参数列表过长   

	shiyanlou:~/ $ cut -d: -f1 < /etc/passwd | sort | xargs echo [16:29:09]
	_apt backup bin colord daemon games gnats irc labex list lp mail man messagebus mongodb mys
	ql news nobody proxy pulse redis root rtkit shiyanlou sshd sync sys systemd-bus-proxy syste
	md-network systemd-resolve systemd-timesync usbmux uucp www-data  
> 用于将/etc/passwd文件按:分割取第一个字段排序后，使用echo命令生成一个列表  


****  
## 小结  
1. 学习了用于数据流重定向的常用指令  
2. 由于缺少实际应用的场景，对这部分的理解不是很透彻  
3. 需要进一步补充些相关的知识，并注意与其他命令的联系  

****  
# 挑战：历史命令  
	shiyanlou:~/ $ cat data1|cut -c 8-|sort|uniq -dc|sort -nr -k1 |head -3 > /home/shiyanlou/result  
> 显示结果  

	shiyanlou:~/ $ less /home/shiyanlou/result      
    105 ls
     90 openstack compute service list
     67 vim /etc/nova/nova.conf
****  
## 小结  
1. 命令掌握的过于简单，需要补充一些细节  
2. 管道符使用时，前后可以不加空格  
    
### [uniq命令补充](https://blog.csdn.net/xifeijian/article/details/9209627)  
#### 常用指令  
> -i:忽略大小写  
> -c:进行计数（在首行显示该行的重复次数）  
> -u:显示唯一行(仅显示不重复的行)  
> -d:显示重复行（仅显示存在重复的行）

#### uniq仅作用于相邻行，因此需要结合sort使用  
	$ sort|uniq  

### head/tail命令的常用选项  
> -c：显示文本的（head）前n字节或（tail）后n字节
> -n：显示文本的（head）前n行或（tail）后n行
