> 在烧录之前，可以用SD Card Formatter进行格式化SD卡，这个应用Mac和Win平台都有。
[SD Memory Card Formatter Download Page](https://www.sdcard.org/downloads/formatter/index.html)

![SD Card Formatter for MacOS](https://upload-images.jianshu.io/upload_images/1512373-14793d108d474038.png)
SD Card Formatter for MacOS

![](https://upload-images.jianshu.io/upload_images/1512373-6e4de63e4ca901f7.png)
使用界面
> 说明：在Select card中一定要选择好需要格式化的SD卡设备,然后再Volume label可以修改SD卡设备的名称

## 一、查看驱动器列表
> 在控制台输入`diskutil list`查看驱动器列表

![](https://upload-images.jianshu.io/upload_images/1512373-7f0063f37b343ec0.png)
记好我们的SD卡设备的路径，即/dev/disk2，下面要用到。

## 二、取消SD卡的挂载
> 在控制台输入`diskutil unmountDisk + SD卡设备路径`，取消挂载。出现`Unmount of all volumes on disk2 was successful`后表示取消挂载成功。
![](https://upload-images.jianshu.io/upload_images/1512373-fb8286897cfe127e.png)

## 三、开始烧录
> 在控制台输入`sudo dd if=img/iso文件的路径 of=SD卡设备的路径 bs=1m;sync`。

* 文件的路径中不能出现中文。
* 可以将bs=1m改成bs=4m来加快烧录的写入速度(更新，后来测试发现这样改写好像也没有多大的变化)。
* 同时将disk改为rdisk也能加快写入速度。
* 接着按下Enter键，会提示输入系统管理员密码，然后进入烧录过程。(这个时间有点长，需要耐心等待)
![](https://upload-images.jianshu.io/upload_images/1512373-b755499b6df6fb8b.png)

## 四、烧录完成，推出设备
* 烧录完成的提示如下
  ![](https://upload-images.jianshu.io/upload_images/1512373-5b91bfbbf4e6d497.png)
* 接着输入diskutil eject SD卡设备路径,推出设备。
  ![](https://upload-images.jianshu.io/upload_images/1512373-7b4d8725a5302b7e.png)
* 最后把SD卡插入到树莓派上就可以进行配置使用了。