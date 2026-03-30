#### Category

系统中可以有多个Category，它们都是继承自同一个根，每个Category负责记录自己的日志;每个Category可以添加多个Appender

#### Appender

每个Appender指定了一个日志的目的地，例如文件、字符流或者Windows日志，当Category记录一条日志时，该日志被写入所有附加到此Category的Appender

**Log4cpp中所有可直接使用的Appender列表如下：**

```c++
1 Ø log4cpp::IdsaAppender                        // 发送到IDS或者
2 Ø log4cpp::FileAppender                         // 输出到文件
3 Ø log4cpp::RollingFileAppender            // 输出到回卷文件，即当文件到达某个大小后回卷
4 Ø log4cpp::OstreamAppender               // 输出到一个ostream类
5 Ø log4cpp::RemoteSyslogAppender             // 输出到远程syslog服务器
6 Ø log4cpp::StringQueueAppender       // 内存队列
7 Ø log4cpp::SyslogAppender                    // 本地syslog
8 Ø log4cpp::Win32DebugAppender      // 发送到缺省系统调试器
9 Ø log4cpp::NTEventLogAppender        // 发送到win事件日志
```



#### Layout

Layout定义了这个Appender上日志的格式