window拖文件到虚拟机解压文件出现乱码问题

因为window是字符集gbk，而linux是utf8

不能右键解压

解决方法：

终端输入：unzip -O cp936 学习总结.zip 