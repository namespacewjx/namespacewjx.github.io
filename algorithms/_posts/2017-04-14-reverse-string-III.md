---
title: LeetCode 557，翻转一个字符串里的单词
date: 2017-04-14 11:47:51 +0800
tags: 算法
---

# LeetCode第557题-翻转一个字符串里的单词

[原题链接](https://leetcode.com/problems/reverse-words-in-a-string-iii/)

输入一句话，然后把话里的单词，用空格分开的，翻转过来。例如：
- 输入："This is a demo"
- 输出："sihT si a omed"

挺简单，可以直接用`String.split()`函数分成单词字符串数组以后，一个一个单词反过来，拼接成结果字符串就ok了。代码如下：

{% highlight java %}
public class Solution {
	public String reverseWords(String s) {
		if (s == null || s.length() == 0)
			return "";

		String[] words = s.split(" ");
		StringBuilder builder = new StringBuilder();
		for (int i = 0; i < words.length - 1; i++) {
			builder.append(reverseAWord(words[i]) + " ");
		}
		builder.append(reverseAWord(words[words.length - 1]));

		return builder.toString();
	}

	private String reverseAWord(String s) {
		StringBuilder builder = new StringBuilder(s.length());
		char[] str = s.toCharArray();//转换成char数组，遍历的时候方便
		//从后往前遍历，将后面的字符append到结果中去就行了
		for (int i = str.length - 1; i >= 0; i--) {
			builder.append(str[i]);
		}
		return builder.toString();
	}
}
{% endhighlight %}

这个算法用了26ms完成所有用例。

有没有一次遍历完成的方法呢？其实可以，只需要遇到一个空格，就把前面那个单词翻转过来啊，就ok了。

{% highlight java %}
public class Solution {
	public String reverseWords(String s) {
		char[] str = s.toCharArray();//转换成char数组，方便调换字符位置
		int start = 0;
		for (int i = 1; i < str.length; i++) {
			if (str[i] == ' ') {
				//当遇到一个空格，就翻转前面从start到当前空格位置的单词，不包含当前空格
				reverseAWord(str, start, i);
				start = i + 1;
			}
		}
		reverseAWord(str, start, str.length);
		return new String(str);
	}

	/**
	 * 调换start到end（不包括）之间的字符的顺序
	 * @param str 包含需要调换顺序的单词的字符数组
	 * @param start 开始的第一个，包含在内
	 * @param end 超尾元素，不算在内
	 */
	public void reverseAWord(char[] str, int start, int end) {
		end--;
		while (start < end) {
			char c = str[start];
			str[start] = str[end];
			str[end] = c;
			start++;
			end--;
		}
	}
}
{% endhighlight%}

实测这个算法，结果是10ms。快了很多，因为不需要split一个字符串了，一次遍历完成，也就快了许多。要是不用调换的方法，直接新建一条字符串来存放，有可能还会更快。

其实，我把单词翻转过来，就是从后往前输出字符，是不是后进先出呢？也就是用栈了。

{% highlight java %}
public class Solution {
	public String reverseWords(String s) {
		Stack<Character> stack = new Stack<>();
		StringBuilder builder = new StringBuilder(s.length());
		for (int i = 0; i < s.length(); i++) {
			char c = s.charAt(i);
			if (c != ' ') {
				stack.push(c);
			} else {
				while (!stack.isEmpty())
					builder.append(stack.pop());
				builder.append(' ');
			}
		}
		while (!stack.isEmpty())
			builder.append(stack.pop());
		return builder.toString();
	}
}
{% endhighlight %}

但是真的很慢，76ms哦。不知道自己实现一个简单的栈会怎么样。

其实这道题挺简单，主要是为了说明，其实一道题有很多种解法，一步一步的优化，减少不必要的步骤，达到简单而高效，是我们的一直的追求啊。
