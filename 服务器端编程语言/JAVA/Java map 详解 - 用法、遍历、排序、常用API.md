> java.util 中的集合类包含 Java 中某些最常用的类。最常用的集合类是 List 和 Map。
> Map 提供了一个更通用的元素存储方法。Map 集合类用于存储元素对（称作“键”和“值”），其中每个键映射到一个值。
> 本文主要介绍java map的初始化、用法、map的四种常用的遍历方式、map的排序以及常用api。

* ### Map 用法
  * 类型介绍
    > Java 自带了各种 Map 类。这些 Map 类可归为三种类型：
    1. 通用`Map`，用于在应用程序中管理映射，通常在 `java.util` 程序包中实现`HashMap`、`Hashtable`、`Properties`、`LinkedHashMap`、`IdentityHashMap`、`TreeMap`、`WeakHashMap`、`ConcurrentHashMap`
    2. 专用`Map`，通常我们不必亲自创建此类Map，而是通过某些其他类对其进行访问`java.util.jar.Attributes`、`javax.print.attribute.standard.PrinterStateReasons`、`java.security.Provider`、`java.awt.RenderingHints`、`javax.swing.UIDefaults`
    3. 一个用于帮助我们实现自己的Map类的抽象类`AbstractMap`
  * 类型区别
    * `HashMap`
      > 最常用的`Map`,它根据键的`HashCode`值存储数据,根据键可以直接获取它的值，具有很快的访问速度。`HashMap`最多只允许一条记录的键为Null(多条会覆盖);允许多条记录的值为Null。非同步的。
    * `TreeMap`
      > 能够把它保存的记录根据键(`key`)排序,默认是按升序排序，也可以指定排序的比较器，当用`Iterator` 遍历`TreeMap`时，得到的记录是排过序的。`TreeMap`不允许`key`的值为`null`。非同步的。 
    * `Hashtable`
      > 与 `HashMap`类似,不同的是:`key`和`value`的值均不允许为`null`;它支持线程的同步，即任一时刻只有一个线程能写`Hashtable`,因此也导致了`Hashtale`在写入时会比较慢。
    * `LinkedHashMap`
      > 保存了记录的插入顺序，在用`Iterator`遍历`LinkedHashMap`时，先得到的记录肯定是先插入的.在遍历的时候会比`HashMap`慢。`key`和`value`均允许为空，非同步的。
  * Map 初始化
    ```java
    Map<String, String> map = new HashMap<String, String>();
    ```
  * 插入元素
    ```java
    map.put("key1", "value1");
    ```
  * 获取元素
    ```java
    map.get("key1")
    ```
  * 移除元素
    ```java
    map.remove("key1");
    ```
  * 清空map
    ```java
    map.clear();
    ```
* ### Map 遍历
  * 初始化数据
    ```java
    Map<String, String> map = new HashMap<String, String>();
    map.put("key1", "value1");
    map.put("key2", "value2");
    ```
  * 增强for循环遍历
    * 使用keySet()遍历
      ```java
      for (String key : map.keySet()) {
        System.out.println(key + " ：" + map.get(key));
      }
      ```
    * 使用entrySet()遍历
      ```java
      for (Map.Entry<String, String> entry : map.entrySet()) {
        System.out.println(entry.getKey() + " ：" + entry.getValue());
      }
      ```
  * 迭代器遍历
    * 使用keySet()遍历
      ```java
      Iterator<String> iterator = map.keySet().iterator();
      while (iterator.hasNext()) {
        String key = iterator.next();
        System.out.println(key + "　：" + map.get(key));
      }
      ```
    * 使用entrySet()遍历
      ```java
      Iterator<Map.Entry<String, String>> iterator = map.entrySet().iterator();
      while (iterator.hasNext()) {
        Map.Entry<String, String> entry = iterator.next();
        System.out.println(entry.getKey() + "　：" + entry.getValue());
      }
      ```
* ### Map 排序
  * HashMap、Hashtable、LinkedHashMap排序
    > TreeMap也可以使用此方法进行排序，但是更推荐下面的方法。
    ```java
    Map<String, String> map = new HashMap<String, String>();
    map.put("a", "c");
    map.put("b", "b");
    map.put("c", "a");
    
    // 通过ArrayList构造函数把map.entrySet()转换成list
    List<Map.Entry<String, String>> list = new ArrayList<Map.Entry<String, String>>(map.entrySet());
    // 通过比较器实现比较排序
    Collections.sort(list, new Comparator<Map.Entry<String, String>>() {
      public int compare(Map.Entry<String, String> mapping1, Map.Entry<String, String> mapping2) {
        return mapping1.getKey().compareTo(mapping2.getKey());
      }
    });
    for (Map.Entry<String, String> mapping : list) {
      System.out.println(mapping.getKey() + " ：" + mapping.getValue());
    }
    ```
  * TreeMap 排序
    > TreeMap默认按key进行升序排序，如果想改变默认的顺序，可以使用比较器
      ```java
      Map<String, String> map = new TreeMap<String, String>(new Comparator<String>() {
        public int compare(String obj1, String obj2) {
          return obj2.compareTo(obj1);// 降序排序
        }
      });
      map.put("a", "c");
      map.put("b", "b");
      map.put("c", "a");
      for (String key : map.keySet()) {
        System.out.println(key + " ：" + map.get(key));
      }
      ```
  * 按value排序(通用)
      ```java
      Map<String, String> map = new TreeMap<String, String>();
      map.put("a", "c");
      map.put("b", "b");
      map.put("c", "a");
 
      // 通过ArrayList构造函数把map.entrySet()转换成list
      List<Map.Entry<String, String>> list = new ArrayList<Map.Entry<String, String>>(map.entrySet());
      // 通过比较器实现比较排序
      Collections.sort(list, new Comparator<Map.Entry<String, String>>() {
          public int compare(Map.Entry<String, String> mapping1, Map.Entry<String, String> mapping2) {
              return mapping1.getValue().compareTo(mapping2.getValue());
          }
      });
 
      for (String key : map.keySet()) {
          System.out.println(key + " ：" + map.get(key));
      }
      ```
* ### 常用API
方法|说明
-|-
clear()|从 Map 中删除所有映射
remove(Object key)|从 Map 中删除键和关联的值
put(Object key, Object value)|将指定值与指定键相关联
putAll(Map t)|将指定 Map 中的所有映射复制到此 map
entrySet()|返回 Map 中所包含映射的 Set 视图。Set 中的每个元素都是一个 Map.Entry 对象，可以使用 getKey() 和 getValue() 方法（还有一个 setValue() 方法）访问后者的键元素和值元素
keySet()|返回 Map 中所包含键的 Set 视图。删除 Set 中的元素还将删除 Map 中相应的映射（键和值）
values()|返回 map 中所包含值的 Collection 视图。删除 Collection 中的元素还将删除 Map 中相应的映射（键和值）
get(Object key)|返回与指定键关联的值
containsKey(Object key)|如果 Map 包含指定键的映射，则返回 true
containsValue(Object value)|如果此 Map 将一个或多个键映射到指定值，则返回 true
isEmpty()|如果 Map 不包含键-值映射，则返回 true
size()|返回 Map 中的键-值映射的数目