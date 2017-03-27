---
title: 算质数集的算法
date: 2017-03-26 18:39:02 +0800 
tags: 算法,编程
---

给定一个N，如何算这个范围里的所有质数呢？

先看定义：质数是除了1和它本身，没有其他因数的数。

从这个定义下手，很容易就能得到一个简单的算法。要判断一个数n是不是质数，从2开始测试，但是一直测到什么数呢？n/2？可以，因为再升上去，商就比2要小的，而质数需要整除，继续检验就没有意义了。其实范围可以更小，上限可以到n的平方根，因为其实过了n的平方根之后，商就会比n的平方根的数要小，而我们已经测过了这些数，所以不需要继续测试了。这样，一个最简单的算数方法就出来了。

{% highlight java %}
public static List<Integer> calZhiShu1(int N) {
	ArrayList<Integer> zhiList = new ArrayList<>();
	for (int i = 2; i < N; i++) {
		boolean isZhishu = true;//指示如果下面循环j都遍历过了，就是质数
		for (int j = 2; j <= Math.sqrt(i); j++) {
			if (i % j == 0) {
				isZhishu = false;
				break;
			}
		}
		if (isZhishu) {
			zhiList.add(i);
		}
	}
	return zhiList;
}
{% endhighlight %}

这个方法实在是很暴力。不过有更好的。既然我们都已经算出来了一部分质数了，为什么不拿来用呢？一个数如果不是质数的话，因数里面一定有质数，甚至说，这个数可以被分解为多个质数的乘积。ok，如果我要算下一个质数的话，我就从已有的质数列表里面选取候选检验的质数，如果除出来都有没余数，这个数就是质数，添加到这个列表里面。继续找下一个。

噢，漏了一点，从列表中取出来的数，也要符合小于n的平方根。

{% highlight java %}
public static List<Integer> calZhiShu2(int N) {
	ArrayList<Integer> zhiList = new ArrayList<>();
	//先加入初始质数
	zhiList.add(2);
	zhiList.add(3);
	for (int i = 4; i < N; i++) {
		int sqrt = (int) Math.sqrt(i);
		boolean isZhishu = true;
		for (int j = 0; j < zhiList.size(); j++) {
			int z = zhiList.get(j);
			if (z > sqrt)//检验是否大于平方根
				break;

			if (i % z == 0) {
				isZhishu = false;
				break;
			}
		}
		if (isZhishu) {
			zhiList.add(i);
		}
	}
	return zhiList;
}
{% endhighlight %}

其实这是我自己想的算法。

但是没有到极致。数一多，还是要算很久。有没有更牛的算法。

有。我从一个博客那里看到的，但是由于有点久了，找不到网址了。

让我们想想。若一个数是质数，那么它的两倍，三倍甚至更多，都不是质数。我就可以维护一个N这么大的数组，里面存着布尔值。利用数组一开始会全部初始化为false的属性。那么，标记false的地方就是质数，为true的地方不是。比如我从数组第二个元素，对应2的位置开始，这个是false，那么，向前走两步，这个就设为true，继续向前，设为true，直到数组末尾。然后我找下一个false的地方，继续这么走。只需要走到整个数组的一半位置就行了，因为后半位置，加一倍，就已经超出数组范围了。最后就以遍历整个布尔数组，false的地方，把对应的数加入到List里面就行了。

突然间想到一个程序上优化的方法。ArrayList扩展会复制旧的内容到新的地方，如果我能指定初始大小，消灭扩展，就能提升效率。

但是并不是测出来并不是我想的那样，里面加的那个if语句反而拖累了。大家可以去掉注释，然后注释掉另外一个ArrayList语句试试。

{% highlight java %}

public static List<Integer> calZhiShu3(int N) {
	boolean[] num = new boolean[N];
//	int count = 1;//用于计算有多少个非质数，初始化为1是因为不把1算质数
	num[0] = true;//1不算到质数里面
	for (int i = 1; i < N / 2; i++) {
		for (int j = 2 * i + 1; j < N; j += i + 1) {
//			if (!num[j]){
//				//如果原本位置为false才加，避免重复加
//				count++;
//			}
			num[j] = true;
		}
	}

//	ArrayList<Integer> zhiList = new ArrayList<>(N - count);
	ArrayList<Integer> zhiList = new ArrayList<>();
	for (int i = 0; i < N; i++) {
		if (!num[i])
			zhiList.add(i + 1);
	}
	return zhiList;
}
{% endhighlight %}

测试下三个算法的运算速度。看上去时间复杂度都是n平方啊。空间复杂度都是n。

|算法\N|1000000|5000000|10000000|
|----|-----|-----|-----|
|算法1|228|2228|5760|
|算法2|66|484|1181|
|算法3|27|214|737|

算法2对算法1的改进提升很明显，而算法3比算法二多用了一整个N那么大的布尔数组的空间，在N不大的时候，也有可观的提升。

如果有更好的算法，欢迎提出来哦
