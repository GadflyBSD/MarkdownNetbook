> 这篇教程是关于如何在Windows操作系统上使用PyCharm IDE远程连接树莓派执行Python 2.7程序。

1. ### 确认Windows电脑和树莓派在同一个网络里。
2. ### 在你的Windows电脑上安装PyCharm Professional Edition。
3. ### 必须获取到树莓派的IP地址. 打开树莓派的终端窗口输入以下命令：ifconfig。
  ![](https://upload-images.jianshu.io/upload_images/3561944-9b9a01f1d8d1c6dd.png)

4. ### 一旦你获得了树莓派的IP地址, 转到Home目录创建如下图所示子目录. 这个目录用了存放你的Python程序
  ![](https://upload-images.jianshu.io/upload_images/3561944-bfcf820b26a04ff0.png)

5. ### 打开你的PyCharm IDE. 创建一个新项目. 打开 File → New Project → Pure Python → 给项目一个名字。 这里使用RaspberryPiProject。
  ![](https://upload-images.jianshu.io/upload_images/3561944-816281c74844aa19.png)

6. ### 右击RaspberryPiProject并且选择新建 → Python文件，给文件命名为rasp.py。
  ![](https://upload-images.jianshu.io/upload_images/3561944-b2dd14342c710251.png)

7. ### 在编辑其中输入print(“Hello RaspberryPi”)。
8. ### 选择 File → Settings
  ![](https://upload-images.jianshu.io/upload_images/3561944-7effbfc4bef8ced9.png)
  * 在左边的面板中选中Project:RaspberryPiProject。 展开并点击Project Interpreter。
  * 在右边的面板, 点击Project Interpreter，选择Add Remote菜单如下所示：
    ![](https://upload-images.jianshu.io/upload_images/3561944-6e87bcd8e5ecc904.png)
9. ### 配置远程Python Interpreter。
  ![](https://upload-images.jianshu.io/upload_images/3561944-826ccd751817b5f0.png)
    > 输入之前记录下来的树莓派的IP地址。 输入用户名、密码。 指定Python interpreter位于树莓派上的路径。单击OK。现在可以看到配置好的树莓派的 Python Interpreter。
  
    ![](https://upload-images.jianshu.io/upload_images/3561944-62a55f728bb665af.png)
10. ### 现在转到 Tools à Deployment → Configuration 并且单击左边面板的 + 按钮. 看到如下弹出窗口。
  ![](https://upload-images.jianshu.io/upload_images/3561944-9ce684f340f3cb3b.png)
  现阶段树莓派被作为一个服务器。 给树莓派服务器一个名字 (GS_Research) 选择类型为： SFTP并且单击OK。

11. ### 单击Connections标签页指定树莓派SFTP主机地址。
  * 保留port和Root path默认设置不变. 输入用户名、密码。
  * 接着选择Mappings标签页。
  * 制定用于在Windows电脑存放python项目的路径.
  * 接着制定在树莓派中用于存放Python程序的路径如：/home/pi/pywork，选择确定。
  ![](https://upload-images.jianshu.io/upload_images/3561944-bf78c2846b0a3c48.png)
12. ### 现在我们可以上传位于Windows电脑的Python到树莓派并使用树莓派的Python Interpreter编译程序。
    > 转到 Tools → Deployment → Upload to GS_Research。
  
    ![](https://upload-images.jianshu.io/upload_images/3561944-bf78c2846b0a3c48.png)
  * 可以看到文件已经上传到树莓派。
  * 现在打开树莓派的终端并转到python文件存放目录.可以看到rasp.py已经上传到文件存放目录了。
13. ### 我们在PyCharm写的代码需要同步到树莓派. 操作方法选择 Tools → Deployment → Sync with Deployed to GS_Research。
  ![](https://upload-images.jianshu.io/upload_images/3561944-82dc7c448dd5afad.png)