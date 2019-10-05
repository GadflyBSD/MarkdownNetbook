```java
import java.util.Arrays;

public class ArrayUtil {
	private static final ArrayUtil instance = new ArrayUtil();

	/**
	 * 对指定的 int 型数组按数字升序进行冒泡排序
	 * @param a 待排序的数组
	 */
	public static ArrayUtil sortBubble(int[] a) {
		int temp;
		for (int i = 1; i < a.length; i++) {
			for (int j = 0; j < a.length - i; j++) {
				if (a[j] > a[j+1]) {
					temp = a[j];//三步交换
					a[j] = a[j+1];
					a[j+1] = temp;
				}
			}
		}
		return instance;
	}

	/**
	 * 对指定的 int 型数组按数字升序进行选择排序
	 * @param a 待排序的数组
	 */
	public static ArrayUtil sortSelection(int[] a) {
		int flag;//标记最小值得位置
		int temp;//用于交换的中间变量
		for (int i = 0; i < a.length; i++) {
			flag = i;
			for (int j = i; j < a.length; j++) {
				if (a[j] < a[flag]) {
					flag = j;
				}
			}
			temp = a[i];//三步交换
			a[i] = a[flag];
			a[flag] = temp;
		}
		return instance;
	}

	/**
	 * 对指定的 int 型数组按数字升序进行插入排序
	 * @param a 待排序的数组
	 */
	public static ArrayUtil sortInsertion(int[] a) {
		int flag = 0;//标记需要插入的位置
		int temp = 0;//存储待插入的数
		for (int i = 1; i < a.length; i++) {
			temp = a[i];
			for (int j = i; j > 0; j--) {
				if (a[j-1] > temp) {
					a[j] = a[j-1];
					flag = j-1;
				}else {
					flag = j;
					break;
				}
			}
			a[flag] = temp;
		}
		return instance;
	}

	/**
	 * 使用线性搜索法来搜索指定的 int 型数组中搜索指定的值，以获得该的值的位置
	 * @param a 要搜索的数组
	 * @param key 要搜索的值
	 * @return -如果这个值被找到，则返回该值得索引，否则返回 -1<br>
	 *         -如果数组中有多个可以匹配的值，则返回第一的值的索引<br>
	 *         -调用该方法之前不需要对数组进行排序
	 */
	public static int searchLinear(int[] a,int key) {
		for (int i = 0; i < a.length; i++) {
			if (a[i] == key) {
				return i;
			}
		}
		return -1;
	}

	/**
	 * 使用二分查找来搜索指定的 int 型数组中搜索指定的值，以获得该的值的位置
	 * @param a 要搜索的数组
	 * @param key 要搜索的值
	 * @return -如果这个值被找到，则返回该值得索引，否则返回 -1<br>
	 *         -如果数组包含多个等于指定对象的元素，则无法保证找到的是哪一个<br>
	 *         -在进行此调用之前，必须根据元素的自然顺序对数组进行升序排序
	 *         -如果没有对数组进行排序，则结果是不确定的
	 */
	public static int searchBinary(int[] a,int key) {
		int minIndex = 0;//首位置
		int maxIndex = a.length - 1;//末位置
		int middleIndex;//中间位置
		while (maxIndex >= minIndex) {
			middleIndex = (minIndex + maxIndex)/2;
			if (a[middleIndex] == key) {
				return middleIndex;
			}else if (a[middleIndex] > key) {
				maxIndex = middleIndex - 1;
			}else if (a[middleIndex] < key){
				minIndex = middleIndex + 1;
			}
		}
		return -1;
	}

	/**
	 * 插入--将一个int型数据插入到指定数组的指定位置
	 * @param arr 待插入的数组
	 * @param k 要插入的位置--从0开始算
	 * @param a 要插入的数据
	 * @return -返回插入后的新数组<br>
	 *             -返回的是一个新数组，原待插入数组不变
	 */
	public static int[] insert(int[] arr,int k,int a) {
		int[] brr = Arrays.copyOf(arr, arr.length+1);
		int flag = brr.length - 1;
		for (int i = brr.length - 2; i >= k; i--) {
			brr[i + 1] = brr[i];
			flag = i;
		}
		brr[flag] = a;
		return brr;
	}

	/**
	 * 从指定数组中删除指定的数  若没有找到要删除的数，则返回原数组
	 * @param arr 待处理的数组
	 * @param num 要删除的数
	 * @return -返回处理后的新数组
	 *             -原数组不会被改变
	 */
	public static int[] delete(int[] arr,int num) {
		int flag = searchLinear(arr, num);//要删除值得索引
		if (flag == -1) {//没有找到删除点
			return arr;
		}
		int[] brr = new int[arr.length - 1];
		for (int i = 0; i < flag; i++) {
			brr[i] = arr[i];
		}
		for (int i = flag; i < brr.length; i++) {
			brr[i] = arr[i + 1];
		}
		return brr;
	}

	/**
	 * 对int型数组进行倒叙操作
	 * 该方法直接对原数组进行操作，不会返回新的数组
	 * @param arr 待反转的数组
	 */
	public static ArrayUtil reversal(int[] arr) {
		int temp;
		for (int i = 0; i < arr.length/2; i++) {
			temp = arr[i];
			arr[i] = arr[arr.length-i-1];
			arr[arr.length-i-1] = temp;
		}
		return instance;
	}

	/**
	 * 对指定的 String 型数组按ASCII升序进行排序
	 * @param str -要排序的数组
	 */
	public static ArrayUtil sort(String[] str) {
		int flag = 0;//标记需要插入的位置
		String temp = "";//存储待插入的字符串
		for (int i = 1; i < str.length; i++) {
			temp = str[i];
			for (int j = i; j > 0; j--) {
				if (str[j-1].compareTo(temp) > 0) {
					str[j] = str[j-1];
					flag = j-1;
				}else {
					flag = j;
					break;
				}
			}
			str[flag] = temp;
		}
		return instance;
	}
}
```