# 帮助命令及任务计划
****  
##   利用type来区分内建命令与外部命令  

- 内建命令  
-
	shiyanlou:~/ $ type exit[16:04:58]
	exit is a shell builtin  

- 外部命令  
-
	shiyanlou:~/ $ type vim[16:07:20]
	vim is /usr/bin/vim

- ls为例（命令别名）
- 
	shiyanlou:~/ $ type ls [16:07:25]
	ls is an alias for ls --color=tty
## 帮助指令  
- 最简单：help
- 更具体：man  
- 最具体：info    

****
## crontab命令  
> 通过 crontab 命令，我们可以在固定的间隔时间执行指定的系统指令或 shell　script 脚本。时间间隔的单位可以是分钟、小时、日、月、周的任意组合。  

	NAME
       crontab - maintain crontab files for individual users (Vixie Cron)

	SYNOPSIS
       crontab [ -u user ] file
       crontab [ -u user ] [ -i ] { -e | -l | -r }

### 首先启动rsyslog（本地Ubuntu系统会自动启动）  
> 通过日志中的信息来了解我们的任务是否真正的被执行了  

	shiyanlou:~/ $ sudo service rsyslog start     
	 * Starting enhanced syslogd rsyslogd       [ OK ] 
	shiyanlou:~/ $ sudo cron -f &
	[1] 3342  

### 添加一个计划任务  

	shiyanlou:~/ $ crontab -e  
> 选[2]---vim---在最后一行按照格式添加  

### 查看
	shiyanlou:~/ $ crontab -l
	# Edit this file to introduce tasks to be run by cron.
	...
	# m h  dom mon dow   command
	*/1 * * * * touch /home/shiyanlou/$(date +\%Y\%m\%d\%H\%M\&S)  

> 注意：此处学习时，错将M和D写为大写字母，这种错误以后会小心避免  
> 之前的5个*号位置依次对应：
> {minute}:0-59,每分钟对应\*或、\*/1  
> {hour}:0-23  
> {day-of-month}：1-31  
> {month}：1-12    
> {day-of-week}：0-6，SUN-0    
> 后面的部分是可执行的{full-path-to-shell-script} 

### 判断添加任务是否成功-方法1  
> ps aux | grep cron

	shiyanlou:~/ $ ps aux | grep cron
	root      3342  0.0  0.0  53904  4032 pts/0    SN   16:18   0:00 sudo cron -f
	root      3347  0.0  0.0  30148  2984 pts/0    SN   16:18   0:00 cron -f
	shiyanl+  3600  0.0  0.0  15384   928 pts/0    S+   16:37   0:00 grep --color=auto --exclud
	e-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn cron


### 判断添加任务是否成功-方法2 
> pgrep cron  

	shiyanlou:~/ $ pgrep cron
	3347
### 删除计划任务  
> crontab -r  

	shiyanlou:~/ $ crontab -r  
	shiyanlou:~/ $ crontab -l  
	no crontab for shiyanlou
### /etc/中关于crontab的文件内容  
> 对应目录下的命令按照不同周期执行  
> 默认执行时间点可以自定义  

	shiyanlou:/etc/ $ ll /etc/ |grep cron
	drwxr-xr-x  2 root root   4.0K 6月   9  2018 cron.d
	drwxr-xr-x  2 root root   4.0K 5月  29 16:18 cron.daily
	drwxr-xr-x  2 root root   4.0K 6月   9  2018 cron.hourly
	drwxr-xr-x  2 root root   4.0K 6月   9  2018 cron.monthly
	-rw-r--r--  1 root root    722 4月   6  2016 crontab
	drwxr-xr-x  2 root root   4.0K 6月   9  2018 cron.weekly  

**** 
## 小结  
- 学习了帮助指令help,man,info的区别和联系  
- 学习了利用crontab指令实现计划任务的相关操作   
- Linux的命令熟练度提高以后，逐渐体会到windows所不具有的快捷感

**** 
# 挑战：备份日志  
	 0 3 * * * cp /var/log/alternatives.log /home/shiyanlou/tmp/$(date +\%Y-\%m-\%d)    

## 小结  
- 仿照教学实例实现了该题
- 通过挑战了解熟悉了对计划命令时间点的自定义操作    
- 答辩月底结束，近期会尽快完成Linux部分的学习。
