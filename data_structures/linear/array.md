

关于数组，梦开始的地方。



## 1.ADT定义

数组：一段连续内存，支持按下标随机访问

核心操作：随机访问、遍历、插入、删除。

## 2.python定义

| ADT      | Python                |        说明        |
| -------- | --------------------- | :----------------: |
| 动态数组 | `list`                | 可变，随机访问O(1) |
| 定长数组 | `array`/`numpy.array` | 省内存，大批量计算 |

## 3.在`list`中的映射

### 省流版

| ADT 数组操作 | Python list 写法                      | 时间复杂度           | 说明 / 坑                           |
| :----------- | :------------------------------------ | :------------------- | :---------------------------------- |
| 创建空数组   | `[]` / `list()`                       | O(1)                 | 空列表                              |
| 创建定长初值 | `[0] * n`                             | O(n)                 | `[[]] * n` 会共享同一个内层列表     |
| 获取长度     | `len(lst)`                            | O(1)                 | 长度单独维护                        |
| 尾部追加     | `lst.append(x)`                       | 摊销 O(1)，最坏 O(n) | 扩容时会复制；返回 `None`           |
| 插入         | `lst.insert(i, x)`                    | O(n-i)               | 头部插入最坏 O(n)；返回 `None`      |
| 删除尾部     | `lst.pop()`                           | 摊销 O(1)            | 返回被删元素；空列表抛 `IndexError` |
| 删除任意位置 | `lst.pop(i)` / `del lst[i]`           | O(n-i)               | 后面元素要前移                      |
| 拼接         | `lst1 + lst2`                         | O(n+m)               | 创建新列表；不是逐元素加法          |
| 复制         | `lst.copy()` / `list(lst)` / `lst[:]` | O(n)                 | 浅拷贝                              |
| 清空         | `lst.clear()`                         | O(n)                 | 释放元素引用                        |

### 详细版

| ADT 数组操作   | Python list 写法                           | 时间复杂度           | 说明                                  |
| :------------- | :----------------------------------------- | :------------------- | :------------------------------------ |
| 创建空数组     | `[]` / `list()`                            | O(1)                 | 空列表                                |
| 创建定长初值   | `[0] * n`                                  | O(n)                 | `[[]] * n` 会共享同一个内层列表       |
| 获取长度       | `len(lst)`                                 | O(1)                 | 长度单独维护                          |
| 读 `get(i)`    | `lst[i]`                                   | O(1)                 | 支持负索引；越界抛 `IndexError`       |
| 写 `set(i, v)` | `lst[i] = v`                               | O(1)                 | 直接替换引用                          |
| 遍历           | `for x in lst`                             | O(n)                 | 遍历时修改列表容易出错                |
| 查找值         | `x in lst` / `lst.index(x)`                | O(n)                 | `index` 找不到抛 `ValueError`         |
| 尾部追加       | `lst.append(x)`                            | 摊销 O(1)，最坏 O(n) | 扩容时会复制；返回 `None`             |
| 插入           | `lst.insert(i, x)`                         | O(n-i)               | 头部插入最坏 O(n)；返回 `None`        |
| 删除尾部       | `lst.pop()`                                | 摊销 O(1)            | 返回被删元素；空列表抛 `IndexError`   |
| 删除任意位置   | `lst.pop(i)` / `del lst[i]`                | O(n-i)               | 后面元素要前移                        |
| 按值删除       | `lst.remove(x)`                            | O(n)                 | 只删第一个匹配；找不到抛 `ValueError` |
| 切片读         | `lst[a:b]`                                 | O(b-a)               | 返回浅拷贝，不是视图                  |
| 切片写         | `lst[a:b] = iterable`                      | O(n+k)               | 可改变长度；步长不为 1 时长度必须匹配 |
| 拼接           | `lst1 + lst2`                              | O(n+m)               | 创建新列表；不是逐元素加法            |
| 原地扩展       | `lst.extend(iterable)` / `lst += iterable` | O(k)                 | 原地修改；`+=` 影响别名               |
| 复制           | `lst.copy()` / `list(lst)` / `lst[:]`      | O(n)                 | 浅拷贝                                |
| 排序           | `lst.sort()`                               | O(n log n)           | 原地稳定排序；返回 `None`             |
| 排序新列表     | `sorted(lst)`                              | O(n log n)           | 原列表不变                            |
| 反转           | `lst.reverse()`                            | O(n)                 | 原地反转；返回 `None`                 |
| 反转迭代       | `reversed(lst)`                            | 迭代 O(n)            | 返回反向迭代器                        |
| 清空           | `lst.clear()`                              | O(n)                 | 释放元素引用                          |
| 计数           | `lst.count(x)`                             | O(n)                 | 线性统计                              |
| 最值 / 求和    | `min(lst)` / `max(lst)` / `sum(lst)`       | O(n)                 | 空列表 `min/max` 报错，`sum` 返回 0   |
| 哈希           | 不可哈希                                   | —                    | 不能做 `dict` 的 key 或 `set` 元素    |

## 4.常见坑点

1. `a=b`不是拷贝，而是别名。`a、b`指向的都是同一个列表。

   浅拷贝用`b = a[:]`、`b = a.copy()`、`b = list(a)`。

2. 浅拷贝只复制外层引用

   ```py
   a = [[1], [2]]
   b = a[:]
   b[0].append(9)   # a[0] 也会变成 [1, 9]
   ```

   需要完全独立用 `copy.deepcopy()`，深拷贝。

   ```py
   import copy
   
   a = [[1, 2], [3, 4]]
   b = copy.deepcopy(a)
   ```

3. `[[]] * n` 的经典坑

   ```py
   a = [[]] * 3
   a[0].append(1)   # *是引用，会导致三个元素都会改变
   
   # 正确写法
   a = [[] for _ in range(3)]
   ```

   原因，

4. 原地方法大多返回 `None`
   `append`、`extend`、`insert`、`remove`、`sort`、`reverse`、`clear` 都返回 `None`。

   如果写 `a = a.append(1)`会很悲壮。可以自己尝试一下，吃一堑长十智

5. `sort()` 和 `sorted()`的区别

   `sort()`会改变原列表，返回`none`，用法`a.sort()`。

   `sorted()`返回新列表，用法`b = sorted(a)`。

6. `append` 和 `extend` 不同
   `lst.append([1, 2])` 添加一个列表元素；
   `lst.extend([1, 2])` 添加两个元素。
   `extend('abc')` 会添加 `'a'`、`'b'`、`'c'`。

7. 遍历时修改列表

   ```py
   for x in lst: 
       lst.remove(x) # 可能会跳过元素，
   ```

   原因：
   
   `for`本质上是一个迭代器，迭代器内部维护一个索引，如`index`，每次`next()`，`index++`，当`index >= len(lst)`，抛出`StopIteration`，循环结束
   
   例子：
   
   ```py
   lst = [1, 2, 3, 4]
   
   for x in lst:
       print("处理", x)
       lst.remove(x)
       print("删除后", lst)
   ```
   
   执行过程
   
   | 步骤 | 迭代器index | 取出                    | 删除后列表 | 下一次index |
   | ---- | ----------- | ----------------------- | ---------- | ----------- |
   | 1    | 0           | `lst[0] = 1`            | `[2,3,4]`  | 1           |
   | 2    | 1           | `lst[1] = 3`            | `[2,4]`    | 2           |
   | 3    | 2           | `index=2 >= len(lst)=2` | `[2,4]`    | _           |
   
   输出：
   
   ```markdown
   处理 1
   删除后 [2, 3, 4]
   处理 3
   删除后 [2, 4]
   ```
   
   除此以外，遍历时修改列表，除了跳过，还可能导致重复处理、无限循环、`IndexError`等错误。
   
   **安全做法：**
   
   做法一：倒序删除
   
   ```py
   for i in range(len(lst) - 1, -1, -1):
       if 条件:
           del lst[i]
   ```
   
   从后往前删，删除后面的元素不会影响前面元素的下标。所以还没处理到的元素下标不会变。
   
   做法二：生成新列表
   
   ```py
   lst = [x for x in lst if not 条件]
   ```
   
   最普遍，推荐
   
   
