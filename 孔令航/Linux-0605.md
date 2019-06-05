# Linux下安装软件    
****  
## apt 包管理工具    
> Advance Packaging Tool（高级包装工具）  
### 一般格式  

### apt-get  
1. install:其后加上软件包名，用于安装一个软件包  
2. update:从软件源镜像服务器上下载/更新用于更新本地软件源的软件包列表  
3. upgrade:升级本地可更新的全部软件包，但存在依赖问题时将不会升级，通常会在更新之前执行一次update  
4. dist-upgrade:解决依赖关系并升级(存在一定危险性)  
5. remove:移除已安装的软件包，包括与被移除软件包有依赖关系的软件包，但不包含软件包的配置文件  
6. autoremove:移除之前被其他软件包依赖，但现在不再被使用的软件包  
7. purge:与remove相同，但会完全移除软件包，包含其配置文件  
8. clean:移除下载到本地的已经安装的软件包，默认保存在/var/cache/apt/archives/  
9. autoclean:移除已安装的软件的旧版本软件包  

### 常用参数  
1. -y:自动回应是否安装软件包的选项，在一些自动化安装脚本中使用这个参数将十分有用  
2. -s：模拟安装  
3. -q：静默安装方式，指定多个q或者-q=#,#表示数字，用于设定静默级别，这在你不想要在安装软件包时屏幕输出过多时很有用  
4. -f：修复损坏的依赖关系  
5. -d：只下载不安装  
6. --reinstall：重新安装已经安装但可能存在问题的软件包  
7. --install-suggests：同时安装APT给出的建议安装的软件包  
### 更新软件  
#### 更新软件源  
	$ sudo apt-get update
#### 升级没有依赖问题的软件包  
	$ sudo apt-get upgrade  
#### 升级并解决依赖关系  
	$ sudo apt-get dist-upgrade  
### 删除软件  
	$ sudo apt-get remove APPNAME  
#### 不保留配置文件的移除  
	$ sudo apt-get purge APPNAME  
#### 或者  
	$ sudo apt-get --purge remove  
#### 移除不再需要的被依赖的软件包
	$ sudo apt-get autoremove  
****  
## dpkg  
> 使用dpkg命令来安装以deb形式打包的软件包  

### 常用参数  
1. -i：安装指定deb包  
2. -R：后面加上目录名，用于安装该目录下的所有deb安装包  
3. -r：remove，移除某个已安装的软件包  
4. -I：显示deb包文件的信息  
5. -s：显示已安装软件的信息  
6. -S：搜索已安装的软件包  
7. -L：显示已安装软件包的目录信息  

### 解决依赖关系  
	$ sudo apt-get update  
	$ sudo apt-get -f install  

### 查看已安装软件包的安装目录  
	$ sudo dpkg -L APPNAME  
****  
# 进程(process)概念  
****  
> 动态性：进程的实质是一次程序执行的过程，有创建、撤销等状态的变化。而程序是一个静态的实体。  
> 并发性：进程可以做到在一个时间段内，有多个程序在运行中。程序只是静态的实体，所以不存在并发性。  
> 独立性：进程可以独立分配资源，独立接受调度，独立地运行。  
> 异步性：进程以不可预知的速度向前推进。  
> 结构性：进程拥有代码段、数据段、PCB（进程控制块，进程存在的唯一标志）。也正是因为有结构性，进程才可以做到独立地运行  
****  
## 线程(thread)概念    
> 是操作系统能够进行运算调度的**最小单位**。它被**包含在进程**之中，是**进程中的实际运作单位**。  
> 一条线程指的是进程中一个单一顺序的控制流，一个进程中可以并发多个线程，每条线程并行执行不同的任务。  
> 因为线程中几乎不包含系统资源，所以执行更快、更有效率。  
## 进程的属性  
### 以进程的功能与服务的对象来分  
1. 用户进程：通过执行用户内核之外的系统程序而产生的进程。  
2. 系统进程：通过执行系统内核程序而产生的进程，而且该进程的运行不受用户，即使是 root 用户的干预。  
### 以应用程序的服务类型来分  
1. 交互进程：由一个 shell 终端启动的进程，需要与用户进行交互操作，可以运行于前台、后台。  
2. 批处理进程：该进程是一个进程集合，负责按顺序启动其他的进程。  
3. 守护进程：守护进程是一直运行的一种进程，独立于控制终端并且周期性的执行某种任务或等待处理某些发生的事件。   
**** 
	shiyanlou:~/ $ ps -fxo user,ppid,pid,pgid,command
	USER      PPID   PID  PGID COMMAND
	shiyanl+ 50152 50161 50152 sshd: shiyanlou@pts/0
	shiyanl+ 50161 50162 50162  \_ -zsh
	shiyanl+ 50162 50201 50201      \_ ps -fxo user,ppid,pid,pgid,command  
1. pid：就是该进程的一个唯一编号  
2. ppid：就是该进程的父进程的pid  
3. command：表示的是该进程通过执行什么样的命令或者脚本而产生的  
****  
## 进程组  
> 每一个进程都会是一个唯一存在的进程组的成员，依靠 PGID（process group ID）来区别  
> 进程组的 PGID 等同于进程组的第一个成员的 PID，并且这样的进程称为领导进程，进程一般通过使用 getpgrp() 系统调用来寻找其所在组的 PGID，领导进程可以先终结，此时进程组依然存在，并持有相同的PGID，直到进程组中最后一个进程终结。
****  
## Sessions  
 1. Session主要是针对一个 tty 建立，Session 中的每个进程都称为一个工作(job) 
 2. Session意义在于将多个jobs囊括在一个终端，并取其中的一个job作为前台，来直接接收该终端的输入输出以及终端信号。其他jobs在后台运行。
 3. 每当一个进程被创建，它便会成为其父进程所在Session中的一员，每一个进程组都会在一个唯一存在的Session中    
 4. 进程组的 PGID 等同于进程组的第一个成员的 PID，并且这样的进程称为领导进程，进程一般通过使用 getpgrp() 系统调用来寻找其所在组的 PGID，领导进程可以先终结，此时进程组依然存在，并持有相同的PGID，直到进程组中最后一个进程终结。
****  
## 进程管理  
### &:将程序在后台运行  
### ctrl+z：停止当前工作并将其放到后台，这种可以使用`jobs`命令来查看  
### 停止并放到后台的命令，使其在后台继续运行，`bg [%jobnumber]`  
### `fg [%jobnumber]`：将后台工作移到前台  
### kill命令  
1. -1：重新读取参数运行，类似与重启（restart）    
2. -2：如同 ctrl+c 的操作退出  
3. -9：强制终止该任务  
4. -15：正常的方式终止该任务  
****  
# Linux进程管理  
****  
## top命令  
> 用来
### load average  
> load average: 0.29,0.20,0.25	分别对应1、5、15分钟内cpu的平均负载  
查看CPU个数  
	$ cat /proc/cpuinfo | grep "physical id" |sort |uniq |wc -l  
查看单CPU核心数  
	$ cat /proc/cpuinfo | grep "physical id" |grep "0" |wc -l  
****  
	shiyanlou:~/ $ cat /proc/cpuinfo | grep "physical id" |sort |uniq |wc -l
	1
	shiyanlou:~/ $ cat /proc/cpuinfo | grep "physical id" |grep "0" |wc -l
	4  

### CPU相关补充  
1. 1.0%us	用户空间进程占用CPU百分比  
2. 1.0% sy	内核空间运行占用CPU百分比  
3. 0.0%ni	用户进程空间内改变过优先级的进程占用CPU百分比  
4. 97.9%id	空闲CPU百分比  
5. 0.0%wa	等待输入输出的CPU时间百分比  
6. 0.1%hi	硬中断(Hardware IRQ)占用CPU的百分比  
7. 0.0%si	软中断(Software IRQ)占用CPU的百分比  
8. 0.0%st	(Steal time) 是 hypervisor 等虚拟服务中，虚拟 CPU 等待实际 CPU 的时间的百分比  
### 进程相关  
1. PID：进程id  
2. USER：该进程的所属用户  
3. PR：该进程执行的优先级 priority 值  
4. NI：该进程的 nice 值  
5. VIRT：该进程任务所使用的虚拟内存的总数  
6. RES：该进程所使用的物理内存数，也称之为驻留内存数  
7. SHR：该进程共享内存的大小  
8. S：该进程进程的状态: S=sleep R=running Z=zombie  
9. %CPU：该进程CPU的利用率  
10. %MEM：该进程内存的利用率  
11. TIME+：该进程活跃的总时间  
12. COMMAND：该进程运行的名字  
#### NICE值  
> 叫做静态优先级，是用户空间的一个优先级值，其取值范围是-20至19。这个值越小，表示进程”优先级”越高  
#### PR值  
> 表示Priority值叫动态优先级，是进程在内核中实际的优先级值，这个值越小，优先级越高  
#### TOP常用交互命令  
1. q：退出程序  
2. I：切换显示平均负载和启动时间的信息  
3. P：根据CPU使用百分比大小进行排序  
4. M：根据驻留内存大小进行排序  
5. i：忽略闲置和僵死的进程，这是一个开关式命令  
6. k：终止一个进程，系统提示输入 PID 及发送的信号值。一般终止进程用 15 信号，不能正常结束则使用 9 信号。安全模式下该命令被屏蔽  
****
## ps工具  
> 最常用的查看进程的工具  
****  
1. F	进程的标志（process flags），当 flags 值为 1 则表示此子程序只是 fork 但没有执行 exec，为 4 表示此程序使用超级管理员 root 权限  
2. USER：进程的拥有用户  
3. PID：进程的ID  
4. PPID：其父进程的PID  
5. SID：session的ID  
6. TPGID：前台进程组的ID。//为'-1'的表示为守护进程  
7. %CPU：进程占用的CPU百分比  
8. %MEM：占用内存的百分比  
9. NI：进程的NICE值  
10. VSZ：进程使用虚拟内存大小  
11. RSS：驻留内存中页的大小  
12. TTY：终端ID  
13. SorSTAT：进程状态  
14. WCHAN：正在等待的进程资源  
15. START：启动进程的时间  
16. TIME：进程消耗CPU的时间  
17. COMMAND：命令的名称和参数  
18. stat:表示进程状态：
> R	Running.运行中  
> S	Interruptible Sleep.等待调用  
> D	Uninterruptible Sleep.不可中断睡眠(可能是I/O出了问题)  
> T	Stoped.暂停或者跟踪状态  
> X	Dead.即将被撤销  
> Z	Zombie.僵尸进程  
> W	Paging.内存交换  
> N	优先级低的进程  
> <	优先级高的进程  
> s	进程的领导者  
> L	锁定状态  
> l	多线程状态  
> \+	前台进程    
****  
### 结合正则表达式  
	shiyanlou:~/ $ ps -fxo user,ppid,pid,pgid,command
	USER      PPID   PID  PGID COMMAND
	shiyanl+ 50152 50161 50152 sshd: shiyanlou@pts/0
	shiyanl+ 50161 50162 50162  \_ -zsh
	shiyanl+ 50162 50201 50201      \_ ps -fxo user,ppid,pid,pgid,command  
****  
	shiyanlou:~/ $ ps aux | grep zsh
	shiyanl+   148  0.2  0.0  45760  5916 pts/0    Ss   18:25   0:00 -zsh
	shiyanl+   219  0.0  0.0  15384   932 pts/0    S+   18:30   0:00 grep --color=auto --exclud
	e-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn zsh  
****  
	$ ps aux
	//查看所有进程  
****  
	$ ps axjf  
	//查看时，将连同部分的进程呈树状显示出来  
****  
## pstree命令  
	shiyanlou:~/ $ pstree
	init.sh─┬─Thunar
	        ├─Xvnc─┬─{llvmpipe-0}
	        │      ├─{llvmpipe-1}
	        │      ├─{llvmpipe-2}
	        │      └─{llvmpipe-3}
	...  
> -A:各程序树之间以 ASCII 字元來連接  
> -p:同时列出每个 process 的 PID  
> -u:同时列出每个 process 的所屬账户名称  
****  
	shiyanlou:~/ $ pstree -up
	init.sh(1)─┬─Thunar(81,shiyanlou)
	           ├─Xvnc(29,shiyanlou)─┬─{llvmpipe-0}(39)
	           │                    ├─{llvmpipe-1}(40)
	           │                    ├─{llvmpipe-2}(41)
	           │                    └─{llvmpipe-3}(42)
	...  
****  
## 进程的执行顺序  
通过nice值和PR值来实现调整进程调度的优先级  
### `nice`命令和`renice`命令  
	$ nice -n -5 vim &  
	$ renice -5 pid  
	//修改已存在进程的优先级  
****  
## 小结  
1. 了解了常用的软件操作命令，安装更新和移除，以及一些之前没有注意到的关于软件间的依赖性的问题  
2. 重新明确了程序、进程、线程的关系，简言之：一个程序至少有一个进程,一个进程至少有一个线程  
3. 进程组和Session的概念讲的十分笼统，这部分会在接下来的学习中，有意识的进行注意和比较以加深理解 
4. 学习了top、ps和pstree等命令，以及使用如何这些命令获取需要的信息，同时我们学会了进程的管理命令 kill，nice，renice，这些命令在实际使用Linux时是十分常用且方便的   
