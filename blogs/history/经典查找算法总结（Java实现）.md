---
title: 经典查找算法总结（Java实现）
date: 2017-07-10 19:46:21
tags: [Java,算法,Algorithm]
categories: Algorithms
---
经典查找算法主要有顺序查找、二分查找、插值查找、斐波那契查找、二叉查找树等，这里用Java语言实现这些算法。
<!--more-->

# 顺序查找
## 说明
顺序查找适合于存储结构为顺序存储或链接存储的线性表。
## 基本思想
顺序查找也称为线形查找，属于无序查找算法。从数据结构线形表的一端开始，顺序扫描，依次将扫描到的结点关键字与给定值k相比较，若相等则表示查找成功；若扫描结束仍没有找到关键字等于k的结点，表示查找失败。
## 实现源码
```java
public int sequenceSearch(int[] arr, int key){
	for(int i=0;i<arr.length;i++){
		if(arr[i]==key){
			return i;
		}
	}
	return -1;
}
```

# 二分查找
## 说明
元素必须是有序的，如果是无序的则要先进行排序操作。
## 基本思想
也称为是折半查找，属于有序查找算法。用给定值k先与中间结点的关键字比较，中间结点把线形表分成两个子表，若相等则查找成功；若不相等，再根据k与该中间结点关键字的比较结果确定下一步查找哪个子表，这样递归进行，直到查找到或查找结束发现表中没有这样的结点。
## 实现源码
```java
//循环
public int binarySearch(int[] arr, int key){
	int low=0, high=arr.length-1;
	while(low<=high){
		int mid=(low+high)/2;
		if(arr[mid]==key){
			return mid;
		}
		if(arr[mid]<key){
			low=mid+1;
		}
		if(arr[mid]>key){
			high=mid-1;
		}
	}
	return -1;
}
	
//递归
public int binarySearch1(int[] arr, int key, int low, int high){
	if(low>high){
		return -1;
	}

	int mid=(low+high)/2;
	if(arr[mid]==key){
		return mid;
	}
	if(arr[mid]<key){
		return binarySearch1(arr, key, mid+1, high);
	} else {
		return binarySearch1(arr, key, low, mid-1);
	}
}
```

# 插值查找
## 说明
在英文字典里面查“apple”，你下意识翻开字典是翻前面的书页还是后面的书页呢？如果再让你查“zoo”，你又怎么查？很显然，这里你绝对不会是从中间开始查起，而是有一定目的的往前或往后翻。同样的，比如要在取值范围1 ~ 10000 之间 100 个元素从小到大均匀分布的数组中查找5， 我们自然会考虑从数组下标较小的开始查找。
二分查找这种查找方式，不是自适应的。二分查找中查找点计算如下：mid=(low+high)/2, 即mid=low+1/2\*(high-low)。我们可以将查找的点改进为如下：mid=low+(key-a[low])/(a[high]-a[low])\*(high-low)，也就是将上述的比例参数1/2改进为自适应的，根据关键字在整个有序表中所处的位置，让mid值的变化更靠近关键字key，这样也就间接地减少了比较次数。
对于表长较大，而关键字分布又比较均匀的查找表来说，插值查找算法的平均性能比折半查找要好的多。反之，数组中如果分布非常不均匀，那么插值查找未必是很合适的选择。
## 基本思想
基于二分查找算法，将查找点的选择改进为自适应选择，可以提高查找效率。
## 实现源码
```java
//循环
public int insertSearch(int[] arr, int key){
	int low=0, high=arr.length-1;
	while(low<=high){
		int mid=low+(key-arr[low])/(arr[high]-arr[low])*(high-low);
		if(arr[mid]==key){
			return mid;
		}
		if(arr[mid]<key){
			low=mid+1;
		}
		if(arr[mid]>key){
			high=mid-1;
		}
	}
	return -1;
}
	
//递归
public int insertSearch1(int[] arr, int key, int low, int high){
	if(low>high){
			return -1;
	}
		
	int mid=low+(key-arr[low])/(arr[high]-arr[low])*(high-low);
	if(arr[mid]==key){
		return mid;
	}
	if(arr[mid]<key){
		return insertSearch1(arr, key, mid+1, high);
	}else {
		return insertSearch1(arr, key, low, mid-1);
	}
}
```

# 斐波那契查找
## 说明
![经典查找算法总结（Java实现）1](http://ofolh8dcq.bkt.clouddn.com/%E7%BB%8F%E5%85%B8%E6%9F%A5%E6%89%BE%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93%EF%BC%88Java%E5%AE%9E%E7%8E%B0%EF%BC%891.jpg)
## 基本思想
斐波那契查找是根据斐波那契序列的特点对表进行分割。假设表中记录的个数比某个斐波那契数小1，即n=Fn−1，然后将给定值和表中第Fn−1个记录的关键字进行比较。若相等，则查找成功；若给定关键字<表中第Fn−1个记录的关键字，则继续在表中从第一个记录到第Fn−1−1个记录之间查找；若给定关键字>表中第Fn−1个记录的关键字，则继续在自Fn−1+1至Fn−1的子表中查找。 
## 实现源码
```java
//斐波那契数列长度，按使用场景变化
public static final int MAX_SIZE=30;
	
public int fibonacciSearch(int[] arr, int key){
	int len=arr.length;

/*1.构造斐波那契数组
	//构造斐波那契数组
	int[] fib=fibnacci(MAX_SIZE);
	//len位于斐波那契数列的位置
	int k=0;
	while(len>(fib[k]-1)){
		k++;
	}
	//将数组arr扩展到fib[k]-1的长度
	int[] temp=new int[fib[k]-1];
	for(int i=0;i<len;i++){
		temp[i]=arr[i];
	}
	//保持temp数组仍然有序
	for(int i=len;i<fib[k]-1;i++){
		temp[i]=arr[len-1];
	}
*/
		
//2.获取所需斐波那契数
	//确定需要的斐波那契数
	int k=0;
	while(len>getFibnacci(k)-1){
		k++;
	}
	//将数组arr扩展到fib[k]-1的长度
	int[] temp=new int[getFibnacci(k)-1];
	for(int i=0;i<len;i++){
		temp[i]=arr[i];
	}
	//保持temp数组仍然有序
	for(int i=len;i<getFibnacci(k)-1;i++){
		temp[i]=arr[len-1];
	}

	int low=0, high=temp.length-1;
	while(low<=high){
		int mid=low+getFibnacci(k-1);
		if(temp[mid]==key){
			//判断是否是扩展的数值
			if(mid<len){
				return mid;	
			}else {
				return len-1;
			}
		}else
		if(temp[mid]<key){
			low=mid+1;
			k=k-1;
		}else {
			high=mid-1;
			k=k-2;
		}
	}
	return -1;
}
	
//得到第n个斐波那契数
public int getFibnacci(int n){
	int result=0;
	int fib0=0, fib1=1;
	if(n==0){
		return fib0;
	}
	if(n==1){
		return fib1;
	}
	for(int i=2;i<=n;i++){
		result=fib0+fib1;
		fib0=fib1;
		fib1=result;
	}
	return result;
}
	
//构造斐波那契数列
public int[] fibnacci(int n){
	int[] result=new int[n];
	result[0]=0;
	result[1]=1;
	for(int i=2;i<n;i++){
		result[i]=result[i-1]+result[i-2];
	}
	return result;
}
```

# 二叉查找树
## 说明
二叉查找树（又称二叉排序树、二叉搜索树）或者是空树，或者是具有下列性质的一颗树：
- 若他的左子树不为空，则左子树上所有结点的值均小于其根节点的值；
- 若他的右子树不为空，则右子树上所有结点的值均大于其根节点的值；
- 他的左右子树也均是二叉排序树。

二叉查找树性质：对二叉查找树进行中序遍历，即可得到有序的数列。

## 基本思想
二叉查找树是先对待查找的数据进行生成树，确保树的左分支的值小于右分支的值，然后在就行和每个节点的父节点比较大小，查找最适合的范围。 这个算法的查找效率很高，但是如果使用这种查找方法要首先创建树。 

## 实现源码
```java
public class BSTSearch {
	// 递归
	public boolean bstSearch(BSTreeNode root, int key) {
		if (root.data == key) {
			return true;
		} else if (root.data < key) {
			return bstSearch(root.right, key);
		} else {
			return bstSearch(root.left, key);
		}
	}

	// 循环
	public boolean bstSearch1(BSTreeNode root, int key) {
		BSTreeNode node = root;
		while (node != null) {
			if (node.data == key) {
				return true;
			}
			if (node.data < key) {
				node = node.right;
			}
			if (node.data > key) {
				node = node.left;
			}
		}
		return false;
	}
}

class BSTreeNode {
	public int data;
	public BSTreeNode left;
	public BSTreeNode right;

	public BSTreeNode(int data) {
		this.data = data;
	}

	public BSTreeNode(int data, BSTreeNode left, BSTreeNode right) {
		this.data = data;
		this.left = left;
		this.right = right;
	}
}
```


# 总结
复杂度比较：

类别|时间复杂度
----|---------
顺序查找|O(n)
二分查找|O($\log_2{n}$)
插值查找|O($\log_2(\log_2{n})$)
斐波那契查找|O($\log_2{n}$)
二叉查找树|O($\log_2{n}$) ~ O(n)



---------------------------
> [[Data Structure & Algorithm] 七大查找算法](http://www.cnblogs.com/maybe2030/p/4715035.html)
[常见查找算法(Java实现)](http://blog.csdn.net/wqc_csdn/article/details/52691019)

