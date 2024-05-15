### centos安装ftp服务
#### 1、安装vsftpd服务

```bash
yum intall -y vsftpd
```

#### 2、核心配置文件

- /etc/vsftpd
- /var/ftp

##### 2.1、/etc/vsftpd目录下面有4个文件

- ftpusers文件，指定哪些用户不能访问ftp服务
- user_list文件，用户列表，当vsftpd.conf里userlist_deny=NO时，只允许这里的用户访问ftp服务，注意此时同事也检测ftpusers文件；当vsftpd.conf里userlist_deny=YES（默认）时，不允许这里的用户访问ftp服务
- vsftpd.conf文件，是vsftp的核心配置文件
- Vsftpd_conf_migrate.sh文件，是vsftpd操作的一些变量和设置脚本

##### 2.2、/var/ftp目录

> /var/ftp目录是默认情况下，匿名用户的根目录，也就是说默认情况下，匿名用户上传的文件将会放在这个目录下。

##### 2.3、核心配置文件的基础配置项

```bash
anonymous_enable=YES   #是否允许匿名登录
local_enable=YES  #是否云讯本地账号（系统账号）登录
write_enable=YES  #是否运行上传操作，如果要运行上传需要开启这个配置
listen=NO  #ftp服务的运行模式，NO时表示xinetd模式，YES时表示standlone模式
userlist_enable=YES #控制user_list的功能 
```

#### 3、服务控制

```bash
systemctl start vsftpd   #开启vsftpd服务
systemctl stop vsftpd  #停止vsftpd服务
systemctl restart vsftpd  #重启vsftpd服务
systemctl status vsftpd  #查看vsftpd服务状态
```

#### 4、ftp测试

- filezilla等ftp工具配置
- ftp命令