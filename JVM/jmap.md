## jmap
## 线上dump文件(慎用，短时间jvm停止响应)
jmap -dump:format=b,file=dump.dat PID

## 参数解释
- format=b     binary format
- file=< file>  dump heap to < file>
