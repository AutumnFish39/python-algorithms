## 单链表

### 什么是单链表？

通俗版：一行人站成一列，每个人记住左边的一个人是谁，于是自然而然地排成一列。

准确版：用任意存储单元存储元素，通过指针域串联。不支持随机存取，但插入删除只需修改指针。

### 特点：修改快，查询慢。

插入：在A,B结点当中插入E结点，将A的指向改成E，再让E指向B即可。

删除：在A,E,B结点当中删除E结点，将A的指向改为B即可

查找：从头开始沿着一张表顺着查（一个一个问）

### python实现

**链表结点结构**

```py
class LIstNode:
	def __init__(self, val=0, next=None)
		self.val = val
		self.next = next
```

**头插法建表**

特点，每次在链表头部插入，最终链表顺序和插入顺序相反

```py
def insert_head(self, val):
    """头插法"""
    new_node = ListNode(val)
    if not self.node:
        # 检查链表是否为空,头指针应默认为None
       	self.node =new_node
    else:
    	self.head = new_node
    self.length += 1
```

**尾插法**

```py
def insert_tail(self, val):
    """尾插法"""
    new_node = ListNode(val)
    if not self.head:
    	self.head = new_node
    else:
        # 遍历寻找尾结点:
        curr = self.head
        while curr.next:
            curr = curr.next
        curr.next = new_node
        
	self.length += 1
```

**删除操作**

```py
def delete(self, index):
    """删除指定位置结点"""
    if index < 0 or index >= self.length:
        raise IndexError
    
    if index == 0
        self.head = self.head.next
    else:
        prev = self.head
        for _ in range(index - 1):
            prev = prev.next
       	prev.next = prev.next.next
```

## 双链表

每个结点维护两个指针，前驱和后继，支持双向遍历。

**结点结构**

```py
class Node:
	def __init__(self, val=0, prev=None, next=None):
		self.val = val
		self.prev = prev
		self.next = next
```

**插入操作**

在A,B结点当中插入E结点：将A的后驱改为E，将E的前驱指向A，后驱指向B，将B的前驱改为E

```py
def insert(self, node, val):
	"""在指定结点之后插入新结点"""
    new_node = Node(val)
    # 由于A结点的后指针即将改变，所以先使用,否则会丢失A的后指针
    new_node.prev = node 
    new_node.next = node.next
    
    if node.next:
        node.next.prev = new_node
        
    # 最后改变A结点的后指针
    node.next = new_node
```

**删除结点**

在A,B,C三个结点当中删除B：将A的后指针指向C，将C的前指针指向A。

特殊情况，被删除的结点位于表头或表尾。

在表头不判断相当于重复引用，没删；在表尾不判断也同理，没删。

你不能让一个即将被删除的结点自己引用自己然后留在链表当中。

```py
def delete_node(self, node):
	"""删除结点"""
    if node.prev:
       node.prev.next = node.next
    else :
        self.head = node.next
    
    if node.next:
        node.next.prev = node.prev
```

### 循环链表

最后一个结点指向头结点的特殊链表（环）。

判空条件：

有头结点（不存放数据）的情况下，可以用`self.head.next == self.head`判断

或者`if not self.head`或`self.head is None`判断

**循环遍历**

```py
def circular(head):
    """遍历循环链表"""
    if not head:
        return
    
    curr = head.next
    while curr != head
    	print(curr.val)
        curr = curr.next
```

