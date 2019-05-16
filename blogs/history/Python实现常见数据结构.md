---
title: Python实现常见数据结构
date: 2017-06-26 22:13:50
tags: [Python, 数据结构]
categories: Python
---
使用Python实现常见的数据结构，如栈、队列、链表、二叉树、图等。
<!--more-->
# 栈
```python
class Stack():
    def __init__(st, size):
        st.stack = []
        st.size = size
        st.top = 0
    
    def push(st, content):
        if st.isFull():
            print('Stack is full!')
        else:
            st.stack.append(content)
            st.top = st.top + 1

    def pop(st):
        if st.isEmpty():
            print('Stack is empty!')
        else:
            st.top = st.top - 1
            return st.stack[st.top]

    def isFull(st):
        if st.top == st.size:
            return True
        else:
            return False

    def isEmpty(st):
        if st.top == 0:
            return True
        else:
            return False
```
# 队列
```python
class Queue():
    def __init__(qu, size):
        qu.queue = []
        qu.size = size
        qu.head = 0
        qu.tail = 0
    
    def isEmpty(qu):
        if qu.head == qu.tail:
            return True
        else:
            return False

    def isFull(qu):
        if qu.tail - qu.head == qu.size:
            return True
        else:
            return False

    def enqueue(qu, content):
        if qu.isFull():
            print('Queue is full!')
        else:
            qu.queue.append(content)
            qu.tail = qu.tail + 1

    def dequeue(qu):
        if qu.isEmpty():
            print('Queue is empty!')
        else:
            qu.head = qu.head + 1
            return qu.queue[qu.head - 1]
```

# 链表
```python
class ListNode():
    def __init__(self, val = None, next = None):
        self.val = val
        self.next = next

class LinkedList():
    #列表转链表
    def listToLinkedList(self, list):
        head = ListNode(0)
        p = head
        for each in list:
            p.next = ListNode(each)
            p = p.next
        return head.next
    #打印链表
    def printLinkedList(self, head):
        p = head
        while p != None:
            print(p.val)
            p = p.next
```

# 二叉树
```python
class Tree():
    def __init__(self, leftnode = None, rightnode = None, data = None):
        self.leftnode = leftnode
        self.rightnode = rightnode
        self.data = data

class BTree():
    def __init__(self, base = None):
        self.base = base
    def isEmpty(self):
        if self.base == None:
            return True
        else:
            return False
    #前序遍历，NLR，根左右
    def preorder(self, node):
        if node == None:
            return
        print(node.data)
        self.preorder(node.leftnode)
        self.preorder(node.rightnode)
    #中序遍历，LNR，左根右
    def order(self, node):
        if node == None:
            return
        self.order(node.leftnode)
        print(node.data)
        self.order(node.rightnode)
    #后序遍历，LRN，左右根
    def postorder(self, node):
        if node == None:
            return
        self.postorder(node.leftnode)
        self.postorder(node.rightnode)
        print(node.data)
```

# 图
```python
'''
使用dict和list构造图
例如：
A -> B
A -> C
B -> C
B -> D
C -> D
D -> C
E -> F
F -> C
'''

graph = {'A': ['B', 'C'],
         'B': ['C', 'D'],
         'C': ['D'],
         'D': ['C'],
         'E': ['F'],
         'F': ['C']}

#寻找一条路径
def findPath(graph, start, end, path = []):
    path = path + [start]
    if start == end:
        return path
    if start not in graph:
        return None
    for node in graph[start]:
        newPath = findPath(graph, node, end, path)
        if newPath:
            return newPath
    return path



#寻找所有路径
def findAllPaths(graph, start, end, path = []):
    path = path + [start]
    if start == end:
        return [path]
    if start not in graph:
        return []
    paths = []
    for node in graph[start]:
        if node not in path:
            newPaths = findAllPaths(graph, node, end, path)
            for each in newPaths:
                paths.append(each)
    return paths



#寻找最短路径
def findShortestPath(graph, start, end, path = []):
    path = path + [start]
    if start == end:
        return path
    if start not in graph:
        return None
    shortestPath = None
    for node in graph[start]:
        if node not in path:
            newPath = findShortestPath(graph, node, end, path)
            if newPath:
                if not shortestPath or len(newPath) < len(shortestPath):
                    shortestPath = newPath
    return shortestPath
```
