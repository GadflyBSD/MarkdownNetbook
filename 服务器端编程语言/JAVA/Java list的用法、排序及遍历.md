> List中可以添加任何对象，包括自己定义的新的类
> List的常用实现类有：ArrayList和LinkedList。

- ### 用法
  - ArrayList
    > ArrayList类实现一个可增长的动态数组
    ```java
    public static void main(String[] args) {
      List<String> list = new ArrayList<String>();
      // 插入元素
      list.add("list1");
      list.add("list2");
      // 打印list的大小
      System.out.println(list.size());
      // 按索引移除元素
      list.remove(0);
      // 按对象移除元素
      list.remove("list2");
      // 打印list的大小
      System.out.println(list.size());
      // 清空list
      list.clear();
    }
    ```
  - LinkedList
    > LinkedList类实现了链表，可初始化化为空或者已存在的集合
    ```java
    public static void main(String[] args) {
      LinkedList<String> list = new LinkedList<String>();
      // 插入元素
      list.add("list2");
      list.add("list3");
      // 向链表头插入数据
      list.addFirst("list1");
      // 向链表尾插入数据
      list.addLast("list4");
      for (String str : list) {
        System.out.println(str);
      }
      // 获取链表头数据
      System.out.println("链表头数据:" + list.getFirst());
      // 获取链表尾数据
      System.out.println("链表尾数据:" + list.getLast());
    }
    ```
- ### 排序
  - 数字排序
    ```java
    public static void main(String[] args) {
      // 创建list
      List<Integer> list = new ArrayList<Integer>();
      // 插入元素
      list.add(2);
      list.add(0);
      list.add(3);
      list.add(4);
      list.add(1);
      Collections.sort(list);
      for (int i : list) {
        System.out.println(i);
      }
    }
    ```
  - 中文排序
    ```java
    public static void main(String[] args) {
      ArrayList<String> list = new ArrayList<String>();
      list.add("一鸣惊人-Y");
      list.add("人山人海-R");
      list.add("海阔天空-H");
      list.add("空前绝后-K");
      list.add("后来居上-H");
      Comparator<Object> cmp = Collator.getInstance(java.util.Locale.CHINA);
      Collections.sort(list, cmp);
      for (String str : list) {
        System.out.println(str);
      }
    }
    ```
  - 实体类排序
    ```java
    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.Comparator;
    import java.util.List;
    public class TextList {
        public static void main(String[] args) {
            List<User> userlist = new ArrayList<User>();
            userlist.add(new User("Y - 易小星 ", 33));
            userlist.add(new User("W - 王大锤", 33));
            Comparator<User> cmp = new ComparatorUser();
            Collections.sort(userlist, cmp);
            for (User user : userlist) {
                System.out.println(user.getName());
            }
        }
    }
    class ComparatorUser implements Comparator<User> {
        @Override
        public int compare(User u1, User u2) {
            // 先按年龄排序
            int flag = u1.getAge().compareTo(u2.getAge());
            // 年龄相等比较姓名
            if (flag == 0) {
                return u1.getName().compareTo(u2.getName());
            } else {
                return flag;
            }
        }
    }
    class User {
        private String name;
        private Integer age;
        public User() {
            super();
        }
        public User(String name, Integer age) {
            super();
            this.name = name;
            this.age = age;
        }
        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
        public Integer getAge() {
            return age;
        }
        public void setAge(Integer age) {
            this.age = age;
        }
    }
    ```
- ### 遍历
  - 三种遍历方法
    ```java
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        // 插入元素
        list.add("list1");
        list.add("list2");
        list.add("list3");
        System.out.println("第一种遍历方法 - >");
        for (String str : list) {
            System.out.println(str);
        }
        System.out.println("第二种遍历方法 - >");
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
        System.out.println("第三种遍历方法 - >");
        Iterator<String> iter = list.iterator();
        while (iter.hasNext()) {
            System.out.println(iter.next());
        }
    }
    ```
  - 遍历时移除元素
    ```java
    import java.util.ArrayList;
    import java.util.Iterator;
    import java.util.List;
    public class RemoveItemFromList {
        public static void main(String[] args) {
            List<String> list = new ArrayList<String>();
            // 插入元素
            list.add("list1");
            list.add("list2");
            list.add("list2");
            list.add("list3");
            // 实例化新的list防止因传递地址而达不到测试效果。
            remove1(new ArrayList<String>(list));
            remove2(new ArrayList<String>(list));
            remove2_1(new ArrayList<String>(list));
            remove3(new ArrayList<String>(list));
        }
        public static void remove1(List<String> list) {
            System.out.print("第一种方法 - > ");
            try {
                for (String str : list) {
                    if (str.equals("list2"))
                        list.remove(str);
                }
            } catch (Exception e) {
                System.out.println("移除失败!");
            }
        }
        public static void remove2(List<String> list) {
            System.out.print("第二种方法 - > ");
            for (int i = 0; i < list.size(); i++) {
                String str = list.get(i);
                if (str.equals("list2"))
                    list.remove(str);
            }
            System.out.println(list);
            System.out.println("也有异常，可以用下面的方法避免。");
        }
        public static void remove2_1(List<String> list) {
            System.out.print("第二种方法修正 - > ");
            for (int i = 0; i < list.size(); i++) {
                String str = list.get(i);
                if (str.equals("list2")) {
                    list.remove(str);
                    // 因移除了元素,位置发生偏移,需要重新对当前位置的元素进行判断。
                    i--;
                }
            }
            System.out.println(list);
        }
        public static void remove3(List<String> list) {
            System.out.print("第三种方法 - > ");
            Iterator<String> iter = list.iterator();
            while (iter.hasNext()) {
                String str = iter.next();
                if (str.equals("list2"))
                    iter.remove();
            }
            System.out.println(list);
        }
    }
    ```
- ### 按批次处理list数据 (list按条数取)
  > 主要应用于list存储数据过多，不能使list整体进行其余操作
    ```java
    import java.util.ArrayList;
    import java.util.List;
    /**
     * 按批次处理list数据的两种方法
     * 主要应用于list存储数据过多，不能使list整体进行其余操作
     * @author zhouhongna
     * @date 2014-10-20
     *
     */
    public class DealListByBatch {
         
        /**
         * 通过list的     subList(int fromIndex, int toIndex)方法实现
         * @param sourList 源list
         * @param batchCount 分组条数
         */
        public static void dealBySubList(List<Object> sourList, int batchCount){
            int sourListSize = sourList.size();
            int subCount = sourListSize%batchCount==0 ? sourListSize/batchCount : sourListSize/batchCount+1;
            int startIndext = 0;
            int stopIndext = 0;
            for(int i=0;i<subCount;i++){
                stopIndext = (i==subCount-1) ? stopIndext + sourListSize%batchCount : stopIndext + batchCount;
                List<Object> tempList = new ArrayList<Object>(sourList.subList(startIndext, stopIndext)); 
                printList(tempList);
                startIndext = stopIndext;
            }
        }
         
        /**
         * 通过源list数据的逐条转移实现
         * @param sourList 源list
         * @param batchCount 分组条数
         */
        public static void dealByRemove(List<Object> sourList, int batchCount){
            List<Object> tempList = new ArrayList<Object>();
            for (int i = 0; i < sourList.size(); i++) {  
                tempList.add(sourList.get(i));
                if((i+1)%batchCount==0 || (i+1)==sourList.size()){
                    printList(tempList);
                    tempList.clear();
                }
            }
        }
         
        /**
         * 打印方法 充当list每批次数据的处理方法
         * @param sourList
         */
        public static void printList(List<Object> sourList){
            for(int j=0;j<sourList.size();j++){
                System.out.println(sourList.get(j));
            }
            System.out.println("------------------------");
        }
         
        /**
         * 测试主方法
         * @param args
         */
        public static void main(String[] args) {
            List<Object> list = new ArrayList<Object>();  
            for (int i = 0; i < 91; i++) {  
                list.add(i);  
            }  
            long start = System.nanoTime();
            dealBySubList(list, 10);
            dealByRemove(list, 10);
            long end = System.nanoTime();
            System.out.println("The elapsed time :" + (end-start));
             
        }
    }
    ```
- ### List接口的扩展方法
  1. public void add(int index,E element)普通  在指定的位置增加元素。
  2. public boolean addAll(int index, Collection<? extends E> c)  普通 在指定的位置增加一组元素。
  3. E get(int index) 普通 返回指定位置的元素。
  4. public int indexOf(Object o) 普通 查找指定元素的位置。
  5. public int lastIndexOf(Object o) 普通 从后向前查找指定元素的位置。
  6. public ListIterator<E> listIterator()普通 为ListIterator接口实例化。
  7. public E remove(int index) 普通 按指定的位置删除元素。
  8. public List<E> subList(int fromIndex, int toIndex) 普通 取出集合中的子集合。
  9. public E set(int index, E element)普通 替换指定位置的元素。