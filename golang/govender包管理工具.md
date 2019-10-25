## govendor 包管理工具

### 1.安装govendor

`go get -u -v github.com/kardianos/govendor`

### 2.init

`govendor init`

将在your dir目录下生成vendor目录和vendor/vendor.json文件

可以配置vendor.json文件，比如编辑“ignore”字段来忽略掉某些目录下的包

（这些包将不会加到vendor目录下），例如

```
{
     "comment": "",
     "ignore": "test github.com/xxx/",
     "package": [],
     "rootPath": "github.com/fenfenbingo/bingosession"
 }
```

### 3.包管理

#### 3.1常用命令

常见的命令如下，格式为 `govendor COMMAND`。

| 命令           | 功能                                                         |
| -------------- | ------------------------------------------------------------ |
| `init`         | 初始化 vendor 目录                                           |
| `list`         | 列出所有的依赖包                                             |
| `add`          | 添加包到 vendor 目录，如 govendor add +external 添加所有外部包 |
| `add PKG_PATH` | 添加指定的依赖包到 vendor 目录                               |
| `update`       | 从 $GOPATH 更新依赖包到 vendor 目录                          |
| `remove`       | 从 vendor 管理中删除依赖                                     |
| `status`       | 列出所有缺失、过期和修改过的包                               |
| `fetch`        | 添加或更新包到本地 vendor 目录                               |
| `sync`         | 本地存在 vendor.json 时候拉去依赖包，匹配所记录的版本        |
| `get`          | 类似 `go get` 目录，拉取依赖包到 vendor 目录                 |

具体来看，这些包可能的类型如下：

![1](https://github.com/tagDong/note/blob/master/assets/image/govender/img.png)

#### 3.2添加依赖

govendor只是用来管理项目的依赖包，如果GOPATH中本身没有项目的依赖包，则需要通过go get先下载到GOPATH中，再通过govendor add +external拷贝到vendor目录中。

```
govendor add +external
或使用缩写： govendor add +e
```

govendor get

使用govendor get可以在安装包的同时将包纳入版本管理，而且会将包安装在$GOPATH/src/your_proj/vendor，更符合我们的要求。

```
#安装在$GOPATH/src/your_proj/vendor下
govendor get github.com/go-sql-driver/mysql
```

#### 3.3 更新依赖包

更新已经在vendor目录下管理的包，从线上更新到本地。

`govendor fetch +v`

### 4 常见错误

服务器提示某个依赖包没有找到：cannot find package 。。。

原因可能是vendor文件中没有该包或者vendor.json文件中没有该包的描述信息。

假设"github.com/astaxie/beego/logs"包的信息在vendor.json文件中没有找到，则在go命令行中执行govendor add github.com/astaxie/beego/logs。

