### golang交叉编译

golang如何在一个平台编译另外一个平台可以执行的文件。比如在mac上编译Windows和linux可以执行的文件。那么我们的问题就设定成：如何在mac上编译64位linux的可执行文件。
解决方案

golang的交叉编译要保证golang版本在1.5以上，本解决方案实例代码1.9版本执行的。

我们想要编译的文件hello.go

hello.go

```
package main

import "fmt"

func main() {
    fmt.Printf("hello, world\n")
}
```

在mac上编译64位linux的命令编译命令

```
GOOS=linux GOARCH=amd64 go build hello.go
```

参数解析

这里用到了两个变量：
- GOOS：目标操作系统
- GOARCH：目标操作系统的架构

| OS    |    ARCH          |   OS version
|-------|------------------|----------------
|linux  | 386 / amd64 / arm|  >= Linux 2.6
|darwin | 386 / amd64      | OS X (Snow Leopard + Lion)
|freebsd| 386 / amd64      | >= FreeBSD 7
|windows| 386 / amd64      | >= Windows 2000

