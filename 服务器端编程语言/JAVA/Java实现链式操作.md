- ### 类的链式操作
```java
/**
类的链式操作
Test t = new Test();
t.set().test();
*/
public class Test {

	public Test set(){
		System.out.println("构造方法");
		return this;
	}

	public Test test(){
		System.out.println("Test");
		return this;
	}
}
```

- ### 静态类的链式操作
```java
/**
类的链式操作
Test.set().test();
*/
public class Test {
  private static final Test instance = new Test();
  
  public static Test set(){
		System.out.println("set");
		return instance;
	}

	public static Test test0(){
		System.out.println("Test0");
		return instance;
	}
}