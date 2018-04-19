Linux如何修改文件和文件夹的权限?

	修改属主：   chown [-R] 新属主  文件
				-R： 将文件夹中的所有文件也更改掉
	修改属组：   charp  [-R]  先属组  文件
	修改权限：   chmod [-R]  新权限  文件
	linux特有功能（ext2 ext3所体现的）：
	列出所有权限命令：lsattr
	设置特殊权限：    chattr
	新权限有两种表示方法：
	（1）    直观：  如  rw-r—r—
	数字        6   4  4
	110100100
	（2）字母法：  如  rw-r–r–
	u  g  o
	修改文件的访问权限不必非得是root，文件属主也可以

init进程作用是什么？
	
	init进程是所有进程的发起者和控制者
	init是第一个运行的进程
	init的进程编号永远是1
	如果init出现问题，系统随之垮掉
	init的角色：终极父进程，失去父进程的子进程以init作为它们的父进程
	特定的运行级别是运行相应的程序
	init读取配置文件/etc/inittab，决定启动运行的级别

linux开机引导步骤是什么？
	
	加载内核
	内核就必须完整地加载到可用RAM的第一个兆字节之内。为了实现这个目的，内核是被压缩了的。
	这个文件的头部包含着必要的代码，先设置CPU进入安全模式（以此解除内存限制）再对内核的剩余部分进行解压缩。
	执行内核
	内核在内存中解压缩之后，它就可以开始运行了
	一旦内核启动运行，对硬件的检测就会决定需要对哪些设备驱动程序进行初始化。
	内核就能够挂装根文件系统，内核挂装了根文件系统之后，
	启动并运行一个叫做init的程序。

ext2和ext3的区别是什么？

	ext2是使用最普遍的linux filesystem
	ext3基于ext2
	都是磁盘结构
	ext2可以转换成ext3
	ext3可以挂载在ext2文件系统

介绍一下linux文件系统：
	（1）/bin：该目录用于存放用户命令。 目录 /usr/bin 中也存放了一些用户命令。
	（2）/sbin：该目录用于存放许多系统命令，例如 shutdown。目录 /usr/bin 中也包括了许多系统命令。
	（3）/root：该目录用于存放根用户（超级用户）的主目录
	（4）/mnt：该目录主要用于存放系统引导后被挂载的文件系统的挂载点。
	（5）/boot：该目录用于存放内核和其它系统启动时使用的文件。
	（6）/lost+found：该目录被fsck用于存放零散文件（没有名称的文件）
	（7）/lib：该目录用于存放被 /bin 和 /sbin 中的程序使用的库文件。 目录 /usr/bin 中含有更多库文件。
	（8）/dev：该目录用于存放设备文件。
	（9）/etc：该目录用于存放许多配置文件和目录
	（10）/var：该目录用于存放系统中不断扩充、变化的文件，例如日志文件和锁定文件。
	（11）/usr：该目录用于存放与系统用户直接有关的文件和目录。
	（12）/proc：该目录是一个虚拟的文件系统（不是实际贮存在磁盘上的），它包括被某些程序使用的系统信息
	（13）/initrd：该目录用于存放在计算机启动时挂载 initrd.img 映像文件的目录以及载入所需的设备模块。
	（14）/tmp：该目录用于存放用户程序运行时所产生或保存的一些临时文件。 /tmp 有全局读写权。
	（15）/home：该目录用于存放用户主目录的位置

Linux上比较文件的命令都有哪些?
	
	cmp命令
	cmp [options] file1 file2
	比较两个文件，给出差别字符的位置和行号。同时可以设置选项使得cmp给出结果时同时显示差别字符。
	-c 显示第一个差别字符
	-l 以十进制显示差别字符的位置，并以八进制显示其数值
	
	diff 命令
	diff [options] file1 file2
	普通输出格式：
	仅按序显示差别行
	上下文输出格式：-C
	以一些行作为上下文（上下文hunk）来显示差别行，以便用户更清楚地知道所比较文件的差别。
	统一输出格式：-U
	修改了上下文格式，取消了重复的上下文并简化了输出。
	
	diff3 命令
	两个人同时修改了一个公用文件的情况下，使用diff3命令，可以比较两个文件对一个源文件的修改，并把结果合并在一个输出文件中，用以指出两个文件对源文件所作的修改的冲突之处。
	diff3 [options] myfile oldfile yourfile

介绍一下Make? 为什么使用make

	1、包含多个源文件的项目在编译时有长而复杂的命令行，可以通过makefile保存这些命令行来简化该工作
	2、make可以减少重新编译所需要的时间，因为make可以识别出哪些文件是新修改的
	3、Make维护了当前项目中各文件的相关关系，从而可以在编译前检查
	是否可以找到所有的文件	

Linux的主要特性有哪些?

	Linux是免费的、源代码开放的、符合POSIX标准规范的操作系统
	PMMU — 页式内存管理
	抢占式多任务处理
	VFS – 虚拟文件系统
	网络功能（如，支持TCP/IP ）
	动态加载模块
	支持SMP
	支持绝大多数的32位和64位CPU 等	

说一下sort命令的作用和用法?

  	 sort命令 •一般格式： sort   [选项]  文件列表 
	•说明：用来对文本文件的各行进行排序        
	 排序比较是依据从输入文件的每一行中提取的一个或多个排序关键字进行的。   
	•选项：    
	 -m    对已经排好序的文件统一进行合并，但不做排序。     
	 -c     检查给定的文件是否已排好序，若没有，则显示出错消息，不做  排序。     
	 -u     与-c选项一起用，严格地按顺序检查；否则，对排序后的重复行只输出第一行。     
	 -o  文件名      将排序输出放到该文件名所指定的文件中。如果该文件不存在，则创建一个新文件。

什么是虚拟内存?虚拟内存有什么优势?
	
	虚拟内存是计算机系统内存管理的一种技术。它使得应用程序认为它拥有连续的可用的内存（一个连续完整的地址空间），而实际上，它通常是被分隔成多个物理内存碎片，还有部分暂时存储在外部磁盘存储器上，在需要时进行数据交换。与没有使用虚拟内存技术的系统相比，使用这种技术的系统使得大型程序的编写变得更容易，对真正的物理内存（例如RAM）的使用也更有效率。

	物理内存有限，是一种稀缺资源
	32位系统中，每个进程独立的占有4G虚拟空间。
	虚拟内存优势：
	用户程序开发方便
	保护内核不受恶意或者无意的破坏
	隔离各个用户进程

shell编程题：

	1．用Shell编程，判断一文件是不是字符设备文件，如果是将其拷贝到 /dev 目录下。
		#!/bin/sh
		FILENAME=
		echo “Input file name：”
		read FILENAME
		if [ -c "$FILENAME" ]
		then
		cp $FILENAME /dev
		fi
	2.请下列shell程序加注释，并说明程序的功能和调用方法：#!/bin/sh
		#!/bin/sh
		#
		# /etc/rc.d/rc.httpd
		#
		# Start/stop/restart the Apache web server.
		#
		# To make Apache start automatically at boot, make this
		# file executable: chmod 755 /etc/rc.d/rc.httpd
		#
		case “$1″ in
		‘start’)
		/usr/sbin/apachectl start ;;
		‘stop’)
		/usr/sbin/apachectl stop ;;
		‘restart’)
		/usr/sbin/apachectl restart ;;
		*)
		echo “usage $0 start|stop|restart” ;;
		esac
		参考答案：
		（1）程序注释
		#!/bin/sh 定义实用的shell
		#
		# /etc/rc.d/rc.httpd 注释行，凡是以#号开始的行均为注释行。
		#
		# Start/stop/restart the Apache web server.
		#
		# To make Apache start automatically at boot, make this
		 
		# file executable: chmod 755 /etc/rc.d/rc.httpd
		#
		case “$1″ in #case结构开始，判断“位置参数”决定执行的操作。本程序携带一个“位置参数”，即$1
		‘start’) #若位置参数为start
		/usr/sbin/apachectl start ;; #启动httpd进程
		‘stop’) #若位置参数为stop
		/usr/sbin/apachectl stop ;; #关闭httpd进程
		‘restart’) #若位置参数为stop
		/usr/sbin/apachectl restart ;; #重新启动httpd进程
		*) #若位置参数不是start、stop或restart时
		echo “usage $0 start|stop|restart” ;; #显示命令提示信息：程序的调用方法
		esac #case结构结束

	3．设计一个shell程序，添加一个新组为class1，然后添加属于这个组的30个用户，用户名的形式为stdxx，其中xx从01到30。
		#!/bin/sh
		i=1
		groupadd class1
		while [ $i -le 30 ]
		do
		if [ $i -le 9 ] ;then
		USERNAME=stu0${i}
		else
		USERNAME=stu${i}
		fi
		useradd $USERNAME
		mkdir /home/$USERNAME
		chown -R $USERNAME /home/$USERNAME
		chgrp -R class1 /home/$USERNAME
		i=$(($i+1))
		done
	4．编写shell程序，实现自动删除50个账号的功能。账号名为stud1至stud50。
		#!/bin/sh
		i=1
		while [ $i -le 50 ]
		do
		userdel -r stud${i}
		i=$(($i+1 ))
		done
	5．某系统管理员需每天做一定的重复工作，请按照下列要求，编制一个解决方案：
	（1）在下午4 :50删除/abc目录下的全部子目录和全部文件；
	（2）从早8:00～下午6:00每小时读取/xyz目录下x1文件中每行第一个域的全部数据加入到/backup目录下的bak01.txt文件内；
	（3）每逢星期一下午5:50将/data目录下的所有目录和文件归档并压缩为文件：backup.tar.gz；
	（4）在下午5:55将IDE接口的CD-ROM卸载（假设：CD-ROM的设备名为hdc）；
	（5）在早晨8:00前开机后启动。

		（1）用vi创建编辑一个名为prgx的crontab文件；
		（2）prgx文件的内容：
		50 16 * * * rm -r /abc/*
		0 8-18/1 * * * cut -f1 /xyz/x1 >> /backup/bak01.txt
		50 17 * * * tar zcvf backup.tar.gz /data
		55 17 * * * umount /dev/hdc

	（3）由超级用户登录，用crontab执行 prgx文件中的内容
		root@xxx:#crontab prgx；在每日早晨8:00之前开机后即可自动启动crontab。
	6．设计一个shell程序，在每月第一天备份并压缩/etc目录的所有内容，存放在/root/bak目录里，且文件名为如下形式yymmdd_etc，yy为年，mm为月，dd为日。Shell程序fileback存放在/usr/bin目录下
		（1）编写shell程序fileback：
			#!/bin/sh
			DIRNAME=`ls /root | grep bak`
			if [ -z "$DIRNAME" ] ; then
			mkdir /root/bak
			cd /root/bak
			fi
			YY=`date +%y`
			MM=`date +%m`
			DD=`date +%d`
			BACKETC=$YY$MM$DD_etc.tar.gz
			tar zcvf $BACKETC /etc
			echo “fileback finished!	
		（2）编写任务定时器：
			echo “0 0 1 * * /bin/sh /usr/bin/fileback” >; /root/etcbakcron
			crontab /root/etcbakcron
			或使用crontab -e 命令添加定时任务：
			0 0 1 * * /bin/sh /usr/bin/fileback
	7．有一普通用户想在每周日凌晨零点零分定期备份/user/backup到/tmp目录下
		（1）第一种方法：
			用户应使用crontab –e 命令创建crontab文件。格式如下：
			0 0 * * 0 cp –r /user/backup /tmp
		（2）第二种方法：
			0 * * sun cp –r /user/backup /tmp
			然后执行 crontab file 使生效
	8. 设计一个Shell程序，在/userdata目录下建立50个目录，即user1～user50，并设置每个目录的权限，其中其他用户的权限为：读；文件所有者的权限为：读、写、执行；文件所有者所在组的权限为：读、执行。
		参考答案: 建立程序 Pro16如下：
		#!/bin/sh
		i=1
		while [ i -le 50 ]
		do
		if [ -d /userdata ];then
		mkdir -p /userdata/user$i
		chmod 754 /userdata/user$i
		echo “user$i”
		let “i = i + 1″ （或i=$（（$i＋1））
		else
		mkdir /userdata
		mkdir -p /userdata/user$i
		chmod 754 /userdata/user$i
		echo “user$i”
		let “i = i + 1″ （或i=$（（$i＋1））
		fi
		done

13．某/etc/fstab文件中的某行如下：
/dev/had5 /mnt/dosdata msdos defaults,usrquota 1 2
	
	（1）第一列：将被加载的文件系统名；（2）第二列：该文件系统的安装点；
	（3）第三列：文件系统的类型；（4）第四列：设置参数；
	（5）第五列：供备份程序确定上次备份距现在的天数；
	（6）第六列：在系统引导时检测文件系统的顺序

14．Apache服务器的配置文件httpd.conf中有很多内容，请解释如下配置项：
（1）MaxKeepAliveRequests 200 （2）UserDir public_html
（3）DefaultType text/plain （4）AddLanguare en.en
（5）DocumentRoot“/usr/local/httpd/htdocs”
（6）AddType application/x-httpd-php.php.php.php4
	
	（1）允许每次连接的最大请求数目，此为200；（2）设定用户放置网页的目录；
	（3）设置服务器对于不认识的文件类型的预设格式；
	（4）设置可传送语言的文件给浏览器；（5）该目录为Apache放置网页的地方；
	（6）服务器选择使用php4。

15．某Linux主机的/etc/rc.d/rc.inet1文件中有如下语句，请修正错误，并解释其内容。
/etc/rc.d/rc.inet1：
	
	ROUTE add –net default gw 192.168.0.101 netmask 255.255.0.0 metric 1
	ROUTE add –net 192.168.1.0 gw 192.168.0.250 netmask 255.255.0.0 metric 1
	参考答案:
	修正错误:
	（1）ROUTE应改为小写：route；（2）netmask 255.255.0.0应改为:netmask 255.255.255.0；
	（3）缺省路由的子网掩码应改为:netmask 0.0.0.0；
	（4）缺省路由必须在最后设定,否则其后的路由将无效。
	解释内容:
	（1）route：建立静态路由表的命令；（2）add：增加一条新路由；
	（3）-net 192.168.1.0：到达一个目标网络的网络地址；
	（4）default：建立一条缺省路由；（5）gw 192.168.0.101：网关地址；
	（6）metric 1：到达目标网络经过的路由器数（跳数）。

6．什么是静态路由，其特点是什么?什么是动态路由，其特点是什么?

	静态路由是由系统管理员设计与构建的路由表规定的路由。适用于网关数量有限的场合，且网络拓朴结构不经常变化的网络。其缺点是不能动态地适用网络状况的变化，当网络状况变化后必须由网络管理员修改路由表。
	动态路由是由路由选择协议而动态构建的，路由协议之间通过交换各自所拥有的路由信息实时更新路由表的内容。动态路由可以自动学习网络的拓朴结构，并更新路由表。其缺点是路由广播更新信息将占据大量的网络带宽。

7．进程的查看和调度分别使用什么命令?

	进程查看的命令是ps和top。
	进程调度的命令有at，crontab，batch，kill。

8．当文件系统受到破坏时，如何检查和修复系统?

	成功修复文件系统的前提是要有两个以上的主文件系统，并保证在修复之前首先卸载将被修复的文件系统。
	使用命令fsck对受到破坏的文件系统进行修复。fsck检查文件系统分为5步，每一步检查系统不同部分的连接特性并对上一步进行验证和修改。在执行 fsck命令时，检查首先从超级块开始，然后是分配的磁盘块、路径名、目录的连接性、链接数目以及空闲块链表、i-node。

9．解释i节点在文件系统中的作用。
	在linux文件系统中，是以块为单位存储信息的，为了找到某一个文件在存储空间中存放的位置，用i节点对一个文件进行索引。I节点包含了描述一个文件所必须的全部信息。所以i节点是文件系统管理的一个数据结构。
10．什么是符号链接，什么是硬链接?符号链接与硬链接的区别是什么?
	链接分硬链接和符号链接。
	符号链接可以建立对于文件和目录的链接。符号链接可以跨文件系统，即可以跨磁盘分区。符号链接的文件类型位是l，链接文件具有新的i节点。
	硬链接不可以跨文件系统。它只能建立对文件的链接，硬链接的文件类型位是－，且硬链接文件的i节点同被链接文件的i节点相同。

12．简述网络文件系统NFS，并说明其作用。
	网络文件系统是应用层的一种应用服务，它主要应用于Linux和Linux系统、Linux和Unix系统之间的文件或目录的共享。对于用户而言可以通过 NFS方便的访问远地的文件系统，使之成为本地文件系统的一部分。采用NFS之后省去了登录的过程，方便了用户访问系统资源。

2．简述进程的启动、终止的方式以及如何进行进程的查看。
	参考答案：
	在Linux中启动一个进程有手工启动和调度启动两种方式：
	（1）手工启动
	用户在输入端发出命令，直接启动一个进程的启动方式。可以分为：
	①前台启动：直接在SHELL中输入命令进行启动。
	②后台启动：启动一个目前并不紧急的进程，如打印进程。
	（2）调度启动
	系统管理员根据系统资源和进程占用资源的情况，事先进行调度安排，指定任务运行的时间和场合，到时候系统会自动完成该任务。
	经常使用的进程调度命令为：at、batch、crontab。
3. 简述DNS进行域名解析的过程。
参考答案：
	首先，客户端发出DNS请求翻译IP地址或主机名。DNS服务器在收到客户机的请求后：
	（1）检查DNS服务器的缓存，若查到请求的地址或名字，即向客户机发出应答信息；
	（2）若没有查到，则在数据库中查找，若查到请求的地址或名字，即向客户机发出应答信息；
	（3）若没有查到，则将请求发给根域DNS服务器，并依序从根域查找顶级域，由顶级查找二级域，二级域查找三级，直至找到要解析的地址或名字，即向客户机所在网络的DNS服务器发出应答信息，DNS服务器收到应答后现在缓存中存储，然后，将解析结果发给客户机。
	（4）若没有找到，则返回错误信息。	

10.其他问题：

	1.如何查看nginx mysql apache的版本
		查看linux版本号：cat /etc/redhat-release
						head -n1 /etc/issue
		查看内核版本：cat /proc/version
					uname -a 
					uname -r

		查看apache的版本：
			/usr/sbin/apachectl -v
			httpd -v
		查看mysql版本：
			mysql -help | grep Distrib
			mysql -V
			/bin/mysql -u root -p -e "select version()"
			mysqladmin version

		查看php版本：
			php -v
			phpinfo()
		查看nginx版本：
			nginx -V
		
		查看nginx编译参数：
			/usr/local/nginx/sbin/nginx -V

		查看apache编译参数：
			cat /usr/local/apache/build/config.nice
			/usr/local/apache/bin/apachectl -V

		查看php编译参数：
			/usr/local/php/bin/php -i|grep configure
			/usr/local/php/bin/php-config

		查看mysql编译参数：
			cat "/usr/local/mysql/bin/mysqlbug" | grep configure


