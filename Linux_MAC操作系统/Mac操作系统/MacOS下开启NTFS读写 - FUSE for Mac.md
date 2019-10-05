# 安装方法
## 1.首先安装FUSE for Mac
>FUSE for Mac是MacFUSE的继承者，基于FUSE为MacOS用户提供除系统外的第三方文件系统的支持。详见[官方网站](https://osxfuse.github.io)。
最新版请前往[官方网站下载](https://github.com/osxfuse/osxfuse/releases)。

## 2.使用Homebrew安装ntfs-3g
> 在安装FUSE后，使用Homebrew安装ntfs文件系统的支持，执行如下命令：

```shell
brew install ntfs-3g
```

## 3.更换MacOS的NTFS默认挂载程序
> 首先备份系统默认的NTFS挂载程序

```shell
sudo mv /sbin/mount_ntfs /sbin/mount_ntfs.orig
```

> 然后将ntfs-3g挂载程序链接到/sbin/mount_ntfs

```shell
sudo ln -s /usr/local/sbin/mount_ntfs /sbin/mount_ntfs
```

## 4.重新启动
> 重新启动后插入NTFS格式的磁盘即可正常进行读写操作。