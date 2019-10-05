## lambda表达式简介
个人理解，lambda表达式就是一种新的语法，没有什么新奇的，简化了开发者的编码，其实底层还是一些常规的代码。Lambda 是一个匿名函数，我们可以把 Lambda 表达式理解为是一段可以传递的代码（将代码像数据一样进行传递）。可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了提升。

## Lambda表达式的语法
Lambda 表达式的基础语法：Java8中引入了一个新的操作符 "->" 该操作符称为箭头操作符或 Lambda 操作符
箭头操作符将 Lambda 表达式拆分成两部分：
左侧：Lambda 表达式的参数列表
右侧：Lambda 表达式中所需执行的功能， 即 Lambda 体
* ###语法格式一：无参数，无返回值
  `() -> System.out.println("Hello Lambda!");`
  ```java
  //平时的写法
  @Test
  public void test1() {
    Runnable runnable = new Runnable() {
        
  @Override
  public void run() {
    System.out.println("线程启动了");        
        }
    };
    runnable.run();
  }
  
  /**
   * 语法格式一：无参数，无返回值
   *        () -> System.out.println("Hello Lambda!");
  */
  @Test
  public void test2() {
        //“->”左边只有一个小括号，表示无参数，右边是Lambda体(就相当于实现了匿名内部类里面的方法了，(即就是一个可用的接口实现类了。))
    Runnable runnable = ()->System.out.println("线程启动了");    
    runnable.run();
  }
  ```
* ### 语法格式二：有一个参数，并且无返回值
  `(x) -> System.out.println(x)`
  ```java
  /**语法格式二：有一个参数，并且无返回值
  *      (x) -> System.out.println(x)
  */
  @Test
  public void test3() {
    //这个e就代表所实现的接口的方法的参数，
    Consumer<String> consumer = e->System.out.println("ghijhkhi"+e);
    consumer.accept("woojopj");
  }
  ```
* ### 语法格式三：若只有一个参数，小括号可以省略不写(第二种方式的一种简化吧)
  ```java
  x -> System.out.println(x)
  ``` 
* ### 语法格式四：有两个以上的参数，有返回值，并且 Lambda 体中有多条语句
  ```java
  @Test
  public void test4() {
    //Lambda 体中有多条语句，记得要用大括号括起来
    Comparator<Integer> com = (x, y) -> {
        System.out.println("函数式接口");
        return Integer.compare(x, y);
    };
    int compare = com.compare(100, 244);
    System.out.println(compare);
  }
  ```
* ### 语法格式五：若 Lambda 体中只有一条语句， return 和 大括号都可以省略不写 即：
  ```java
  Comparator com = (x, y) -> Integer.compare(x, y);
  ```
* ### 语法格式六：Lambda 表达式的参数列表的数据类型可以省略不写，因为JVM编译器通过上下文推断出，数据类型，即“类型推断”(Integer x, Integer y) -> Integer.compare(x, y);

## 函数式接口
* 到这儿，相信大家也看出来规律了，这个Lambda表达式，好像离不开接口咦.......你还真说对了，这个Lambda表达式，是需要函数式接口的支持的，那么什么是函数式接口呢？
* 函数式接口 ，即只包含一个抽象方法的接口，称为函数式接口。
* 你可以通过 Lambda 表达式来创建该接口的对象。（若 Lambda 表达式抛出一个受检异常，那么该异常需要在目标接口的抽象方法上进行声明）。
* 我们可以在任意函数式接口上使用 @FunctionalInterface 注解，这样做可以检查它是否是一个函数式接口，同时 javadoc 也会包含一条声明，说明这个接口是一个函数式接口。像上面的Consumer接口就是一个函数式接口。

  函数式接口|描述
  -|-
  Function|传递一个参数返回一个结果。这个结果的类型可以与参数的类型不相同。
  BiFunction|传递两个参数返回一个结果。这个结果的类型可以与任一参数的类型不相同。
  UnaryOperator|代表一个操作符的操作，它的返回结果类型与操作符的类型一样。实际上它可以被看作是Function 它的返回结果跟参数一样，它是Function 的子接口。
  BiOperator|代表两个操作符的操作，它的返回结果类型必须与操作符相同。
  Predicate|传递一个参数，基于参数的值返回boolean值。
  Supplier|代表一个供应者的结果。
  Consumer|传递一个参数但没有返回值。

1. ### Consumer<T>：消费性接口       传进一个参数 然后对该参数进行操作 没有返回值 操作了也就纯操作
  ```java
  @Test
  public void run() {
      happy(10000, (m) -> System.out.println("消费了" + m + "元"));
  }

  public void happy(double money, Consumer<Double> consumer) {
      consumer.accept(money);
  }
  ```
2. ### Supplier<T>：供给型接口        用于给你产生一些对象 你可以指定你要产生的对象类型
  ```java
  @Test
  public void run2() {
      List<Integer> numList = getNumList(10, () -> (int) (Math.random() * 100));
      for (Integer integer : numList) {
          System.out.println(integer);
      }
  }

  //需求：产生指定个数的整数，并放入集合中
  public List<Integer> getNumList(int num, Supplier<Integer> supplier) {
      List<Integer> list = new ArrayList<>();
      for (int i = 0; i < num; i++) {
          Integer n = supplier.get();
          list.add(n);
      }
      return list;
  }
  ```
3. ### Function<T,R>函数型接口：传进去一个传参数，返回一个参数，可以通过定义泛型对其类型进行操作
  ```java
  @Test
  public void run3(){
      //去除首尾空格
      String s = strHandler("\t\t\t\t 九点三公分课程", (str) -> str.trim());
      System.out.println(s);
      //截取字符串
      String s1 = strHandler("九点三公分课程", (str) -> str.substring(1, 5));
      System.out.println(s1);
  }
  //需求：用于处理字符串
  public String strHandler(String str, Function<String, String> function) {
      return function.apply(str);
  }
  ```
4. ### Predicate<T>: 断言型接口 用于一些做一些判断型操作
  ```java
  @Test
  public void run4(){
      List<String> list = Arrays.asList("action","string","char","shore");
      List<String> strings = filterStr(list, (s) -> s.length() > 5);
      for (String s:strings) {
          System.out.println(s);
      }
  }
  //需求：将满足条件的字符串放入集合中
  public List<String> filterStr(List<String> list, Predicate<String> pre){
      List<String> list1 = new ArrayList<>();
      for (String str:list) {
          if (pre.test(str)){
              list1.add(str);
          }
      }
      return list1;
  }
  ```
  ![](https://img-blog.csdn.net/20180815111535990?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0lUX2xhb2JhaQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
  ![](https://img-blog.csdn.net/20180815111506500?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0lUX2xhb2JhaQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)