1.E: 无法获得锁 /var/lib/dpkg/lock-frontend - open (11: 资源暂时不可用) E: 无法获取 dpkg 前端锁 (/var/lib/dpkg/lock-frontend)，是否有其他进程正占用它？

解决方案：

强制解锁,命令 sudo rm /var/cache/apt/archives/lock

​						 sudo rm /var/lib/dpkg/lock

