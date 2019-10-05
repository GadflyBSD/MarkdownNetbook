## 一、安装node.js
从 nodejs.org 中下载Node.js for Mac 安装包，也就是一个6M多的pkg文件，下载之后点击安装即可。它将在你的机器上安装 Node.js 和 npm (node package manager).安装成功后你就可以使用 node 和 npm 命令了。

## 二、安装Homebrew
```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 三、安装ANT
```shell
brew install ant
```

## 四、安装android-sdk-tools和相应的SDK
* 从 http://www.androiddevtools.cn/ 中下载相应版本的SDK Tools
* 安装gradle
  ```shell
  brew install gradle
  gradle -v
  ```
* 解压完之后目录下执行命令,该命令会打开android sdk manageer界面
  ```shell
  /Users/yourname/android-sdk-macosx/tools/android 
  ```
* 通过android sdk manageer安装6个包：
  1. Tools --> Android SDK Tools
  2. Tools --> Android SDK Platform-tools
  3. Tools --> Android SDK Build-tools
  4. Andropid 7.1.1 (API 25) --> SDK Platform
  5. Andropid 7.1.1 (API 25) --> Andropid TV Intel X86 Atom System Image
  6. Andropid 6.0 (API 23) --> SDK Platform
  7. Extras --> Android Support Repository

## 配置环境变量
* 配置ant环境变量 `sudo vi .bash_profile`
  ```ini
  export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home
  export CLASSPAHT=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
  export JAVA_BIN=$JAVA_HOME/bin
  export ANT_HOME=/usr/local/Cellar/ant/1.10.1
  export PATH=${PATH}:${ANT_HOME}/bin
  export ANDROID_HOME=/Users/gadflybsd/android-sdk-macosx
  export ANDROID_TOOLS=$ANDROID_HOME/tools
  export ANDROID_PLATFORM_TOOLS=$ANDROID_HOME/platform-tools
  PATH=$PATH:$ANDROID_HOME:$ANDROID_TOOLS:$ANDROID_PLATFORM_TOOLS:$JAVA_HOME:$CLASSPAHT:$JAVA_BIN
  ```
  > 注意：`ANT_HOME` 和 `ANDROID_HOME` 为 `ant` 和 `Android SDK Tools` 的安装目录
* 导入运行环境变量 `source .bash_profile`
* 请到：http://www.oracle.com/technetwork/java/javase/downloads/index.html 下载1.8或以上的Java SDK

## 安装 cordova、Ionic等
* 将cordova和ionic包安装到全局环境中（可供命令行使用）:
```shell
npm --registry https://registry.npm.taobao.org info underscore
sudo npm i npm@latest -g
sudo npm audit
sudo npm install -g phonegap minimatch
sudo npm install -g cordova ionic ionic-framework
```
* 安装ios模拟器 和 ios真机调试环境：
```shell
sudo npm install -g ios-sim
sudo npm install -g ios-deploy --unsafe-perm=true
```