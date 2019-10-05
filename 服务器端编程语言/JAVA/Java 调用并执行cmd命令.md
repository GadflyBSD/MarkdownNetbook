```java
/**
 * 每个程序都有自己的Runtime实例
 * 使程序能与运行环境相关联
 */
public class RuntimeDemo {
    public static void main(String[] args) throws Exception {
        Runtime runtime = Runtime.getRuntime();
         
        /*
         * 执行指定的字符串命令
         * 相对路径现在当前目录找，然后去path找
         * 绝对路径直接在绝对路径里找
         */
        //runtime.exec("mspaint.exe");
         
        /*
         * 返回对创建的进程的管理对象
         */
        //Process p = runtime.exec("mspaint.exe");
        //Thread.sleep(10000);
        //杀死刚才创建的进程，打开资源管理器，10秒钟后进程消失
        //p.destroy();
         
         
        /*
         * 还可以用指定方式打开文件
         * 默认是在当前目录找，但是eclipse里面有个src
         * 在eclipse里会去src的上层目录找
         */
        runtime.exec("notepad.exe src/net/xsoftlab/baike/RuntimeDemo.java");
    }
}
```