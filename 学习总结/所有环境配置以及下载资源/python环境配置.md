## 1.安装Python

- apt install python3.6
- python 设置环境  *vim .bashrc*   
   里面添加一句export $PATH=$HOME/.local/bin:$PATH
- 设置 python3变python  pip3变pip
  ln -s /usr/bin/python3 /usr/local/bin/python

## 2.安装Pycharm

- 下载地址: https://www.jetbrains.com/pycharm/download/download-thanks.html?platform=linux&code=PCC

- 下载好后解压缩

- 解压好后打开终端

- 打开pycharm.sh所在文件目录

- 输入命令：sh ./pycharm.sh 打开pycharm看是否能运行

- 创建pycharm快捷方式,所有步骤完成

  ```
  在/usr/share/applications中创建文件pycharm.desktop
  
  [Desktop Entry]
  Type=Application
  Name=Pycharm
  GenericName=Pycharm3
  Comment=Pycharm3:The Python IDE
  Exec=/home/mimi/下载/pycharm-2019.2.1/bin/pycharm.sh
  Icon=/home/mimi/下载/pycharm-2019.2.1/bin/pycharm.png
  Terminal=pycharm
  Categories=Pycharm;
  ```

  没有下载vim就下载vim   sudo apt install vim

## 3.安装VNC

- 下载地址: https://www.realvnc.com/download/file/viewer.files/VNC-Viewer-6.19.715-Linux-x64.deb
  