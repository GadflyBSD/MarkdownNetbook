# String和各种格式互转 String转int int转String
  *  ## 其他类型转String
      ```java
      String s = String.valueOf( value); // 其中 value 为任意一种数字类型。
      ```
  
  * ## 字符串型转换成各种数字类型
    ```java
    String s = "169"; 
    byte b = Byte.parseByte( s ); 
    short t = Short.parseShort( s ); 
    int i = Integer.parseInt( s ); 
    long l = Long.parseLong( s ); 
    Float f = Float.parseFloat( s ); 
    Double d = Double.parseDouble( s );
    ```
    
  * ## 字符串转数组
    > split() 方法根据匹配给定的正则表达式来拆分字符串。
    ```java
    // 字符串转数组  java.lang.String
    String str = "0,1,2,3,4,5";
    String[] arr = str.split(","); // 用,分割
    System.out.println(Arrays.toString(arr)); // [0, 1, 2, 3, 4, 5]
    ```
  * ## 数组转字符串
    * ### 方法一：遍历
      ```java
      String[] arr = { "0", "1", "2", "3", "4", "5" };
      // 遍历
      StringBuffer str5 = new StringBuffer();
      for (String s : arr) {
          str5.append(s);
      }
      System.out.println(str5.toString()); // 012345
      ```
    * ### 方法二：使用StringUtils的join方法
      ```java
      //数组转字符串 org.apache.commons.lang3.StringUtils
      String str3 = StringUtils.join(arr); // 数组转字符串,其实使用的也是遍历
      System.out.println(str3); // 012345
      String str4 = StringUtils.join(arr, ","); // 数组转字符串(逗号分隔)(推荐)
      System.out.println(str4); // 0,1,2,3,4,5
      ```
    * ### 方法三：使用ArrayUtils的toString方法
      ```java
      // 数组转字符串 org.apache.commons.lang3.ArrayUtils
      String str2 = ArrayUtils.toString(arr, ","); // 数组转字符串(逗号分隔,首尾加大括号)
      System.out.println(str2); // {0,1,2,3,4,5}
      ```

# 使用map装入多种类型的值
  > 可以使用多种类型的父类,来指定值的类型.比如Object是其他类的父类.
  > `HashMap<Object,Object> map` 这里键和值都可以存储多种类型
  
```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map.Entry;
 
public class Demo {
    public static void main(String[] args) {
        HashMap<Object,Object> map = new HashMap<Object,Object>();
        map.put(1,"三国");//值是字符串
        map.put("数组",new int[]{1,2,3});//值是数组
        map.put(null, null);//值是null
        map.put(map,map);//值是map自己
        map.put('A',2.8 );//值是浮点数
         
        Iterator<Entry<Object,Object>> it = map.entrySet().iterator();
        while(it.hasNext()){
            Entry<Object,Object> e = it.next();
            System.out.println(e.getKey()+","+e.getValue());
        }
    }
} 
```

# 毫秒转成时分秒
```java
/**
 * 把毫秒数转换成时分秒
 * @param millis
 * @return
 */
public static String millisToStringShort(long millis) {
	StrBuilder strBuilder = new StrBuilder();
	long temp = millis;
	long hper = 60 * 60 * 1000;
	long mper = 60 * 1000;
	long sper = 1000;
	if (temp / hper > 0) {
		strBuilder.append(temp / hper).append("小时");
	}
	temp = temp % hper;
	if (temp / mper > 0) {
		strBuilder.append(temp / mper).append("分钟");
	}
	temp = temp % mper;
	if (temp / sper > 0) {
		strBuilder.append(temp / sper).append("秒");
	}
	return strBuilder.toString();
}
```

# 根据毫秒数转换为时分秒（格式为00:00:00）
```java
private static String formatLongToTimeStr(Long l) {
  int hour = 0;
  int minute = 0;
  int second = 0;
  second = l.intValue() / 1000;
  if (second > 60) {
    minute = second / 60;
    second = second % 60;
  }
  if (minute > 60) {
    hour = minute / 60;
    minute = minute % 60;
  }
  return (getTwoLength(hour) + ":" + getTwoLength(minute) + ":" + getTwoLength(second));
}

private static String getTwoLength(final int data) {
  if(data < 10) {
    return "0" + data;
  } else {
    return "" + data;
  }
}
```