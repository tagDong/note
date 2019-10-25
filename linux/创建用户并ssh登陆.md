# linux创建账户生成主目录并ssh登陆

## 创建账户并创建主目录

1.添加用户并生成主目录：useradd -d /home/test -m test;

2.然后给test设置密码：passwd test;

语法：useradd  选项 用户名 该命令的各选项含义如下：

```-c comment    描述新用户帐号，通常为用户全名，comment 为字符串。
-d home_dir   设置用户主目录，默认值为用户的登录名，并放在/home目录下。
-D            创建新帐号后保存为新帐号设置的默认信息。
-e expire_date  用 MM/DD/YYYY 格式设置帐号过期日期。
-f inactivity   设置口令失效时间，该值为 0 使口令失效后帐号立即失效，为 -1 使该选项失效。
-g group      设置所要创建新用户所在的基本组，group为组名。
-k skel_dir   设置框架目录，该目录包含用户的初始配置文件，
              创建用户时该目录下的文件都被复制到用户主目录下。
-m   自动创建用户主目录，并把框架目录(默认为/etc/skel)下的文件复制到用户主目录下。
-M   不创建用户主目录。
-r   允许保留的系统帐号使用用户ID创建一个新帐号。 
-s shell    指定用户的登录shell。
-u user_id  设置用户ID。
```
## 生成sshkey并登陆

1.生成key： ssh-keygen

2.创建目录及文件
  sudo mkdir /home/test/.ssh
  sudo touch /home/test/.ssh/authorized_keys
  
3.#修正所有者
  sudo chown -R zero. /home/test/.ssh
  
4.修改权限
  sudo chmod 700 /home/test/.ssh
  sudo chmod 600 /home/test/.ssh/authorized_keys
  
5.开启ssh远程登陆，用户管理

5.1开始编辑

`sudo vi /etc/ssh/sshd_config`

5.2端口配置
```
Port 419        #开放的端口
PermitRootLogin no        #禁止root登陆
PasswordAuthentication no         #禁止密码登陆
```

5.3 限制用户ssh登陆
只允许指定用户进行登录（白名单）：
在/etc/ssh/sshd_config配置文件中设置AllowUsers选项，（配置完成需要重启 SSHD 服务）格式如下：

`AllowUsers    aliyun test@192.168.1.1  # 允许 aliyun 和从 192.168.1.1 登录的 test 帐户通过 SSH 登录系统。`

只拒绝指定用户进行登录（黑名单）：
在/etc/ssh/sshd_config配置文件中设置DenyUsers选项，（配置完成需要重启SSHD服务）格式如下：   

`DenyUsers    zhangsan aliyun    #Linux系统账户  # 拒绝 zhangsan、aliyun 帐户通过 SSH 登录系统`

5.4保存后重启sshd

`sudo service sshd restart`

参考：

Linux限制某些用户或IP登录SSH、允许特定IP登录SSH ：https://www.cnblogs.com/EasonJim/p/8334122.html