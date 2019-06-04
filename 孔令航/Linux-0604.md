# 补充-awk文本处理语言初步  
****  
> [AWK](https://awk.readthedocs.io/en/latest/chapter-one.html)是一种用于处理文本的编程语言工具  
### 一般格式  
	$ pattern {action}  
1. pattern通常是表示用于匹配输入的文本的“关系式”或“正则表达式”  
2. action则是表示匹配后将执行的动作  

## 命令基本格式    
	awk [-F fs] [-v var=value] [-f prog-file | 'program text'] [file...]  
1. -F参数用于预先指定前面提到的字段分隔符（还有其他指定字段的方式）  
2. -v用于预先为awk程序指定变量  
3. -f参数用于指定awk命令要执行的程序文件，或者在不加-f参数的情况下直接将程序语句放在这里  

	shiyanlou:~/ $ awk '{print}' test
	I like Linux
	www.shiyanlou.com

> awk处理文本的方式，是将文本分割成一些“字段”，然后再对这些字段进行处理，默认情况下，awk以空格作为一个字段的分割符  
### 常用内置变量  
1. FILENAME：当前输入文件名，若有多个文件，则只表示第一个。如果输入是来自标准输入，则为空字符串  
2. $0：当前记录的内容  
3. $N：N表示字段号，最大值为NF变量的值  
4. FS：字段分隔符，由正则表达式表示，默认为" "空格  
5. RS：输入记录分隔符，默认为"\n"，即一行为一个记录  
6. NF：当前记录字段数  
7. NR：已经读入的记录数  
8. FNR：当前输入文件的记录数，请注意它与NR的区别  
9. OFS：输出字段分隔符，默认为" "空格  
10. ORS：输出记录分隔符，默认为"\n"  
****  
# 挑战：数据提取  
****  
## 在文件中匹配数字开头的行，将所有以数字开头的行都写入文件  
	shiyanlou:~/ $ cat data2 | sort -n | egrep '^[1-9]' > /home/shiyanlou/num  
	shiyanlou:~/ $ less num                                                  
	2133131@qq.com
	129512421@shiyanlou.com
	213399394@qq.com
	542341312@qq.com
	1231333123
	1232134123
	3312313213@qq.com
	4123512313
	11111111111
	22222222222  

	shiyanlou:~/ $ grep '^[1-9]' /home/shiyanlou/data2> /home/shiyanlou/num  
## 在文件中匹配出正确格式的邮箱，将所有的邮箱写入文件，注意该文件中每行为一个邮箱  
	shiyanlou:~/ $ cat data2 | sort -n | egrep '*@*\.com' > /home/shiyanlou/mail
	shiyanlou:~/ $ less mail                                                    
	chenghaoyang@gmail.com
	testfile@163.com
	2133131@qq.com
	129512421@shiyanlou.com
	213399394@qq.com
	542341312@qq.com
	3312313213@qq.com  
	shiyanlou:~/ $ egrep '^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(.[a-zA-Z0-9_-])+$' /home/shiyanlou/data2> /home/shiyanlou/mail  
## 小结  
1. 具有多解性的问题，有许多解决方案  
2. 多思考比较，能够提高实际中的操作效率    
3. 在匹配邮箱格式时，仅使用了'*@*\.com'的判断语句，考虑并不周道      
****  
