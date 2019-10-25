## go test

go test 默认执行当前目录下以xxx_test.go的测试文件。

go test -v 可以看到详细的输出信息。

go test -v xxx_test.go 指定测试单个文件，但是该文件中如果调用了其它文件中的模块会报错。


```
带上-count=1参数禁用缓存。

如，执行下面命令测试，便会禁用缓存测试结果

go test -v -count=1 filename_test.go
```

常用的参数：
```
-bench regexp 执行相应的 benchmarks，例如 -bench=.
-cover 开启测试覆盖率
-run regexp 只运行 regexp 匹配的函数，例如 -run=Array 那么就执行包含有 Array 开头的函数
-v 显示测试的详细命令
```

#### 1) 单元测试命令行

单元测试使用 go test 命令启动，例如：

```
$ go test helloworld_test.go
ok          command-line-arguments        0.003s
$ go test -v helloworld_test.go
=== RUN   TestHelloWorld
--- PASS: TestHelloWorld (0.00s)
        helloworld_test.go:8: hello world
PASS
ok          command-line-arguments        0.004s
```
代码说明如下：
```
第 1 行，在 go test 后跟 helloworld_test.go 文件，表示测试这个文件里的所有测试用例。
第 2 行，显示测试结果，ok 表示测试通过，command-line-arguments 是测试用例需要用到的一个包名，0.003s 表示测试花费的时间。
第 3 行，显示在附加参数中添加了-v，可以让测试时显示详细的流程。
第 4 行，表示开始运行名叫 TestHelloWorld 的测试用例。
第 5 行，表示已经运行完 TestHelloWorld 的测试用例，PASS 表示测试成功。
第 6 行打印字符串 hello world。
```

#### 2) 运行指定单元测试用例

go test 默认执行文件内的所有测试用例。可以使用-run参数选择需要的测试用例单独执行，参考下面的代码。

```
package code11_3
import "testing"
func TestA(t *testing.T) {
    t.Log("A")
}
func TestAK(t *testing.T) {
    t.Log("AK")
}
func TestB(t *testing.T) {
    t.Log("B")
}
func TestC(t *testing.T) {
    t.Log("C")
}
```
这里指定 TestA 进行测试：
```
$ go test -v -run TestA select_test.go
=== RUN   TestA
--- PASS: TestA (0.00s)
        select_test.go:6: A
=== RUN   TestAK
--- PASS: TestAK (0.00s)
        select_test.go:10: AK
PASS
ok          command-line-arguments        0.003s
```
TestA 和 TestAK 的测试用例都被执行，原因是-run跟随的测试用例的名称支持正则表达式，使用-run TestA$即可只执行 TestA 测试用例。

#### 3) 单元测试日志

每个测试用例可能并发执行，使用 testing.T 提供的日志输出可以保证日志跟随这个测试上下文一起打印输出。testing.T 提供了几种日志输出方法，详见下表所示。

单元测试框架提供的日志方法
```
Log 打印日志，同时结束测试
Logf	格式化打印日志，同时结束测试
Error	打印错误日志，同时结束测试
Errorf	格式化打印错误日志，同时结束测试
Fatal	打印致命日志，同时结束测试
Fatalf	格式化打印致命日志，同时结束测试
```

### 基准测试——获得代码内存占用和运行效率的性能数据

基准测试可以测试一段程序的运行性能及耗费 CPU 的程度。Go 语言中提供了基准测试框架，使用方法类似于单元测试，使用者无须准备高精度的计时器和各种分析工具，基准测试本身即可以打印出非常标准的测试报告。
#### 1) 基础测试基本使用

基准测试的基本使用方法。
```
$ go test -v -bench=. benchmark_test.go
goos: linux
goarch: amd64
Benchmark_Add-4           20000000         0.33 ns/op
PASS
ok          command-line-arguments        0.700s
```
代码说明如下：
```
第 1 行的-bench=.表示运行 benchmark_test.go 文件里的所有基准测试，和单元测试中的-run类似。
第 4 行中显示基准测试名称，2000000000 表示测试的次数，也就是 testing.B 结构中提供给程序使用的 N。“0.33 ns/op”表示每一个操作耗费多少时间（纳秒）。
```
Windows 下使用 go test 命令行时，-bench=.应写为-bench="."。
#### 2) 基准测试原理

基准测试框架对一个测试用例的默认测试时间是 1 秒。开始测试时，当以 Benchmark 开头的基准测试用例函数返回时还不到 1 秒，那么 testing.B 中的 N 值将按 1、2、5、10、20、50……递增，同时以递增后的值重新调用基准测试用例函数。

#### 3) 自定义测试时间

通过-benchtime参数可以自定义测试时间，例如：
```
$ go test -v -bench=. -benchtime=5s benchmark_test.go
goos: linux
goarch: amd64
Benchmark_Add-4           10000000000                 0.33 ns/op
PASS
ok          command-line-arguments        3.380s
```

#### 4) 测试内存

```
基准测试可以对一段代码可能存在的内存分配进行统计，下面是一段使用字符串格式化的函数，内部会进行一些分配操作。
func Benchmark_Alloc(b *testing.B) {
    for i := 0; i < b.N; i++ {
        fmt.Sprintf("%d", i)
    }
}
```
在命令行中添加-benchmem参数以显示内存分配情况，参见下面的指令：
```
$ go test -v -bench=Alloc -benchmem benchmark_test.go
goos: linux
goarch: amd64
Benchmark_Alloc-4 20000000 109 ns/op 16 B/op 2 allocs/op
PASS
ok          command-line-arguments        2.311s
```
代码说明如下：
```
第 1 行的代码中-bench后添加了 Alloc，指定只测试 Benchmark_Alloc() 函数。
第 4 行代码的“16 B/op”表示每一次调用需要分配 16 个字节，“2 allocs/op”表示每一次调用有两次分配。
```

#### 5) 控制计时器

有些测试需要一定的启动和初始化时间，如果从 Benchmark() 函数开始计时会很大程度上影响测试结果的精准性。testing.B 提供了一系列的方法可以方便地控制计时器，从而让计时器只在需要的区间进行测试。我们通过下面的代码来了解计时器的控制。
```
func Benchmark_Add_TimerControl(b *testing.B) {
    // 重置计时器
    b.ResetTimer()
    // 停止计时器
    b.StopTimer()
    // 开始计时器
    b.StartTimer()
    var n int
    for i := 0; i < b.N; i++ {
        n++
    }
}
```
从 Benchmark() 函数开始，Timer 就开始计数。StopTimer() 可以停止这个计数过程，做一些耗时的操作，通过 StartTimer() 重新开始计时。ResetTimer() 可以重置计数器的数据。