---
title: 经典排序算法总结（Java实现）
date: 2017-07-03 19:05:57
tags: [Java,算法,Algorithm]
categories: Algorithms
---
经典排序算法主要包括冒泡排序，插入排序，选择排序，希尔排序，归并排序，快速排序，堆排序等，这里用Java语言实现这些排序算法。
<!--more-->
# 冒泡排序
## 基本思想
两个数比较大小，较大的数下沉，较小的数冒起来。
## 过程
1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
2. 对第0个到第n-1个数据做同样的工作。这时，最大的数就“浮”到了数组最后的位置上。
3. 针对所有的元素重复以上的步骤，除了最后一个。
4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

## 源代码
```java
public void bubbleSort(int[] arr){
	for(int i=0;i<arr.length-1;i++){
		for(int j=i+1;j<arr.length;j++){
			if(arr[i]>arr[j]){
				int temp=arr[i];
				arr[i]=arr[j];
				arr[j]=temp;
			}
		}
	}
}
```
优化后：
```java
public void bubbleSort1(int[] arr){
	int exchange=0;  //记录交换次数
	int k=0;
	while(k<arr.length){
		for(int i=k;i<arr.length-1;i++){
			if(arr[i]>arr[i+1]){
				int temp=arr[i];
				arr[i]=arr[i+1];
				arr[i+1]=temp;
				exchange++;
			}
		}
		if(exchange>0){
			k++;
		}else {
			break;
		}
	}
}
```


# 选择排序
## 基本思想
不断遍历数组找到最小数字交换到数组前面
## 过程
1. 在长度为n的无序数组中，第一次遍历n-1个数，找到最小的数值与第一个元素交换；
2. 第二次遍历n-2个数，找到最小的数值与第二个元素交换；
3. ......
4. 第n-1次遍历，找到最小的数值与第n-1个元素交换，排序完成。

## 源代码
```java
public void selectSort(int[] arr){
	for(int i=0;i<arr.length;i++){
		int minIndex=i;
		int min=arr[i];
		for(int j=i+1;j<arr.length;j++){
			//如果有更小值，更新min和minIndex
			if(arr[j]<min){
				minIndex=j;
				min=arr[j];
			}
		}
		arr[minIndex]=arr[i];
		arr[i]=min;
	}
}
```


# 插入排序
## 基本思想
在要排序的一组数中，假定前n-1个数已经排好序，现在将第n个数插到前面的有序数列中，使得这n个数也是排好顺序的。如此反复循环，直到全部排好顺序。
## 过程
1. 从第一个元素开始，该元素可以认为已经被排序
2. 取出下一个元素，在已经排序的元素序列中从后向前扫描
3. 如果被扫描的元素（已排序）大于新元素，将该元素后移一位
4. 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置
5. 将新元素插入到该位置后
6. 重复步骤2~5

## 源代码
```java
public void insertSort(int[] arr){
	for(int i=1;i<arr.length;i++){
		for(int j=i;j>=0;j--){
			//不断交换至不影响前面已排序数组
			if(arr[j]<arr[j-1]){
				int temp=arr[j-1];
				arr[j-1]=arr[j];
				arr[j]=temp;
			}else {
				break;
			}
		}
	}
}
```


# 希尔排序
## 基本思想
希尔排序，也称递减增量排序算法，实质是分组插入排序。
在要排序的一组数中，根据某一增量分为若干子序列，并对子序列分别进行插入排序。然后逐渐将增量减小,并重复上述过程。直至增量为1,此时数据序列基本有序,最后进行插入排序。
## 过程
![经典排序算法总结（Java实现）1](http://ofolh8dcq.bkt.clouddn.com/%E7%BB%8F%E5%85%B8%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93%EF%BC%88Java%E5%AE%9E%E7%8E%B0%EF%BC%891.png)

## 源代码
```java
public void shellSort(int[] arr){
	if(arr==null||arr.length==0){
		return;
	}

	int incre=arr.length;

	while(true){
		//每次增量减半，直至为1
		incre=incre/2;
		//遍历每个插入排序序列		
		for(int k=0;k<incre;k++){
			//遍历当前插入排序序列每个值			
			for(int i=k+incre;i<arr.length;i=i+incre){
				//从右向左插入排序
				for(int j=i;j>k;j=j-incre){
					if(arr[j-incre]>arr[j]){
						int temp=arr[j-incre];
						arr[j-incre]=arr[j];
						arr[j]=temp;
					}else {
						break;
					}
				}
			}
		}
		if(incre==1){
			break;
		}
	}
}
```


# 快速排序
## 基本思想
分治法思想
## 过程
1. 从数列中挑出一个元素作为基准数；
2. 分区过程，将比基准数大的放到右边，小于或等于它的数都放到左边；
3. 再对左右区间递归执行第二步，直至各区间只有一个数。

## 源代码
```java
public void quickSort(int[] arr, int left, int right){
	if(left>=right){
		return;
	}
	//选第一个数作为基准数key
	int key=arr[left];
	int i=left, j=right;
	while(i<j){
		while(i<j&&arr[j]>=key){	//从右向左找第一个小于key的值
			j--;
		}
		if(i<j){
			arr[i++]=arr[j];
		}
		while(i<j&&arr[i]<key){		//从左向右找第一个大于key的值
		i++;
		}
		if(i<j){
			arr[j--]=arr[i];
		}
	}
	//以上过程数组左边和右边的值交替替换，而key就相当于交换时的临时变量临时存放arr[left]值
	//i==j，将临时变量key的值放到数组arr中
	arr[i]=key;
	quickSort(arr, left, i-1);
	quickSort(arr, i+1, right);
}
```


# 归并排序
## 基本思想
归并排序的思想就是先递归分解数组，再合并数组。
## 过程
1. 先考虑合并两个有序数组，基本思路是比较两个数组的最前面的数，谁小就先取谁，取了后相应的指针就往后移一位。然后再比较，直至一个数组为空，最后把另一个数组的剩余部分复制过来即可。
2. 再考虑递归分解，基本思路是将数组分解成left和right，如果这两个数组内部数据是有序的，那么就可以用上面合并数组的方法将这两个数组合并排序。如何让这两个数组内部是有序的？可以再二分，直至分解出的小组只含有一个元素时为止，此时认为该小组内部已有序。然后合并排序相邻二个小组即可。

## 源代码
```java
public void mergeSort(int[] arr, int first, int last, int[] temp){
	if(first<last){
		int middle=(first+last)/2;
		mergeSort(arr, first, middle, temp);
		mergeSort(arr, middle+1, last, temp);
		mergeArray(arr, first, middle, last, temp);
	}
}
//将arr[first--middle]和arr[middle+1--end]两个有序序列合并
public void mergeArray(int[] arr, int first, int middle, int end, int temp[]){
	int i=first, j=middle+1, k=0;
	while(i<=middle&&j<=end){
		if(arr[i]<arr[j]){
			temp[k++]=arr[i++];
		}else {
			temp[k++]=arr[j++];
		}
	}
	while(i<=middle){
		temp[k++]=arr[i++];
	}
	while(j<=end){
		temp[k++]=arr[j++];
	}
	//将暂存数组里有序的序列复制到arr中
	for(int x=0;x<k;x++){
		arr[first+x]=temp[x];
	}
}
```


# 堆排序
## 基本思想
堆排序是采用二叉堆的数据结构来实现的，虽然实质上还是一维数组。二叉堆是一个近似完全二叉树 。
二叉堆具有以下性质：
1. 父节点的键值总是大于或等于（小于或等于）任何一个子节点的键值。
2. 每个节点的左右子树都是一个二叉堆（都是最大堆或最小堆）。

## 过程
1. 构造最小堆
2. 堆排序：由于堆是用数组模拟的。得到一个小根堆后，数组内部并不是有序的。因此需要将堆化数组有序化。

## 源代码
```java
public void minHeapSort(int[] a, int n){
	int temp=0;
	makeMinHeap(a, n);
	
	for(int i=n-1;i>0;i--){
		temp=a[0];
		a[0]=a[i];
		a[i]=temp;
		minHeapFixDown(a, 0, i);
	}
}

//构建最小堆
public void makeMinHeap(int[] a, int n){
	for(int i=(n-1)/2;i>=0;i--){
		minHeapFixDown(a, i, n);
	}
}

//从i节点开始调整，n为节点总数，从0开始计算，i节点的子节点为2*i+1，2*i+2
public void minHeapFixDown(int[] a, int i, int n){
	int j=2*i+1;	//子节点
	int temp=0;
	while(j<n){
		//在左右子节点中寻找最小的
		if(j+1<n&&a[j+1]<a[j]){
			j++;
		}
		if(a[i]<=a[j]){
			break;
		}
		//较大节点下移
		temp=a[i];
		a[i]=a[j];
		a[j]=temp;
		
		i=j;
		j=2*i+1;
	}
}
```


# 总结
![经典排序算法总结（Java实现）2](http://ofolh8dcq.bkt.clouddn.com/%E7%BB%8F%E5%85%B8%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93%EF%BC%88Java%E5%AE%9E%E7%8E%B0%EF%BC%892.jpg)








-----------
> [排序算法总结|菜鸟教程](http://www.runoob.com/w3cnote/sort-algorithm-summary.html)
[经典排序算法总结与实现](http://wuchong.me/blog/2014/02/09/algorithm-sort-summary/)
