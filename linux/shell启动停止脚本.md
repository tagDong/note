### Start.sh

后台启动 重定向输出到out.log文件
```
#!/bin/sh
nohup ./name > out.log 2>&1 &
```
nohup:

no hangup，不挂断地运行命令。只用nohup命令，关闭终端，进程还存在。若在终端中直接使用Ctrl+c,则会关闭进程。

&：

后台运行。当你只使用“&”时，关闭终端，进程会关闭。

建议：

所以当你要让程序在后台不挂断运行时，需要将nohup和&一起使用。

```
linux中有三种标准输入输出
STDIN  --- 0
STDOUT --- 1
STDERR --- 2
```

### Stop.sh

批量杀死名字为name 的进程
```
#!/bin/sh#
ps -ef | grep name | grep -v grep | awk '{print $2}' | xargs kill -9
```

```
ps -ef 用于获取当前系统所有进程，如上图所示。 
grep rtprecv 过滤出与“rtprecv”字符相关的数据（以行为单位）。 
grep -v grep 的作用是除去本次操作所造成的影响，-v 表示反向选择。 
awk '{print $2}' 表示筛选出我们所关注的进程号，$2 表示每行第二个变量，在这个例子中就是进程号。所以如果你使用ps工具不一样，或者ps带的参数不一样，那需要关注的就可能不是$2，可能是$1 。 
xargs kill -9 中的 xargs 命令表示用前面命令的输出结果（也就是一系列的进程号）作为 kill -9 命令的参数，-9 表示强制终止，不是必须的。
```
上面是用 kill 配合过滤操作来完成，实际上还有更简单的方法——使用 killall 命令。killall 通过进程名字终止所有进程，用法如下：killall <process_name> 。 

```
killall -9 name
```

```
ps ： 报告当前进程的快照
kill ： 向一个进程发出信号
killall ： 按名字消灭进程
pkill ： 根据名字和其它属性查看或者发出进程信号
skill ： 发送一个信号或者报告进程状态
xkill ： 按照X资源消灭一个客户程序
```

### 权限

```
chmod +x *.sh
```