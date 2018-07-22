---
title: 一个计算字符串是否由可以移动变成另外一个字符串的算法
date: 2017-03-23 22:38:41 +0800
tags:
  - 算法
  - 字符串
layout: post
---

今天在做牛客网的摸底考试题的时候，遇到一道可能很经典的字符串算法题。

>  如果一个字符串可以通过移动字符的位置获得另外一个字符串，则称这两个字符串属于同一类。现在输入N个字符串，请判断里面有多少类字符串。  
  输入格式：第一行输入N，后面紧接着N行分别是N个字符串。每条字符串不超过50个字符。
  输入示例：  
  4  
  abcd  
  bcda  
  accb  
  abdd  
  输出示例：  
  3

移动一条字符串，并不会改变一条字符串字符的数量。于是，问题就很好解了，算出一个字符串，分别有什么字符，和字符的数量，一条字符串相当于有了自己的一个签名，或者说是散列值。有着相同散列值的字符串就是同一类的字符串了。

{% highlight java %}
import java.util.HashMap;
import java.util.Scanner;

public class Main{
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		int numOfString = in.nextInt();
		in.nextLine();
		String[] strings = new String[numOfString];
		for (int j = 0; j < numOfString; j++) {
			strings[j] = in.nextLine();
		}
		System.out.println(fenlei(strings));
	}

	public static int fenlei(String[] strings) {
		HashMap<Integer, Integer> hashMap = new HashMap<>();
		for (int i = 0; i < strings.length; i++) {
			String s = strings[i];//取出当前字符串
			int[] map = new int[26];//存着26个小写字母频率的数组
			//计算频率
			for (int j = 0; j < strings[i].length(); j++) {
				char c = s.charAt(j);
				map[c - 'a']++;
			}

			//笨方法，重新合成一条字符串，然后用String.hashCode()。真的超级笨的
			StringBuilder builder = new StringBuilder();
			for (int j = 0; j < 26; j++) {
				for (int k = 0; k < map[j]; k++) {
					builder.append(((char) ('a' + j)));
				}
			}
			int hashCode = builder.toString().hashCode();

			//把刚才计算的值放进去HashMap
			if (hashMap.containsKey(hashCode)) {
				Integer remove = hashMap.remove(hashCode);
				hashMap.put(hashCode, remove + 1);
			} else {
				hashMap.put(hashCode, 1);
			}
		}
		return hashMap.size();//这个散列表的大小就是多少类了

	}
{% endhighlight %}

里面用了一个最笨的办法，居然又重新合成一条字符串。。。

我之前看到过一个方法，利用质数来算签名，也就是他这个类的标识符。

因为一个数可以分为多个质数的乘积，也就是质因数分解。质数本身无法被其他数整除。因为一个数的质因数分解是唯一的。因此，质数乘积能够保证唯一，只与这条字符串有关，只要改变一个字符串，乘积就会变。我们赋给一个字符唯一一个对应的质数，比如'a'是2，'b'是3，'c'是5。若有一个字符串是acbb，则对应的乘积`2*5*3*3=90`，有一个字符串是bbca的话，乘积=`3*3*5*2=90`。算出的乘积一样，就是同一类了。

那好，26个小写字母对应的质数依次是，2,3,5,7,11,13,17,19,23,29,31,37,41,43,47,53,59,61,67,71,73,79,83,89,97,101。

说到质数，看到过一篇算N以内质数的算法，有空写。

最后一个居然是101了。恩恩，那么，问题来了。

如果用int型来储存的话，老大难的溢出问题！如果long呢？一样！因为字符串最长50，如果50个z，想想100的50次方是多少。

那么，我用double型？我把所有的质数除以10或者100再算，可能可行吧，还没试过。但是也要注意浮点数的精度问题。字符串长了，运算多了，可能就会失去精度。

因为我想到，对int的乘法，溢出是溢出，但是其实只是没有了高位而已，低位的32个bit仍然保留着原本结果低位的32个bit。int的范围有-2147483648~2147483647这么多的数，遇到散列冲突的问题可能有，但是几率不大。

担心的话，直接上long。甚至BigDecimal。

不过这个算法还是很简陋，不够MD5，SHA-1这种算法冲突少。

给出一个简单的实现。

{% highlight java %}
public static int[] ZHISHU = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101};
public static int fenlei2(String[] strings) {
	HashMap<Integer, Integer> map = new HashMap<>();
	for (int i = 0; i < strings.length; i++) {
		String s = strings[i];
		//一个一个字符，取出来，然后乘以对应的质数
		int sig = ZHISHU[s.charAt(0) - 'a'];
		for (int j = 1; j < s.length(); j++) {
			sig *= ZHISHU[s.charAt(j) - 'a'];
		}
		//放到哈希表里面去，相同签名的会放到同一格，
           //如果已经存在这个签名，取出直接加一存回去，
		//JDK1.8以后可以用replace方法。如果没有这个签名，则新建一个。
		if (map.containsKey(sig)) {
			Integer remove = map.remove(sig);
			map.put(sig, remove + 1);
		} else
			map.put(sig, 1);
	}
	return map.size();
}
{% endhighlight %}

类似问题还有：
+  给出两条字符串，求出其中一条是否包含另外一条的所有字符。
+  字符串旋转问题：给出若干条字符串，其中如果能够通过字符串旋转变成另外一条字符串，则两条字符串为一类，求有多少类字符串

（文中的推理都是基于我的认识而来，并没有做验证。如果有错，欢迎指出）
