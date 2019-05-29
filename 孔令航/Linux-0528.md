# 文件系统操作与磁盘管理  
**** 
## df命令  
> 查看磁盘容量  
  
	shiyanlou:~/ $ df -h
	文件系统                                                                                   
        容量  已用  可用 已用% 挂载点
	/dev/mapper/docker-202:33-524289-5c5ecad2b9a2846caae0b159047ff0b08589f0648fc9546241741063a9
	7f6ddd   12G  6.5G  4.7G   59% /
	tmpfs                                                                                      
         64M     0   64M    0% /dev
	tmpfs                                                                                      
        3.9G     0  3.9G    0% /sys/fs/cgroup
	/dev/xvdc1                                                                                 
        296G  193G   88G   69% /etc/hosts
	shm                                                                                        
         64M   84K   64M    1% /dev/shm
	tmpfs                                                                                      
        3.9G     0  3.9G    0% /proc/acpi
	tmpfs                                                                                      
        3.9G     0  3.9G    0% /proc/scsi
	tmpfs                                                                                      
        3.9G     0  3.9G    0% /sys/firmware
**** 
## du命令  
> 查看目录容量  

	shiyanlou:~/ $ du -h  
	...  
	8.0K./.dbus/session-bus
	12K./.dbus
	4.0K./.gconf
	28K./.vnc
	8.0K./.pip
	32K./Desktop
	92M.  

- -a_(all)_显示目录中所有文件的大小  
- -h_(human_readable)_以K,M,G为单位，提高可读性  
- -s_(summarize)_显示总计,仅列出最后总的值  

	shiyanlou:~/ $ du -s  
	93500.  
	shiyanlou:~/ $ du -sh  
	92M.  
****  
## dd指令  
> 转换和复制文件（创建虚拟磁盘）  
	shiyanlou:~/ $ dd if=/dev/stdin of=vertual.img bs=1M count=256[15:25:45]
	记录了0+0 的读入
	记录了0+0 的写出
	0 bytes copied, 2.97814 s, 0.0 kB/s
- 并没有实现给出例子的效果  
****  
## mkfs指令  
> 格式化磁盘  
	shiyanlou:~/ $ sudo mkfs.ext4 virtual.img [15:28:05]
	mke2fs 1.42.13 (17-May-2015)
	mkfs.ext4: Device size reported to be zero.  Invalid partition specified, or
	partition table wasn't reread after running fdisk, due to
	a modified partition being busy and in use.  You may need to reboot
	to re-read your partition table.  
****  
## mount指令  
> 挂载磁盘到目录树  
> 通常用于USB或其他移动存储设备  

	mount [-o 操作选项] [-t 文件系统类型] [-w|--rw|--ro] [文件系统源] [挂载点]  

- 查看主机下已经挂载的文件系统  

	shiyanlou:~/ $ sudo mount
	/dev/mapper/docker-202:33-524289-5c5ecad2b9a2846caae0b159047ff0b08589f0648fc9546241741063a9
	7f6ddd on / type ext4 (rw,relatime,stripe=16,data=ordered)
	proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
	tmpfs on /dev type tmpfs (rw,nosuid,size=65536k,mode=755)  

****  
## unmount指令  
> 卸载已挂载磁盘  
	shiyanlou:~/ $ sudo unmount /mnt  
## fdisk指令  
> 为磁盘分区  

- 查看磁盘分区表信息  

	shiyanlou:~/ $ sudo fdisk -l  

****  
##小结  
- 实验楼系统中许多操作无法直接测试  
- Linux系统下与硬件相关的部分知识还需要在今后进一步强化学习  
- 近期会加快Linux部分的学习进度    
