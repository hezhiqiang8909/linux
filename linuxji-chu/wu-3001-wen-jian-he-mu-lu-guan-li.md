###一、Linux的文件

	Linux系统中文件或目录的属性主要包括：文件或目录的索引节点(inode)、类型、权限属性、链接数、所归属的用户和用户组、最近修改时间等内容：
	
**执行 ls -lhi命令的结果：**

	total 4.0K
	24563    drwxr-xr-x        2      root   root 4.0K   Jun 28 21:30   123
 	6464     -rw-r--r-- 	   1      root   root    0   Jun 28 21:37   file
	inode   文件类型及权限 硬链接数   用户   组     大小  最近修改时间  文件名


**文件的修改、访问、创建的时间查看:**


	[root@localhost linux-study]# stat file
	  File: `file'
	  Size: 5               Blocks: 8          IO Block: 4096   regular file
	Device: 803h/2051d      Inode: 6464        Links: 1
	Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
	Access: 2015-06-28 21:37:27.652640852 +0800
	Modify: 2015-06-28 21:43:01.275627486 +0800
	Change: 2015-06-28 21:43:01.275627486 +0800
		

###二、索引节点inode
	
	inode概述：inode中文意思是索引节点。每个存储设备或存储设备的分区（存储设备可以是硬盘、软盘、U盘...)被格式化为文件系统后，应该后又两个部分：一部分是inode,另一部分是Block。Block是用来存储数据用的。而inode就是用来不存储这些数据信息的，这些信息包括文件大小、属主、归属的用户组、读写权限等。
	inode为每个文件进行信息索引，所以就有了inode的数值。操作系统根据指令，能通过inode值最快的找到相对应的文件。
	打个比方，比如一本书，存储设备或分区就相当于这本书，Block相当于书中的每一页，inode就相当于这本书的前面的目录，一本书有很多内容，如果想查看某个部分的内容，我们就可以先查看目录，通过目录能更快的找到我们想要看到的内容。
	
	虽然不恰当，但是很形象。
	ls -i 我们就可以查看到inode节点号了。
	inode值相同的文件是硬链接文件

	
###三、Linux文件的权限

**1、文件权限概述：**

	Linux中的文件或目录的权限和用户与用户组关联很大，要理解这部分内容，需要先了解一下linux系统中用户管理方面的知识。
	每个文件或目录都有一组共9个权限位。没三位被分为一组，他们分别是属主权限位（占三个字符）、用户组权限位（占三个字符）、其它用户权限位（占三个字符）。比如rwx-rxr-x.在Linux中正是9个权限位（更多权限位后面会提到）来控制文件属主、用户组以及其他用户的权限

- **权限位说明：**


	Linux文件或目录的权限是由9个权限位来控制，每三位位一组，他们分别是文件属主（Ower）的读、写、执行，用户组的（Group）的读、写、执行以及其他用户（Other）的读、写、执行；
	文件属主：读R、写W、执行X
	用户 组：读R、写W、执行X
	其他用户：读R、写W、执行X
	
	如果权限位不可读、不可写、不可执行，则用-来表示。
- **Linux 普通文件的读、写、执行权限说明：**


	可读R：表示具有阅读文件内容的权限。
	可写W：表示具有新增、修改文件内容的权限：（特别提示:删除或修改的权限受父目录的权限控制）；
	可执行X:表示具有执行文件的权限。
- **Linux 目录的读、写、执行权限说明：**


	进入目录的的权限 X
	浏览目录的权限 R
	修改目录内文件的权限 W

- **Linux  文件与目录权限对比说明：**


    r(Read,读权限)
	对文件而言，表示具有阅读文件内容的权限；
	读目录来说，表示具有浏览目录的权限（注意：与进入目录的权限不同）
    w(Write,写权限）
	对于文件来说，表示具有新增、修改文件内容的权限（注意，删除和移动与文件本身属性无关）
	对目录来说，表示具有删除，移动目录内文件的权限。	
    x(Execute,执行权限）
	对文件而言，表示具有执行的权限。
	对目录而言，表示具有进入目录的权限。
    -（无任何权限）
	若对应位置权限位为字符“-”，表示对应用户没有读、写、执行的任何权限。

**特别注意：**
	
	当删除或移动一个文件或目录，仅与该文件目录所在的上一层目录权限有关，与该文件本身属性无任何关系。对于文件来说，写文件是修改文件，而不是删除文件，因此文件是与该文件的本身属性有关系的。


**2、改变权限属性命令chmod**

	chmod是用来改变文件或目录的权限的命令，但只有文件的属主和超级用户root才有这种权限。通过chmod来改变文件或目录的权限有两种方法:一种是通过权限字木和操作符表达式的方法来设置权限；另外一种是使用数字的方式来设置权限。
	例如：
		chmod 755 file
		chmow u+x,go-x file
- **chmod 数字权限方法：**

	chmod [数字组合] 文件名
	chmod的数字语法简单直观： r---4	 w---2  x---1 "-"---0	
	属主的权限位三个权限位的数字加起来的总和。（归属组个其他权限算法一样）
- **chmod字符式权限表示法：**

		
	chmod [用户类型] [+ - =] [权限字符] 文件名
	用户类型：u表示主，g表示属主，o表示其他用户，a表示所有
	权限定义字母：r代表读权限 ，w代表写权限,x代表执行权限
	权限增减字符： +添加某个权限，-取消某个权限，=赋予制定的权限
- **示例技巧：**	

		
	u=r+x 为文件属主添加读写权限；
	ug=rwx,o=r 为属主和组添加读、写、执行权限，为其他用户设置读权限。
	a+x为所有人添加执行权限
	g=u 让用户的属组和属主的权限相同；
	对于目录权限设置，要用到-R参数。
- **默认权限分配的命令umask:**
	
	umask是通过八进制的数值来定义用户创建文件或目录的默认权限。umask表示的是禁止的权限。具体的细节，文件和目录略有不同。

	
- **重要总结：**


	对于文件来说，umask的设置是在假定文件拥有八进制666权限上进行，文件的权限就是666 减去umask的掩码数值
	对于目录来说，umask的设置是在嘉定文件拥有八进制777权限上进行，目录八进制权限777减去umask的掩码数值
 	默认 文件与 目录的权限算法
	666  ==>文件的起始权限值     777 ==>目录的起始权限值
	022- ==>umask的值	     022- ==>umask的值
	\----			     ---
	644			     755

**例如：**	

	1、umask 044 && touch file1 && mkdir test && ll -al 验证
	2、linux系统用户的家目录的权限是通过在配置文件中指定的，比如Centos中用的是/etc/login.defs文件
	其中有个权限umask 为077。当添加用户的时，系统在/home中创建用户的家目录，并且设置它的权限为777-077=700，也就是说权限位rwx------.
	3、umask 一般都是放在用户相关的SHELL的配置文件中，比如用户家目录下的.bashrc或.profile，也可以放在全局性的用户配置文件中，比如/etc/login.defs,还可以放在SHELL全局的配置文件中，比如/etc/profile或/etc/bashrc等。
	4、umask放在相关的配置文件中，目的是当管理员创建用户时，系统会自动为用户创建文件或目录时配置默认的权限代码。
	特别提示：在一般的生产场景，umask的使用不多见。
	


###四、Linux特殊权限

**1、setuid和setgid位**

	特别提示：在一般的生产环境，运维人员使用setuid，setgid的情况不多见，也不推荐大家使用（setuid，sitgid本身功能不错，但是会带来安全隐患）
	介绍：
	setuid和setgid位是让普通用户可以以root用户的角色运行只有root账号才能运行的程序和命令。(注意区分su 及sudo的区别)	
	在Linux中，有时执行某一个命令的时，需要对另一个文件进行操作，而这个文件有不是普通用户有权限进行操作。例如，修改用户密码的命令passwd,实际最终修改的是/etc/passwd文件，该文件的所有者和组都是root,同组用户和其他用户具有执行权限只有个root权限的用户才能更改，但普通用户也可以使用该命令修改自己的密码

	[root@localhost ~]# ls -l /etc/passwd
	-rw-r--r--. 1 root root 904 Jun 16 23:10 /etc/passwd

	如果普通用户通过修改/etc/passwd修改自己的口令肯定是不可完成的任务。作为普通用户可以通过passwd来修改自己的口令，既然没有权限，为什么还能修改密码呢？
	让我们来看看passwd命令的权限：
	[root@localhost ~]# ls -l /usr/bin/passwd 
	-rwsr-xr-x. 1 root root 30768 Feb 22  2012 /usr/bin/passwd
	因为/usr/bin/passwd文件已经设置了setuid权限位（也就是rwsr-xr-x种的s),所以普通用户在执行/usr/bin/passwd命令时可以使用root用户的权限，间接地修改/etc/passwd。以达到修改自己口令的权限
	我们知道Linux的用户管理是极为严格的，不同的用户拥有不同的权限，为了完成只有root用户才能完成的工作，我们必须为普通用户提升权限，最常见的方法就是su或sudo.虽然setuid和setgid也是让普通用户超越自身拥有的普通权限达到使用root权限的方法，但不推荐大家使用，因为它能为系统带来安全隐患！
	特别注意：setuid和setgid会面临风险，所以尽可能的少用（尤其是使用不当）

**实例：**

	我们想让一个普通用户longge拥有root用户才拥有的rm删除权限，我们除了用su或sudo临时切换到root身份或以root身份操作之外。还可以怎么使用呢
	chmod 4755 /bin/rm
	我们就可以轻松删除。

**2、setuid和setgid设置说明：**

	特殊权限位数字权限（八进制)方法
	setuid位的设置是用4000，setgid占用的是八进制的2000；	
	chmod 4775  命令  chmod 2755 文件名

	特别说明：我们见过的是小写的s;表明文件归属的用户组位有执行权限X.因为我们用了2755,意思是说文件属组拥有可读可写可执行权限，所归属的用户组拥有可读可执行权限，并设置了setuid，所以这是本来文件所归属的用户组拥有了r-x，现在加了setgid位，就把其中的x换成了s。如果文件所归属的用户组没有执行权限，这个权限应该是S.同理setuid位中的大写的S和小写的s，也是这个原理。
	如果本来在该位上有x，则这些特殊标志显示为小写字母(s,s,t)、否则为大写字母(S,S,T)
	
- 特殊权限字符式语法：

	还是用chmod的字符式语法，通过u+s 或u-s来增减setuid位，同理，我们可以通过g+s或g-s来设置setgid位
	file命令也可以用来查看setuid和setgid位，当然也可以查看文件的类型。


- 粘贴位及设置方法：
	
	粘贴位的理解，我们还是先看一个例子
	[root@localhost ~]# ls -ld /tmp/
	drwxrwxrwt. 4 root root 4096 Jun 28 21:26 /tmp/
	
	我们看到/tmp权限最后一个字母是t.这就设置了粘贴位。
	粘贴位的设置，用八进制的1000来表示。
	一个目录即使设置了rwxrwxrwx的权限，如果是设置了粘贴位，除非目录的属主和root用户有权限删除它，除此之外其他用户都不能删除这个目录。用途一般是把一个文件的权限都打开，然后来共享文件，像/tmp目录一样。方便带来安全隐患，生产环境一般不使用。
	
- 文件或目录的归属关系：
	
	文件或目录的归属关系主要定义文件或目录归属那个用户所有及归属于那个用户组所有


- 改变文件所属关系命令chown
	
	当我们要改变一个文件的属组，我们所使用的用户必须是该文件的属主而且同时是木匾属组成员，或超级用户，只有超级用户才有改变文件的属主。

**3、chown语法：**

	chown [选项]  [所有者][:[组]]	文件
	说明： chown所接的新的属组和信的属组之间应该以.或：连接，属主和属组任意一个可以为空。如果属主位空，应该是：属组；如果属组位空，那就不需要. 或：。

	改变文件的属组命令chgrp
**4、chgrp 语法：**

	chgrp [参数选项]  组 文件

	它的用法和chown类似，只不过他仅仅是用来改变文件或目录的属组的，-R参数用于目录及目录一下搜友文件改变属组的。和chown也是一样的，可以吧chgrp看做是chown的一个子集。一般会用chown就可以了。

	[root@localhost /]# ls /home/longge/file  -l
	-rw-rw-r-- 1 500 500 0 Jun 29 00:06 /home/longge/file
	
	上面的理智，为什么属主和属组都是一个数值。出现这种情况的原因是系统不存在与之对应的用户，所以只能以数字形式显示了。有时我们删除了用户，但没有删除其他家目录，这种情况下，它的家目录的属主和属组也会变成数字；
	提示：删除用户的时候可以执行userdel-r longge 连同家目录一起删除。
	
**文件被修改或被访问的时间：**
	
	我们通过查看文件的属性时，会发现他的时间标记。这个时间并不代表文件被创建的时候，他代表文件被访问或被修改的时间，文件被修改的时间比较好理解，比如我们可以用编辑器来修改文本文件，然后保存一下，这样文件的时间就变了。
	当然也有其他的工具不修改文件的内容，只修改文件的时间，这是可以被称为访问时间。比如touch工具能达到这个目的。

	
**文件属性和文件系统属性的关系：**
	
	文件系统的特性决定文件属性的定义和修该，比如我们通过chattr来锁定一个文件为不可修改或不可删除是，要用到chattr的+i参数；这在ext2和ext3的文件系统是有效的，但是reiserfs文件系统是没有任何效果的
	chattr +i file  
	文件就无法被删除了。

