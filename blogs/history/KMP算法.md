---
title: KMP算法
date: 2017-08-08 20:54:24
tags: [Algorithm, Java]
categories: Algorithms
---
字符串匹配是字符串的一种基本操作：给定一个长度为 M 的文本和一个长度为 N 的模式串，在文本中找到一个和该模式相符的子字符串，并返回该字字符串在文本中的位置。字符串匹配常见的两种算法：暴力匹配算法（BF算法）和Knuth-Morris-Pratt算法（KMP算法）。
<!--more-->
# BF算法
## 基本思想
从主串的起始位置（或指定位置）开始与模式串的第一个字符比较，若相等，则继续逐个比较后续字符；否则从主串的下一个字符再重新和模式串的字符比较。依次类推，直到模式串成功匹配，返回主串中第一次出现模式串字符的位置，或者模式串匹配不成功，这里约定返回-1；

## 代码实现
```java
	/**
	 * 暴力匹配
	 * @param s	源串
	 * @param p	模式串
	 * @return
	 */
	public int bruteForce(String s, String p){
		int sLen=s.length();
		int pLen=p.length();
		for(int i=0;i<=sLen-pLen;i++){
			int j=0;
			for(j=0;j<pLen;j++){
				if(s.charAt(i+j)!=p.charAt(j)){
					break;
				}
			}
			if(j==pLen){
				return i;
			}
		}
		return -1;
	}
```
## 复杂度
时间复杂度为O(n*m)，n为源串长度，m为模式串长度。

**BF算法的不足：**
![KMP算法1](http://ofolh8dcq.bkt.clouddn.com/KMP%E7%AE%97%E6%B3%951.png)
![KMP算法2](http://ofolh8dcq.bkt.clouddn.com/KMP%E7%AE%97%E6%B3%952.png)

# KMP算法
## 基本思想
KMP算法的想法是，设法利用已知的匹配信息，改进暴力匹配中i回溯的操作。KMP算法中当一趟匹配过程中出现字符比较不等时，不直接回溯i，而是利用已经得到的“部分匹配”的结果将模式串向右移动（j-next[k]）的距离。

"前缀"和"后缀"： "前缀"指除了最后一个字符以外，一个字符串的全部头部组合；"后缀"指除了第一个字符以外，一个字符串的全部尾部组合。
![KMP算法3](http://ofolh8dcq.bkt.clouddn.com/KMP%E7%AE%97%E6%B3%953.png)

**next数组**
![KMP算法4](http://ofolh8dcq.bkt.clouddn.com/KMP%E7%AE%97%E6%B3%954.png)

## 代码实现
```java
public int[] getNext(String p){
    int len=p.length();
    int[] next=new int[len];
    next[0]=-1;
    int k=-1;
    int j=0;
    while(j<len-1){
        if(k==-1||p.charAt(k)==p.charAt(j)){
            k++;
            j++;
            next[j]=k;
        }else{
            k=next[k];
        }
    }
    return next;
}
public int kmp(String s, String p){
    int i=0;
    int j=0;
    int sLen=s.length();
    int pLen=p.length();
    int[] next=getNext(p);
    while(i<sLen && j<pLen){
        if(j==-1||s.charAt(i)==p.charAt(j)){
            i++;
            j++;
        }else{
            j=next[j];
        }
    }
    if(j==pLen){
        return i-pLen;
    }else{
        return -1;
    }
}
```
## 复杂度
时间复杂度为O(m+n)。预处理时间复杂度是O(m)，匹配时间复杂度是O(n)，m为源串（或文本串）长度，n为模式串长度。
