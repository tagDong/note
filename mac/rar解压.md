### MAC RAR

mac默认不支持rar文件的解压和压缩

第一步，先下载rar到本地。

https://www.rarlab.com/rar/rarosx-5.6.0.tar.gz

第二步，解压

tar -zxvf rarosx-5.6.0.tar.gz

第三步，需要执行两个命令。一个是安装rar压缩命令，一个是安装unrar解压命令。

```
sudo install -c -o $USER rar /usr/local/bin/
sudo install -c -o $USER unrar /usr/local/bin
```

第四步，解压rar

```
unrar x name.rar outPath
unrar x a.rar ~/Downloads
```