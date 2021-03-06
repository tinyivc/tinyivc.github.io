#### 背景

定期删除服务器上日志文件，只保留一定期限的文件。

#### 解决方案

用crontab命令定时执行删除命令。

0 1 * * * /bin/find /opt/logs/logs/ -name bizad-api.log.* -mtime +45 -delete > /dev/null 2>&1

每天凌晨1点执行命令。

#### 验证方案

1、验证调度的命令是否正确，首先，把要删除的文件查出来，不要加-delete参数，看是否符合删除范围。

2、验证没问题后，验证整个命令是否生效，可以把-mtime的参数调大一点，多保留一些日志。以便下一步骤使用。

3、修改定时任务计划，改为* * * * *，每分钟执行一次。观察用定时调度执行时，是否正常。

4、没有问题后，把任务计划改回去。到此就完成了。

#### 总结

command > /dev/null 2>&1 &

这句命令意思是后台执行命令，将错误输出(2)重定向到标准输出(1)，然后将标准输出(1)全部输出到/dev/null文件，也就是清空。

0表示键盘输入
1表示标准输出
2表示错误输出

command > out.txt 2>&1表示将错误输出重定向到标准输出，然后将标准输出重定向到out.txt

&1文件描述符，代表标准输出，少了&就成了数字1，就表示重定向到文件1。

& ：后台执行



2>&1写在后面的原因

command >out.txt 2>&1 == command 1>out.txt 2>&1

重定向符号>左边默认是1，标准输出重定向到out.txt

首先标准输出重定向到out.txt中，2>&1是错误输出拷贝到标准输出，最终标准输出和错误输出重定向到out.txt

如果改成： command 2>&1 >out.txt

错误输出重定向到标准输出，此时标准输出还是在终端，所以错误输出指针指向了终端，>file后标准输出才重定向到out.txt，但错误输出仍然指向终端。