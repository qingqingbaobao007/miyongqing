# Kernel

Kernel 是操作系统的核心，直接与硬件打交道，处理很多底层的事情。一般禁止普通用户直接操作，外部程序想要调用内核功能需要使用“系统调用”的接口。

![1566904296278](C:\Users\miyongqing\AppData\Roaming\Typora\typora-user-images\1566904296278.png)

# shell

shell 是一个应用程序，它连接了用户和linux内核，让用户能够更加高效，安全，低成本使用linux内核，这就是shell的本质

shell一般分为“用户界面GUI”和“命令行CLI”两种

## 常见的GUI Shell

- Gnome
- KDE
- Xface

## 常见的GLI Shell

- sh
- csh
- bash
- zsh
- 可以通过 cat /etc/shells  查看当前系统安装了哪些 shell

# Bash快捷键

- ctrl + f  前进一个字符

- ctrl + b  后退一个字符

- ctrl + a  回到行首 

- ctrl + e  回到行尾 

- ctrl + w  向左删除一个单词

- ctrl + u  向左删除全部

- ctrl + k  向右删除全部

- ctrl + y  粘贴上次删除的内容

- ctrl + l 清屏

- ctrl + d  注销，退出shell

- ctrl + v  输入控制字符

- ctrl + p - 上一条命令

- ctrl + n - 下一条命令

- ```
  Ctrl-Z: 暂停程序。
  ```

- ```
  Ctrl-S: 停止向屏幕输出。
  ```

- ```
  Ctrl-H: 擦除光标前面的一个字符。
  ```

- ```
  Ctrl-C: 终止当前正在运行的程序。
  ```

- ```
  history = 显示命令历史
  ```

# 目录结构

在Windows中经常看到这样的⽂件路径: C:\Users\Rich\Documents\test.doc 。这个⽂件路径表明
了⽂件 test.doc 位于 C 磁盘分区中。
Linux 采⽤了⼀种不同的⽅式, 在路径名中不使⽤驱动器盘符。
在 Linux 中，你会看到下⾯这种路径: /home/Rich/Documents/test.doc ，路径本身并没有提供任何
有关⽂件究竟存放在哪个物理磁盘上的信息。
Linux虚拟⽬录中⽐较复杂的部分是它如何协调管理各个存储设备。在Linux PC上安装的第⼀块硬盘称
为根驱动器。根驱动器包含了虚拟⽬录的核⼼，其他⽬录都是从那⾥开始构建的。
Linux会在根驱动器上创建⼀些特别的⽬录，我们称之为挂载点(mount point)。挂载点是虚 拟⽬录中⽤
于分配额外存储设备的⽬录。虚拟⽬录会让⽂件和⽬录出现在这些挂载点⽬录中，然 ⽽实际上它们却存
储在另外⼀个驱动器中

![1566904840693](C:\Users\miyongqing\AppData\Roaming\Typora\typora-user-images\1566904840693.png)

![1566904854051](C:\Users\miyongqing\AppData\Roaming\Typora\typora-user-images\1566904854051.png)

## 目录操作

+ 绝对路径：  /usr/local/bin

+ 相对路径:  ../foo/bar

+ 命令列表

  | Command        |         Description          |
  | :------------- | :--------------------------: |
  | pwd            |    显示当前目录的绝对路径    |
  | ls ./          |      显示当前目录的文件      |
  | ls -l ./       |      以列表形式显示文件      |
  | ls -lh ./      | 以人类友好的方式显示文件列表 |
  | ls -A ./       |         显示隐藏文件         |
  | cd xxx         |         切换文件目录         |
  | cd -           |      回到上一次所在位置      |
  | mkdir abc      |     创建名为 abc 的目录      |
  | mkdir -p a/b/c |    创建三层目录结构 a/b/c    |
  |                |                              |

+ ## cp / mv /rm 的通用参数

  -  -i : 覆盖前提示
  -  -n : 如果目标文件已经存在，则停止操作
  -  -f : 如果目标文件已存在，则强制操作，覆盖前不提示
  -  -r: 递归文件夹执行某操作（mv 不需要 -r）

+ ## ln 命令详解

  - 硬链接：ln foo bar
    - 只能在同分区内创建
    - 一个文件的多个硬链接相当于一个文件有多个名字，多个硬链接在磁盘上只占用一个文件的大小
    - 修改硬链接时，所有同源的硬链接都会发生变化
  - 软链接： ln -s foo bar 
    - 可以跨越分区创建
    - 内部只记录目标文件的路径，类似于windows 下的快捷方式
    - 通过软链接修改文件，源文件也会发生修改

## 压缩文件处理

- tar
  - 压缩 tar -czf abc.tar.gz abc/
  - 解压 tar -xzf abc.tar.gz
- zip
  - 压缩 zip -r abc.zip zbc/
  - 解压 unzip abc.zip

## 查找文件find 命令

+ 查找当前⽂件夹下全部⽂件: find ./   隐藏文件也能查找出来
+ 只查找⽂件: find ./ -type f
+ 只查找⽬录: find ./ -type d
+ 只查找链接: find ./ -type l
+ 按名称查找: find ./ -name '*.py'
+ 按权限查找: find ./ -perm 0644
+ 按⼤⼩查找: find ./ -size +1k -size -5k

