# go 什么时候发生内存泄漏
https://gfw.go101.org/article/memory-leaking.html
1. 共享底层字节序列，字节、字符串。 s0 = s1[:50] 。若s1 远大于50 ，由于共享底层，不会释放多余的内存块。
2. 协程被永久阻塞而造成的永久性内存泄露。
3. 没有停止不再使用的time.Ticker值而造成的永久性内存泄露
4. context timerCtx 没有调用cancel

### struct 是否可比较，可用于map的key ，不同的struct呢？
可比较：Integer，Floating-point，String，Boolean，Complex(复数型)，Pointer，Channel，Interface，Array
不可比较：Slice，Map，Function

可以比较，也不能比较。根据成员变量类型。
struct必须是可比较的，才能作为key，否则编译时报错
如果成员变量中含有不可比较成员变量，即使可以强制转换，也不可以比较