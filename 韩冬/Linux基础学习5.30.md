# 5.30学习总结

## 学习心得



今天学习了关于顺序执行、选择执行、管道、cut 命令、grep 命令、wc 命令、sort 命令等，高效率使用 Linux 的技巧，简单而使用，引用学习可见的一句话“Linux/UNIX 哲学吸引人的地方，大繁至简，一个命令只干一件事却能干到最好”















## 学习笔记



### 命令执行顺序控制与管道

**顺序执行、选择执行、管道、cut 命令、grep 命令、wc 命令、sort 命令等，高效率使用 Linux 的技巧。**

![1559220550438](C:\Users\韩冬\AppData\Roaming\Typora\typora-user-images\1559220550438.png)

**管道**

管道是一种通信机制，通常用于进程间的通信（也可通过socket进行网络通信），它表现出来的形式就是将前面每一个进程的输出(stdout)直接作为下一个进程的输入(stdin)。

**管道又分为匿名管道和具名管道**

匿名管道，在命令行中由`|`分隔符表示

通常只会在源程序中用到具名管道

![1559221748353](C:\Users\韩冬\AppData\Roaming\Typora\typora-user-images\1559221748353.png)

![1559221805326](C:\Users\韩冬\AppData\Roaming\Typora\typora-user-images\1559221805326.png)

**3.3 grep 命令，在文本中或 stdin 中查找匹配字符串**

`grep`命令：它结合正则表达式可以实现很复杂却很高效的匹配和查找

`grep`命令的一般形式为：

```
grep [命令选项]... 用于匹配的表达式 [文件]...
```

`-r` 参数表示递归搜索子目录中的文件,`-n`表示打印匹配项行号，`-I`表示忽略二进制文件

也可以在匹配字段中使用正则表达式

![1559222310161](C:\Users\韩冬\AppData\Roaming\Typora\typora-user-images\1559222310161.png)

3.4 wc 命令，简单小巧的计数工具     

wc 命令用于统计并输出一个文件中行、单词和字节的数目，比如输出`/etc/passwd`文件的统计信息

![1559222716069](C:\Users\韩冬\AppData\Roaming\Typora\typora-user-images\1559222716069.png)

 **sort 排序命令**     

功能很简单就是将输入按照一定方式排序，然后再输出,它支持的排序有按字典排序,数字排序，按月份排序，随机排序，反转排序，指定特定字段进行排序等等。

默认为字典排序：

 

```
$ cat /etc/passwd | sort
```

反转排序：

 

```
$ cat /etc/passwd | sort -r
```

按特定字段排序：

 

```
$ cat /etc/passwd | sort -t':' -k 3
```

上面的`-t`参数用于指定字段的分隔符，这里是以":"作为分隔符；`-k 字段号`用于指定对哪一个字段进行排序。这里`/etc/passwd`文件的第三个字段为数字，默认情况下是以字典序排序的，如果要按照数字排序就要加上`-n`参数

![1559222871862](C:\Users\韩冬\AppData\Roaming\Typora\typora-user-images\1559222871862.png)

  **uniq 去重命令**     

- 输出重复行

 

```
# 输出重复过的行（重复的只输出一个）及重复次数
$ history | cut -c 8- | cut -d ' ' -f 1 | sort | uniq -dc
# 输出所有重复的行
$ history | cut -c 8- | cut -d ' ' -f 1 | sort | uniq -D
```

 `istory`命令查看最近执行过的命令（实际为读取${SHELL}_history文件,如我们环境中的~/.zsh_history文件），不过你可能只想查看使用了哪个命令而不需要知道具体干了什么，那么你可能就会要想去掉命令后面的参数然后去掉重复的命令

**因为uniq命令只能去连续重复的行，不是全文去重**

![1559223215226](C:\Users\韩冬\AppData\Roaming\Typora\typora-user-images\1559223215226.png)
