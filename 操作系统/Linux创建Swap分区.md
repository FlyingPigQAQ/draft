## Linux创建swap分区
1. 创建要作为swap分区的文件:增加2GB大小的交换分区，则命令写法如下，其中的count等于想要的块的数量（bs*count=文件大小）。   
  ```shell
  dd if=/dev/zero of=/root/swapfile bs=1M count=2048
  ```


2. 格式化为交换分区文件:
  ```shell
  mkswap /root/swapfile #建立swap的文件系统
  ```


3. 启用交换分区文件:
  ```shell
  swapon /root/swapfile #启用swap文件
  ```


4. 使系统开机时自启用，在文件/etc/fstab中添加一行：
  ```shell
  /root/swapfile swap swap defaults 0 0
  ```
