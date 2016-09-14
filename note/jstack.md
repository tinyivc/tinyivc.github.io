### jstack命令

#### 功能

查看Java线程的调用堆栈，生成JVM当前时刻的线程快照(JVM内每一个线程正在执行的方法堆栈的集合)可以用来分析线程问题（如死锁、锁争用、CPU负载高）、内存问题(OOM、内存泄漏)。

#### 为什么要生成线程快照？

主要是定位线程出现长时间停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间等待等。

线程出现停顿的时候通过jstack来查看各个线程的调用堆栈，就可以知道无响应的线程到底在后台做什么事情，或者等待什么资源。 如果程序崩溃生成core文件，jstack可以获得core文件的java stack和native stack的信息，从而可以轻松地知道java程序是如何崩溃和在程序何处发生问题。另外，jstack还可以附属到正在运行的java程序中，看到当时运行的java程序的java stack和native stack的信息，如果现在运行的java程序呈现hung的状态，jstack是非常有用的。

#### 如何使用？

jstack -help
![](https://raw.githubusercontent.com/tinyivc/tinyivc.github.io/master/img/jstack-help.jpg)

```shell
jstack [-l] <pid>
    (to connect to running process)
jstack -F [-m] [-l] <pid>
    (to connect to a hung process)
jstack [-m] [-l] <executable> <core>
    (to connect to a core file)
    executable:Java executable from which the core dump was produced
    core:将被打印信息的core dump文件
jstack [-m] [-l] [server_id@]<remote server IP or hostname>
    (to connect to a remote debug server)
```

#### 示例

jps
找到Java进程id:2334

```shell
jstack -l 2334
jstack -l -m 2334
jstack -m 2334
```

#### 具体参数


    -h or -help to print this help message

    -l  long listing. Prints additional information about locks
打印额外的锁信息，在发生死锁时，用来观察锁持有情况

![](https://raw.githubusercontent.com/tinyivc/tinyivc.github.io/master/img/jstack-l.jpg)

    -m  to print both java and native frames (mixed mode)
不仅会输出Java堆栈信息，还会输出C/C++堆栈信息（比如native方法）

![](https://raw.githubusercontent.com/tinyivc/tinyivc.github.io/master/img/jstack-m.jpg)

jstack -l -m pid

![](https://raw.githubusercontent.com/tinyivc/tinyivc.github.io/master/img/jstack-l-m.jpg)

    -F  to force a thread dump. Use when jstack <pid> does not respond (process is hung)
进程没有响应的时候，强制打印线程栈信息。
