---
title: 华为的笔试，有点太简单了吧。。。
date: 2017-03-21 16:09:57 +0800 
tag: 实习
---

# 华为的笔试，有点太简单了吧。。。

----------------------


17号晚上7点，进行了人生第一次的笔试。之前还一直在看数据结构和算法的那些内容。好了，到了7点，战战兢兢的打开笔试首页，紧张的看完考试守则，进入了考试。

好紧张，以至于按按钮的鼠标都出了手汗。

然后，一进去，啊，3道编程题。嗯，还好吧，两个小时可能搞的定吧。

好了，看完两道例题，紧张的点入第一题，然后被震惊！

>  请将一段字符串中的小写字母替换成大写字母，并输出。  
>  输入示例：  
>  aBc  
>  输出示例  
>  ABC  

什么鬼！

这也太简单了吧，华为在搞啥？

或许三行代码就能搞定了。

{% highlight java %}
Scanner in = new Scanner(System.in);
String str = in.nextLine();
System.out.println(str.toUpperCase());
{% endhighlight %}

甚至只需要一行

{% highlight java %}
System.out.println(new Scanner(System.in).nextLine().toUpperCase());
{% endhighlight %}

写完之后，还是持怀疑态度，于是按了“保存并调试”。

数秒后

>  恭喜你！你的程序通过了所有用例！

天啊！真的是这样就行了吗？

还以为不能用String类的`toUpperCase()`函数呢。

这真的是华为的笔试吗？

不过可能是第一次笔试的缘故吧，才出得那么简单。

好吧，下一题（不太记得题干了）

>  支付宝集五福活动中，集齐爱国福，和谐福，友善福，富强福，敬业福就能够拿红包。现在以一个五位的字符串表示一个人拿了多少张福卡，例如01011代表拿了3个不同的福卡。现在随机选出不多于10个的人的拿到的福卡的数据，请计算他们最多能集齐多少套福卡。  
>  输入示例：  
>  11001  
>  10011  
>  11011  
>  00111  
>  01100  
>  输出示例：  
>  2

这，还是挺简单的。。。

将5个数分别加起来，然后再取其中的最小值就是了。

代码丢了，不过会一点语言基础也会写吧。

好吧，居然只用了15分钟，就把前两题搞定了。（很慢吧。。。）

那么，终于来到第三题了。看了下，终于有了一点难度。

>  给出一个算式的后缀表示（不是很懂怎么称呼），实现一个函数postfix(String s)，要求输出这条字符串算式的结果。  
>  输入示例：  
>  32+5-  
>  输出示例：  
>  0

可能有人不明白，这是一棵树后续遍历之后得到的字符串，原本的树是这个样子的

![算式树](/assets/pictures/studying/equation_tree.PNG)

原本的算式就是3 + 2 - 5，后序遍历的结果。

我一开始想的方法就是建一棵树，依次从右往左读取，一个字符建一个树的结点，结点有一个`getValue()`方法，可以得到结点的值，值的大小具体看这个节点是运算符还是数字。代码如下：

{% highlight java %}

import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		String shi = in.nextLine();

		//初始化cur变量为最后一个
		cur = shi.length() - 1;
		//建树
		Node node = getTree(shi);
		//输出值
		System.out.println(node.getValue());

	}

	public static int cur = 0;//用来记录当前所指向的字符，用于创建结点的
	public static Node getTree(String tree) {
		if (cur < 0) {
			return null;
		}
		char c = tree.charAt(cur);
		cur--;

		//如果是数字的话，直接返回数字结点就好了
		if (c >= '0' && c <= '9') {
			return new NumNode(c - '0');
		}
		if (c >= 'A' && c <= 'F') {
			return new NumNode(c - 'A' + 10);
		}

		//如果不是数字，则创建运算符结点
		CalNode node;
		switch (c) {
			case '+':
				node = new CalNode(CalNode.ADD);
				break;
			case '-':
				node = new CalNode(CalNode.SUBTRACT);
				break;
			case '*':
				node = new CalNode(CalNode.MULTIPLY);
				break;
			default:
				node = null;
		}

		//递归调用，设置该结点的左右子树。
		node.setRight(getTree(tree));
		node.setLeft(getTree(tree));
		return node;
	}

	public abstract static class Node {
		abstract public int getValue();
	}

	public static class CalNode extends Node {
		public static final int ADD = 1;
		public static final int SUBTRACT = 2;
		public static final int MULTIPLY = 3;
		private int cal;
		private Node left;
		private Node right;

		public void setLeft(Node left) {
			this.left = left;
		}

		public void setRight(Node right) {
			this.right = right;
		}

		public CalNode(int cal) {
			this.cal = cal;
		}

		@Override
		public int getValue() {
			switch (cal) {
				case ADD:
					return left.getValue() + right.getValue();
				case SUBTRACT:
					return left.getValue() - right.getValue();
				case MULTIPLY:
					return left.getValue() * right.getValue();
				default:
					return -1;
			}
		}
	}

	public static class NumNode extends Node {
		int value;

		public NumNode(int value) {
			this.value = value;
		}

		@Override
		public int getValue() {
			return value;
		}
	}
}
{% endhighlight %}

好吧，虽然这个算法可能有点慢，但是也通过了。

好顺利，感觉有点成就感。因为我从来就不是一个很厉害的人，在算法方面。

后面想想，好像可以更进一步的优化。因为计算结果也是后序遍历这棵树，它给的字符串就是后序遍历的。应该能优化一下。

*此处可能有另一个版本的算法*

回到现实，后面还有好多好多的面试呢。这个笔试，只是第一步啊。。。

有点害怕
