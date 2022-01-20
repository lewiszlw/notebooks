# 常用命令
### wc: 统计文本中行数、字数、字符数
-l 统计行数 -c 统计字符数 
`wc \<filename\>`
`cat info.log \| grep "a" \| wc -l`

### scp: 远程文件拷贝
1.将本地文件复制到远程主机文件夹
`scp local_file remote_username@remote_ip:remote_folder `
2.将远程主机文件复制到本地文件夹
`scp remote_username@remote_ip:remote_filepath local_folder`

### tcpdump: 网络抓包工具
// TODO
`tcpdump -i eth0 -w dump2.pcap` 监听 eth0 端口，获取抓包文件。用Wireshark打开抓包文件进行分析。

### top: 实时显示进程和系统状态
![top命令截图](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/linux/top%E5%91%BD%E4%BB%A4.png)

第一行：概况
- `HH:mm:ss`：当前的系统时间。
- `up xxx days, HH:mm`：从本次开机到现在经过的时间。
- `x user`：当前有几个用户登录到该机器。
- `load average`：系统1分钟、5分钟、15分钟内的平均负载值。

第二行：进程计数（Tasks）
- `total`：进程总数。
- `running`：正在运行的进程数，对应状态TASK_RUNNING。
- `sleeping`：睡眠的进程数，对应状态TASK_INTERRUPTIBLE和TASK_UNINTERRUPTIBLE。
- `stopped`：停止的进程数，对应状态TASK_STOPPED。
- `zombie`：僵尸进程数，对应状态TASK_ZOMBIE。
![Linux进程状态转换](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/linux/Linux%E8%BF%9B%E7%A8%8B%E7%8A%B6%E6%80%81%E8%BD%AC%E6%8D%A2.png)

第三行：CPU使用率（%Cpu(s)）
- `us`：进程在用户空间（user）消耗的CPU时间占比，不包含调整过优先级的进程。
- `sy`：进程在内核空间（system）消耗的CPU时间占比。
- `ni`：调整过用户态优先级的（niced）进程的CPU时间占比。
- `id`：空闲的（idle）CPU时间占比。
- `wa`：等待（wait）I/O完成的CPU时间占比。
- `hi`：处理硬中断（hardware interrupt）的CPU时间占比。
- `si`：处理软中断（software interrupt）的CPU时间占比。
- `st`：当Linux系统是在虚拟机中运行时，等待CPU资源的时间（steal time）占比。

第四、五行：物理内存和交换空间（Mem/Swap）
- `total`：内存总量。
- `free`：空闲内存量。
- `used`：使用中的内存量。
- `buff/cache`：缓存和page cache占用的内存量。

以下所有行：进程详细信息
- `PID`：进程ID。
- `USER`：进程所有者的用户名。
- `PR`：从系统内核角度看的进程调度优先级。
- `NI`：进程的nice值，即从用户空间角度看的进程优先级。值越低，优先级越高。
- `VIRT`：进程申请使用的虚拟内存量。
- `RES`：进程使用的驻留内存（即未被swap out的内存）量。
- `SHR`：进程使用的共享内存量。
- `S`：进程状态。R=running，S=interruptible sleeping，D=uninterruptible sleeping，T=stopped，Z=zombie。
- `%CPU`：进程在一个更新周期内占用的CPU时间比例。
- `%MEM`：进程占用的物理内存比例。
- `TIME+`：进程创建后至今占用的CPU时间长度。
- `COMMAND`：运行进程使用的命令。

`top` 命令可以接一些参数使用
- `-c` 显示完整命令
- `-p {pid}` 显示指定的进程信息

**FAQ**
**1.某一进程的 CPU 占用率超过 100%？**
每个进程的 CPU 占用率是进程在每个 CPU 核心上的占用率累加起来，即如果任务占用 200％ 的CPU，则它将占用两个内核。

**2.如何计算机器的 CPU 利用率 和 Mem 使用率？**
CPU 利用率% = 100 - `id`（第 3 行）

实际已用内存 = used - ( buffers + cache )
实际可用内存 = free + buffers + cache
内存使用率 =  (used - ( buffers + cache )) / total
当 swap 的 used 值大于 0 时基本可以认为系统遇到瓶颈了，需要加内存。

内存可以使用 `free -h` 命令查看。

### ps
// TODO
```
ps -ef | grep {关键字}
```

### vmstat
// TODO

### lsof: 列出当前系统打开文件
```
查看端口占用情况
lsof -i:{port}
```

```
通过某个进程号显示该进行打开的文件
lsof -p {pid}
```

### netstat


# 常用命令组合
### find /home/work/data -type f | grep -v "d246c\|6e922\" | xargs rm
查找 /home/work/data 目录下所有文件（非目录），当其 filepath 不包含 d246c 或 6e922 字符串时，进行删除操作。

### find / -type l | xargs ls -l | grep "\-> liblua-5.1.so"
查找根目录下所有指向 liblua-5.1.so 的软链接。