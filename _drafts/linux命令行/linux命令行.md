1. 通过关键字搜索手册页

   `man -k 关键字`

   例：`man -k hostname`

   ![image.png](assets/image-20210524164946-s5no19r.png)

   - 后面的数字表示手册来自哪块内容

     例：查看指定命令第一部分的内容

     输入 `man 1 hostname` 就可以查看具体的内容
   
2. `ls -F`

   在没安装彩色终端仿真器时，该命令可以区分文件和目录

   ![image.png](assets/image-20210524172000-l910zj1.png)

   - 目录名后面会添加正斜线`/`
   - 可执行文件后面会添加`*`
   
3. `ls -R`

   递归显示当前目录下包含的子目录中的文件。如果目录很多，这个输出就会很长
   
4. `file file_name`

   查看文件的类型
   
5. `cat -n file_name`

   给所有的行加上行号

   ![image.png](assets/image-20210525111048-52swcou.png)
   
6. `cat -b file_name`

   给有文本的行加上行号

   ![image.png](assets/image-20210525111058-ir4o8vc.png)
   
7. `head file_name`

   显示文件开头多少行的内容。默认情况下显示10行。

   与`tail`参数类似
   
8. `top`命令显示实时的进程信息

   ![image.png](assets/image-20210525112845-wb2c64a.png)

   第一行显示了当前时间、系统的运行时间、登录的用户数以及系统的平均负载

   平均负载有3个值：最近1分钟的、最近5分钟的和最近15分钟的平均负载

   > 值越大说明系统的负载越高。如果近15分钟内的平均负载都很高，就说明系统可能有问题
   > 通常，如果系统的负载值超过了2，就说明系统比较繁忙了
   
9. `df`查看所有已挂载磁盘的使用情况

   有可能系统上有运行的进程已经创建或删除了某个文件，但尚未释放文件。这个值是不会算进闲置空间的
   
10. 默认情况下， `du`命令会显示当前目录下所有的文件、目录和子目录的磁盘使用情况

11. `grep -v`搜索不包含指定模式的行

    例：`grep -v t file1` 搜索`file1`中不包含`t`的行

    ![image.png](assets/image-20210525142520-wf737z9.png)
    
12. `grep -n`显示匹配到的行的行号

    例：`grep -n t file1`

    ![image.png](assets/image-20210525142635-a7zp7k1.png)
    
13. `grep -e`搜索时匹配多个模式

    例：`grep -e t -e f file1`搜索含有字符 t 或字符 f 的所有行

    ![image.png](assets/image-20210525142822-sduemsk.png)
    
14. `tar function [options] object1 object2 ...` 对数据进行归档

    - function

      - `-c` --create 

        创建一个新的tar归档文件
      
      - `-x` --extract 
    
        从已有tar归档文件中提取文件
      
      - `-t` --list 
    
        列出已有tar归档文件的内容

    - options

      - `-f` file
      
        输出结果到文件或设备
      
      - `-v`
      
        在处理文件时显示文件
        
      - `-z` 
      
        将输出重定向给gzip命令来解压/压缩内容
    
    例：
    
    - `tar -cvf test.tar test/ test2/`
    
      创建了名为test.tar的归档文件，含有test和test2目录内容
    
    - `tar -tf test.tar`
    
      列出tar文件test.tar的内容（但并不提取文件）
    
    - `tar -xvf test.tar`

        从tar文件test.tar中提取内容。如果tar文件是从一个目录结构创建的，那整个目录结构都会在当前目录下重新创建

    - `tar -zxvf filename.tgz`

      解压gzip压缩过的tar文件

      > gzip压缩过的tar文件以.tgz结尾
    
15. 使用`env`或`printenv`命令查看全局变量

16. 显示个别环境变量的值，可以使用`printenv`命令或者`echo env_name`命令

    ![image-20210525160333322](assets/image-20210525160333322.png)

    > `env`命令不能查看单个环境变量的值

    > 在echo命令中，在变量名前加上`$`不但可以显示变量当前的值，还能够让变量作为命令行参数

17. 设置变量时，变量名、等号和值之间没有空格

    ![image-20210525162158629](assets/image-20210525162158629.png)

18. 删除环境变量

    ![image-20210525162610335](assets/image-20210525162610335.png)

    > 不要使用`$`

19. 什么时候该使用`$`来引用环境变量

    如果要用到变量，使用`$` ；如果要操作变量，不使用`$` 。

    这条规则的一个例外就是使用`printenv`显示某个变量的值

20. 定位系统环境变量

    在登入Linux系统启动一个bash shell时，默认情况下bash会在启动文件中查找命令。bash检查的启动文件取决于启动bash shell的方式。

    启动bashshell有3种方式：

    - 登录时作为默认登录shell

      登录shell会从5个不同的启动文件里读取命令：

      - `/etc/profile`

        该文件是系统上默认的bash shell的主启动文件。系统上的每个用户登录时都会执行这个启动文件。

        会去读取`/etc/profile.d`目录下的所有文件，并执行这些文件

        > 在Ubuntu系统中，会去执行`/etc/bash.bashrc`文件。这个文件包含了系统环境变量

      shell会按照按照下列顺序，运行第一个被找到的文件，余下的则被忽略

      > `$HOME/.bashrc`文件通常通过其他文件运行的

      - `$HOME/.bash_profile`
      - `$HOME/.bash_login`
      - `$HOME/.profile`
      - `$HOME/.bashrc`

      

    - 作为非登录shell的交互式shell

      如果是在命令行提示符下敲下`bash`启动的，只会检查用户`HOME`目录中的`.bashrc`文件

    - 作为运行脚本的非交互shell
