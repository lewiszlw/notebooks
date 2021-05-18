# B 树 (B-tree) 和 B+ 树
**B树（B-tree）**

3阶B树
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/storage/B%E6%A0%91.png)

M阶B树定义
1. 定义任意非叶子结点最多只有M个儿子，且M>2；
2. 根结点的儿子数为[2, M]；
3. 除根结点以外的非叶子结点的儿子数为[M/2, M]；
4. 每个结点存放至少M/2-1（取上整）和至多M-1个关键字；（至少2个关键字）
5. 非叶子结点的关键字个数=指向儿子的指针个数-1；
6. 非叶子结点的关键字：K[1], K[2], …, K[M-1]；且K[i] < K[i+1]；
7. 非叶子结点的指针：P[1], P[2], …, P[M]；其中P[1]指向关键字小于K[1]的子树，P[M]指向关键字大于K[M-1]的子树，其它P[i]指向关键字属于(K[i-1], K[i])的子树；
8. 所有叶子结点位于同一层；

特点
1. 关键字集合分布在整颗树上
2. 每一个关键字有且只出现一次
3. 所有关键字按照从小到大的顺序进行排列
4. 每个节点除了存储关键字，还存储数据
5. 若经常访问的元素离根节点较近，则访问更加迅速
6. 叶子节点存储在同一层
7. 相当于二分查找，可以在非叶节点结束

**B+树**

3阶B+树
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/storage/B%2B%E6%A0%91.png)

m阶的B+树定义如下:
1. 每个结点至多有m个子女；
2. 除根结点外，每个结点至少有 m/2（向上取整）个子女，根结点（非叶节点）至少有两个子女；
3. 有k个子女的结点必有k个关键字；
4. 叶节点的高度一致；

特点（MySQL Innodb 引擎实现为例）
1. 有n棵子树的非叶节点有n个关键字，关键字会存储重复。
2. 非叶节点只保存关键字，仅包含子树的最大或者最小的关键字，只用来索引，关键字从小到大排列
4. 所有叶子节点包含全部的关键字信息，以及指向关键字记录的指针
5. 叶子节点构成链表
6. 有两个指针，一个指向根节点，一个指向关键字最小的叶子节点
7. 非叶子节点的子树指针p[i],指向关键字值属于[k[i],k[i+1]]的子树.(B树是开区间,也就是说B树不允许关键字重复,B+树允许重复)；
8. 所有关键字都在叶子节点出现(稠密索引). (且链表中的关键字恰好是有序的)；
9. 非叶子节点相当于是叶子节点的索引(稀疏索引),叶子节点相当于是存储(关键字)数据的数据层；

**什么是B树的阶？**
B树中一个节点的子节点数目的最大值。

**B树和B+树对比**：
1. B树在提高了IO性能的同时并没有解决元素遍历的我效率低下的问题，正是为了解决这个问题，B+树应用而生。B+树只需要去遍历叶子节点就可以实现整棵树的遍历。而且在数据库中基于范围的查询是非常频繁的，而B树不支持这样的操作或者说效率太低。
2. B+树更适合外部存储，由于内节点无 data 域，一个结点可以存储更多的内结点，每个节点能索引的范围更大更精确，也意味着 B+树单次磁盘IO的信息量大于B树，I/O效率更高。
3. B+树的查找路径是由根到叶子节点，每次查找路径长度比较稳定，查询时间稳定；而B树查找路径不一，查询时间不同。
4. 一般来说B+树比B树矮胖，IO次数更少。

# 堆
堆总是满足下列性质：
1. 堆中某个结点的值总是不大于（或不小于）其父结点的值；
2. 堆总是一棵完全二叉树。

根结点最大的堆叫做 最大堆 或 大根堆，根结点最小的堆叫做 最小堆 或 小根堆。

完全二叉树的特点：
叶子结点只能出现在最下层和次下层，且最下层的叶子结点集中在树的左部。
需要注意的是，满二叉树肯定是完全二叉树，而完全二叉树不一定是满二叉树。
 ```

 完全二叉树举例
        a
       / \
      b   c
     / \
    d   e

 满二叉树举例
         a
       /  \
      b    c
     / \  / \
    d   e f  g

 最大堆举例
         50
        /  \
      45    40
     / \   / \
    20 25 35 30

 最小堆举例
         10
        /  \
      20    15
     /  \   / \
    25  50 30  40
 ```

# 红黑树
TODO

# 限流算法
**令牌桶**

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/cs-basic-knowledge/%E4%BB%A4%E7%89%8C%E6%A1%B6%E7%AE%97%E6%B3%95%E5%9B%BE.png)

令牌桶算法的原理是系统以恒定的速率产生令牌，然后把令牌放到令牌桶中，令牌桶有一个容量，当令牌桶满了的时候，再向其中放令牌，那么多余的令牌会被丢弃；当想要处理一个请求的时候，需要从令牌桶中取出一个令牌，如果此时令牌桶中没有令牌，那么则拒绝该请求。

**漏桶**

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/cs-basic-knowledge/%E6%BC%8F%E6%A1%B6%E7%AE%97%E6%B3%95%E5%9B%BE.png)

把请求比作是水，水来了都先放进桶里，并以限定的速度出水，当水来得过猛而出水不够快时就会导致水直接溢出，即拒绝服务。

**令牌桶和漏桶区别**：

漏桶算法能够强行限制数据的传输速率，而令牌桶算法能够在限制数据的平均传输速率的同时还允许某种程度的突发传输。

# 最短路径算法
// TODO 后续补充

**Dijkstra算法**
白话：从起源点开始找路径最短的下一个点，不构成环形。

https://www.jianshu.com/p/a8f9c6b69915

https://www.cnblogs.com/biyeymyhjob/archive/2012/07/31/2615833.html

**Floyd算法**

# 红黑树
红黑树是一种 自平衡 的二叉树，所谓的自平衡是指在插入和删除的过程中，红黑树会采取一定的策略对树的组织形式进行调整，以尽可能的减少树的高度，从而节省查找的时间。

**特点**
1. 时间复杂度低，O(lgN)
2. 自平衡

# 判断链表是否有环、两个链表是否相交 
第一部分——判断单个链表是否有环

使用两个指针，一个快指针，一个慢指针，快指针一次走两步，慢指针一次走一步；
若快指针最后变为空，则单链表为无环单链表，返回空指针；
若快慢指针在某一时刻指向同一节点，即二者完全一样，则为有环单链表，
此时让快指针重新指向链表头结点，然后快慢指针均一次走一步，
当快慢指针再次相同时，则此节点即为链表入环节点，将其返回。

第二部分——判断链表是否相交

情况一：两链表中一个为有环链表，一个为无环链表，则此时两链表一定不相交，返回空即可；

情况二：两个链表都是无环链表，此时有两种情况，1）两个无环链表相交；2）两个无环链表不相交；
首先遍历两链表，分别得到两个链表的长度，
取两个长度的差值，然后让长链表先遍历差值长度，此时长链表剩余部分与短链表长度相同，
然后两条链表同时遍历，直到遇到相同节点为止，若相同节点为空，则两链表不相交，返回空，
否则两链表相交，返回相交节点即可；

情况三：两个链表都是有环链表，此时有三种情况，1）两个有环链表不相交；2）两个有环链表相交，且交点在环外；3）两个有环链表相交，且交点在环内。
首先获得两个链表的入环节点，若入环节点相同，则可转变为情况二，此时将入环节点作为链表的终止节点即可；若两个入环节点不同，以一个入环节点开始遍历，若遍历一遍过程中都没有遇见另一个入环节点，则两链表不相交，返回空，若遇到另一入环节点，则说明两链表相交，此时返回任意一个入环节点即可。

# 一致性Hash算法

**解决的问题**

避免在节点扩容的情况下，使用hash函数计算桶位置会有变化。如redis集群有 4 台服务器，使用key % 4 = 2 定位 key 读写在 2 号服务器，当redis集群扩容到 5 台，这时候 key % 5 不等于 2。

**一致性Hash算法原理**

一致性Hash算法是对2^32取模，将整个哈希值空间组织成一个虚拟的圆环：

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/cs-basic-knowledge/%E5%93%88%E5%B8%8C%E7%8E%AF.jpg)

然后将服务器通过ip或主机名进行hash，如下图的Node A、Node B、Node C、Node D，接下来使用如下算法定位数据访问到相应服务器：`将数据key使用相同的函数Hash计算出哈希值，并确定此数据在环上的位置，从此位置沿环顺时针“行走”，第一台遇到的服务器就是其应该定位到的服务器`。数据对象进行hash，如下图的Object A、Object B、Object C、Object D，根据一致性Hash算法，数据A会被定为到Node A上，B被定为到Node B上，C被定为到Node C上，D被定为到Node D上。

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/cs-basic-knowledge/%E4%B8%80%E8%87%B4%E6%80%A7Hash%E7%AE%97%E6%B3%95%E5%9B%BE%E7%A4%BA.jpg)

**一致性Hash算法的容错性和可扩展性**

假设Node C不幸宕机，可以看到此时对象A、B、D不会受到影响，只有C对象被重定位到Node D。一般的，在一致性Hash算法中，如果一台服务器不可用，则受影响的数据仅仅是此服务器到其环空间中前一台服务器（即沿着逆时针方向行走遇到的第一台服务器）之间数据，其它不会受到影响。

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/cs-basic-knowledge/%E4%B8%80%E8%87%B4%E6%80%A7Hash%E7%AE%97%E6%B3%95%E5%AE%B9%E9%94%99%E6%80%A7%E5%9B%BE%E7%A4%BA.jpg)

如果在系统中增加一台服务器Node X，此时对象Object A、B、D不受影响，只有对象C需要重定位到新的Node X ！一般的，在一致性Hash算法中，如果增加一台服务器，则受影响的数据仅仅是新服务器到其环空间中前一台服务器（即沿着逆时针方向行走遇到的第一台服务器）之间数据，其它数据也不会受到影响。

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/cs-basic-knowledge/%E4%B8%80%E8%87%B4%E6%80%A7Hash%E7%AE%97%E6%B3%95%E5%8F%AF%E6%89%A9%E5%B1%95%E6%80%A7%E5%9B%BE%E7%A4%BA.jpg)

**哈希环数据倾斜问题**

致性Hash算法在服务节点太少时，容易因为节点分部不均匀而造成数据倾斜（被缓存的对象大部分集中缓存在某一台服务器上）问题

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/cs-basic-knowledge/%E5%93%88%E5%B8%8C%E7%8E%AF%E6%95%B0%E6%8D%AE%E5%80%BE%E6%96%9C%E9%97%AE%E9%A2%98%E5%9B%BE%E7%A4%BA.jpg)

为了解决这种数据倾斜问题，一致性Hash算法引入了虚拟节点机制，即`对每一个服务节点计算多个哈希，每个计算结果位置都放置一个此服务节点，称为虚拟节点`。具体做法可以在服务器IP或主机名的后面增加编号来实现。如可以为每台服务器计算三个虚拟节点，于是可以分别计算 “Node A#1”、“Node A#2”、“Node A#3”、“Node B#1”、“Node B#2”、“Node B#3”的哈希值，于是形成六个虚拟节点：

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/cs-basic-knowledge/%E4%B8%80%E8%87%B4%E6%80%A7Hash%E7%AE%97%E6%B3%95%E8%99%9A%E6%8B%9F%E8%8A%82%E7%82%B9%E5%9B%BE%E7%A4%BA.jpg)

# 布隆过滤器
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/cs-basic-knowledge/BloomFilter.jpg)

算法过程
1. 首先需要k个hash函数，每个函数可以把key散列成为1个整数
2. 初始化时，需要一个长度为n比特的数组，每个比特位初始化为0
3. 某个key加入集合时，用k个hash函数计算出k个散列值，并把数组中对应的比特位置为1
4. 判断某个key是否在集合时，用k个hash函数计算出k个散列值，并查询数组中对应的比特位，如果所有的比特位都是1，认为在集合中。


优点：不需要存储key，节省空间，能精准判断不存在
缺点：1. 无法精准判断存在 2. 无法删除

为什么用多个hash函数？
利用再哈希法解决哈希冲突。

底层实现
底层可以采用long[\]数组实现，如Java中BitSet基于long[\]数组，GoogleGuava中LockFreeBitArray基于AtomicLongArray对象，本质也是long[\]数组。

# 经典排序算法
https://github.com/lewiszlw/hello-world/tree/master/hello-world-algorithms-java/src/main/java/lewiszlw/algorithms/sort

### 冒泡排序

基本思想
两个数比较大小，较大的数下沉，较小的数冒起来。

过程
1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
2. 对第0个到第n-1个数据做同样的工作。这时，最大的数就“浮”到了数组最后的位置上。
3. 针对所有的元素重复以上的步骤，除了最后一个。
4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

源代码
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

### 选择排序

基本思想
不断遍历数组找到最小数字交换到数组前面

过程
1. 在长度为n的无序数组中，第一次遍历n-1个数，找到最小的数值与第一个元素交换；
2. 第二次遍历n-2个数，找到最小的数值与第二个元素交换；
3. ......
4. 第n-1次遍历，找到最小的数值与第n-1个元素交换，排序完成。

源代码
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


### 插入排序

基本思想
在要排序的一组数中，假定前n-1个数已经排好序，现在将第n个数插到前面的有序数列中，使得这n个数也是排好顺序的。如此反复循环，直到全部排好顺序。

过程
1. 从第一个元素开始，该元素可以认为已经被排序
2. 取出下一个元素，在已经排序的元素序列中从后向前扫描
3. 如果被扫描的元素（已排序）大于新元素，将该元素后移一位
4. 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置
5. 将新元素插入到该位置后
6. 重复步骤2~5


源代码
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


### 希尔排序
基本思想
希尔排序，也称递减增量排序算法，实质是分组插入排序。
在要排序的一组数中，根据某一增量分为若干子序列，并对子序列分别进行插入排序。然后逐渐将增量减小,并重复上述过程。直至增量为1,此时数据序列基本有序,最后进行插入排序。

过程
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/cs-basic-knowledge/SheelSort.png)

源代码
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


### 快速排序

基本思想
分治法思想

过程
1. 从数列中挑出一个元素作为基准数；
2. 分区过程，将比基准数大的放到右边，小于或等于它的数都放到左边；
3. 再对左右区间递归执行第二步，直至各区间只有一个数。

源代码
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


### 归并排序

基本思想
归并排序的思想就是先递归分解数组，再合并数组。
过程
1. 先考虑合并两个有序数组，基本思路是比较两个数组的最前面的数，谁小就先取谁，取了后相应的指针就往后移一位。然后再比较，直至一个数组为空，最后把另一个数组的剩余部分复制过来即可。
2. 再考虑递归分解，基本思路是将数组分解成left和right，如果这两个数组内部数据是有序的，那么就可以用上面合并数组的方法将这两个数组合并排序。如何让这两个数组内部是有序的？可以再二分，直至分解出的小组只含有一个元素时为止，此时认为该小组内部已有序。然后合并排序相邻二个小组即可。

源代码
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


### 堆排序

基本思想
堆排序是采用二叉堆的数据结构来实现的，虽然实质上还是一维数组。二叉堆是一个近似完全二叉树 。
二叉堆具有以下性质：
1. 父节点的键值总是大于或等于（小于或等于）任何一个子节点的键值。
2. 每个节点的左右子树都是一个二叉堆（都是最大堆或最小堆）。

将待排序序列构造成一个大顶堆，此时，整个序列的最大值就是堆顶的根节点。将其与末尾元素进行交换，此时末尾就为最大值。然后将剩余n-1个元素重新构造成一个堆，这样会得到n个元素的次小值。如此反复执行，便能得到一个有序序列了。

降序过程
1. 构造最小堆
2. 堆排序：由于堆是用数组模拟的。得到一个小根堆后，数组内部并不是有序的。因此需要将堆化数组有序化。

源代码
```java
    public void sort(int[] arr){

        // 建立最大堆
        for(int i = (arr.length - 1) / 2; i >= 0; i--){
            // 此时是完全无序堆，从最后一个非叶节点从右往左从下往上遍历，即从最底部最右部的第一个子树开始不停堆化，直至堆化整棵树
            heapify(arr, i, arr.length);
        }

        for(int heapLen = arr.length; heapLen > 1; heapLen--){
            // 堆顶（最大值）与末尾元素交换
            swap(arr, 0, heapLen - 1);
            // 除掉末尾元素，重新堆化，堆顶元素不断下沉到合适位置（此时的堆除了堆顶元素，其他元素未变化，即部分符合堆）
            heapify(arr, 0, heapLen - 1);
        }
    }

    // 堆化当前节点parent下属子树
    public void heapify(int[] arr, int parent, int len){
        int leftChild = 2 * parent + 1;	// 左子节点
        int rightChild = leftChild + 1;  // 右子节点
        int largest = parent;

        // 在父节点和左右子节点中最大的那个
        if (leftChild < len && arr[leftChild] > arr[largest]) {
            largest = leftChild;
        }
        if (rightChild < len && arr[rightChild] > arr[largest]) {
            largest = rightChild;
        }
        if (largest != parent) {
            // 原堆顶元素下移
            swap(arr, parent, largest);
            // 继续调整原堆顶元素下属子树
            heapify(arr, largest, len);
        }
    }

    private void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
```