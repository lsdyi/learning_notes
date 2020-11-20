### 虚拟机的网络连接三种方式的说明

#### 桥式连接：可能造成ip冲突

虚拟机和母机共享同一个ip地址。虽然很方便，但是可能出现ip地址不够用的情况

#### NAT模式：不会造成ip冲突

母机上有两个ip地址，一个是正常的网络环境下的ip地址，另一个ip地址的字段和虚拟机的ip地址的字段相同，用于和虚拟里之间的通信。

#### 主机模式：虚拟机不能访问外网

虚拟机有一个独立的字段的ip，而主机只有一个主机所在网络环境下的ip地址，主机和虚拟机之间不能通信，因此不能访问外网。



### 常见的目录结构

#### /bin: 

binary的缩写，这个目录存放着最经常使用的命令

#### /sbin:

s是super user，这里存放的是系统管理员使用的系统管理程序

#### /home:

普通用户的主目录，类似于root用户的root目录

#### /root

#### /lib：

库文件，其作用是windows里的dll文件

#### /etc:

里面放的是程序的一些配置文件

#### /usr:

这是非常重要的目录，类似于windows下的program files，里面装的是各种软件

#### /opt:

相当与是放安装包的



### vi和vim

在正常模式下：

yy复制当前行，

p粘到当前行的下一行

dd删除当前行

在命令行模式下输入/string 即可查找字符串string，然后会让文中的所有string高亮显示，如果要失去高亮的话那也简单就是输入命令nohl，我猜是no highlight 不高亮的意思惹

命令set nu和set nonu分别是显示行号和不显示行号

在正常模式下输gg光标到第一行，输G光标到最后一行，输入一个行号，然后再按shift+g就可以在正常模式定位下将光标定位到该行号，这样再进入插入模式的时候就可以直接编辑

##### 当在命令行模式下输完了一条命令之后，就会直接进入正常模式这个是真的，如何看当前是在命令行模式还是正常模式还是插入模式，如果在编辑器的右下角有显示number,number这是表示当前光标在哪一行哪一列，所以现在是正常模式，可以对文本行进行一系列的快捷操作。如果是左下角有insert提示，这说明当前是在插入模式，现在可以编辑文字。

##### 正常模式就是命令行模式和插入模式之间的桥梁，不可能直接从插入模式进入命令行模式，也不可能直接从命令行模式进入插入模式

如果vim的一个文件所在的文件夹根本就木有创建的话 那么vim编辑完成之后会无法保存 报 can't open file for writing



### 用户的登录和注销


##### logout指令在图形化的终端中是无效的
```
增加用户 useradd -d (directory ) (username)
```
```
删除用户,递归删除家目录  userdel -r (username)
```
```
查询 id (username)
```
```
切换用户 su - (username)  
```
```
修改密码 passwd (username)
```
这是不是一个攻击的方式 因为在su命令的时候 它有缓存先前的登录记录而不用输密码

##### ping好像只能ping外网，curl是外网内网都是curl哈哈哈哈哈哈哈哈哈哈



### 用户组

```
用户组的创建 groupadd (groupname)
```

##### 用户组的删除 groupdel (groupname)

##### 创建用户的时候指定用户组 如果不指定用户组的话 系统会默认帮我们创建一个和用户名同名的用户组 然后把用户组放在里面  因此创建用户的时候指定用户组的写法为 useradd -g (groupname) -d (directory) (username)

##### 为用户重新分配用户组 usermod -g (groupname) (username)

#### 用户 和 用户组相关的文件

用户信息在 /ect/passwd
组信息在 /etc/group 
密码信息在 /etc/shadow 但是密码加密了 看不懂额

### 运行级别 
0：关机
1：单用户 
2：多用户无网络
3：多用户有网络 （最常用）
4：保留
5：图形界面
6：重启
#### 用户的运行级别的设置文件在/etc/inittab文件中指定
``` 
指定当前运行级别 init (1or 2 or 3 or 5 or 6)
```

### 帮助指令
```
获得帮助信息 man (command or filename)
```

### 文件 和 目录的指令
```
显示当前工作目录的绝对路径 pwd
直接回到家目录 cd ~
创建多级目录 mkdir (directory) -p      //加上-p的选项就行

创建空文件 touch (filename)	

拷贝文件到文件夹 cp (source) (destination)     //如果destination目录不存在的话 									那么会创建一个名为destination的文件 和							source文件完全相同 多可怕55 所以还是要创建目标文件夹先啊
递归拷贝文件夹 cp (source) (destination) -r
递归拷贝文件夹 不提示是否覆盖文件 \cp (source) (destination) -r

重命名文件或目录 mv old new
如果new是一个目录的话则是移动到那个目录 
但是如果new是一个文件的话那就是把内容移动到那个文件中
总结过程：读 写 删

浏览文件 但是不能修改 cat (filename) 
	选项：-n 显示行号
		 | more 分页显示
		 
分屏查看文件 less (filename)  它不是一次性加载所有文件内容的 所以打开的速度非常快 如果文件很大的时候 使用这种会比较合适

重定向 >
追加重定向 >>

echo 回音嘻嘻嘻 
echo $PATH 输出环境变量
	选项： -n 不换行

head (filename) 显示文件的头部内容
	选项： -n (lines)
	
tail (filename) 显示文件的尾部内容
	选项： -n (lines)
		  -f 实时监控文件嘻嘻嘻
		  
ln -s (orginalfile) (link) 设置软链接 意思就是创建了一个目录 但是这个目录和一般的目录还不同 它和你的目标目录有着一一映射的关系 对文件的任何改变都是双向生效的
	
history (commandnumber)	显示执行过的历史指令


```
#### Now think  how many ways can you copy a file 

### 时间 日期 相关指令
```
date "+(parameters)" 查看时间日期
date -s "(parameters)" 设置时间日期

显示当前日历 cal
```

### 搜索 查找 相关指令
```
find (scope starting to find)
	选项：
		-name (filename)
		-user (username)
		-size (requirements about the file size)

updatedb 更新locate数据库 用locate进行查找的第一步
locate (filename) 用locate进行查找的第二步也是最后一步

grep命令一般和cat命令搭配着使用 用cat获得内容 再用管道传递给grep命令
cat (filename) |grep (keywords)
	grep后面的选项：
		-i keywords不区分大小写
		-n 显示行号
```

### 压缩 解压缩 相关指令
```
gzip (filename) 将名为filename的文件进行压缩 只能将文件压缩成 *.gz文件
gunzip (zipname.gz) 将zipname.gz文件解压缩
```
##### 上面的两个指令都是做的一般文件和压缩文件之间的转换 就是要么让一般的文件变成压缩文件 要么让一个压缩文件变成一个一般的文件

```
zip (goalname) (zippedfile)
	选项：
		-r 递归压缩 也就是压缩文件夹
```

#### 最常用的在下面
```
tar 
	选项：
		-zcvf (zipname) (filenames to be zipped) 打包并压缩
		-zxvf (zipname) -C (destination directory) 解压到指定目录 C必须是大写 且 到指定的目录必须是存在的
		
```

### 组管理和权限管理
```
chown (username) (filename) // change owner 改变文件所有者
chgrp (groupname) (filename) // change group 改变文件的所在组

```
#### 文件类型
##### - 普通文件
##### d 目录
##### l 软链接
##### c 字符设备 （键盘 鼠标）
##### b 块文件 （硬盘）

```
文件记录的第一个属性是表示文件的类型 如上
第二个属性是分别表示文件所有者 文件所在组和其他组 这三个部分的读写权限
第三个属性是 如果是文件 表示硬链接的数 如果是目录 表示子目录的个数
```



输入vi命令是进入一个临时缓冲区，如果该文件存在，则将内容拷贝到临时缓冲区



命令模式下：

dd 删除一行

3dd 连续删除3行



cc 修改一行

3cc 连续修改三行



yy 拷贝光标所在行至缓冲区

p 把缓冲区的文字复制到光标之处



. 重复上一个命令

u 撤销上次的命令



末行命令主要有以下几类：

字符串搜索命令

/

全局替换命令

:/p1/p2 将当前行中的第一个p1用p2替换 后面加g是当前行所有 c是要询问

文件操作命令

其他命令

:n 调至第n行

:n1,n2 co n3 把n1行到n2行之间的内容移动到n3行下



:!command 执行shell命令



自动制作工具： make 自动执行脚本 

调试工具： gdb



预处理：条件编译 宏替换 执行文件包含

编译： 预处理后的源代码编译成汇编代码 进行词法，语法和语义分析

汇编：汇编器将汇编代码转化为机器语言

连接：解析目标代码的外部引用，将多个目标代码文件连接为一个可执行文件



gcc -I (directory)