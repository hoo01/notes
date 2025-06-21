# 一. 初识算法

## 1.3 二分查找 [^3]

二分查找算法也称折半查找，是一种非常高效的工作于有序数组的查找算法。后续的课程中还会学习更多的查找算法，但在此之前，不妨用它作为入门。

### 1) 基础版

需求：在**有序**数组 $A$ 内，查找值 $target$

* 如果找到返回索引
* 如果找不到返回 $-1$

算法描述

|      |                                                              |
| ---- | ------------------------------------------------------------ |
| 前提 | 给定一个内含 $n$ 个元素的有序数组 $A$，满足 $A_{0}\leq A_{1}\leq A_{2}\leq \cdots \leq A_{n-1}$，一个待查值 $target$ |
| 1    | 设置 $i=0$，$j=n-1$                                          |
| 2    | 如果 $i \gt j$，结束查找，没找到                             |
| 3    | 设置 $m = floor(\frac {i+j}{2})$ ，$m$ 为中间索引，$floor$ 是向下取整（$\leq \frac {i+j}{2}$ 的最小整数） |
| 4    | 如果 $target < A_{m}$ 设置 $j = m - 1$，跳到第2步            |
| 5    | 如果 $A_{m} < target$ 设置 $i = m + 1$，跳到第2步            |
| 6    | 如果 $A_{m} = target$，结束查找，找到了                      |

> ***P.S.***
>
> * 对于一个算法来讲，都有较为严谨的描述，上面是一个例子
> * 后续讲解时，以简明直白为目标，不会总以上面的方式来描述算法



java 实现


```java
public static int binarySearch(int[] a, int target) {
    int i = 0, j = a.length - 1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {			// 在左边
            j = m - 1;
        } else if (a[m] < target) {		// 在右边
            i = m + 1;
        } else {
            return m;
        }
    }
    return -1;
}
```

* $i,j$ 对应着搜索区间 $[0,a.length-1]$（注意是闭合的区间），$i<=j$ 意味着搜索区间内还有未比较的元素，$i,j$ 指向的元素也可能是比较的目标
  * 思考：如果不加 $i==j$ 行不行？
  * 回答：不行，因为这意味着 $i,j$ 指向的元素会漏过比较
* $m$ 对应着中间位置，中间位置左边和右边的元素可能不相等（差一个），不会影响结果
* 如果某次未找到，那么缩小后的区间内不包含 $m$

### 2) 改变版

另一种写法

```java
public static int binarySearch(int[] a, int target) {
    int i = 0, j = a.length;
    while (i < j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {			// 在左边
            j = m;
        } else if (a[m] < target) {		// 在右边
            i = m + 1;
        } else {
            return m;
        }
    }
    return -1;
}
```

* $i,j$ 对应着搜索区间 $[0,a.length)$（注意是左闭右开的区间），$i<j$ 意味着搜索区间内还有未比较的元素，$j$ 指向的**一定不是**查找目标
  * 思考：为啥这次不加 $i==j$ 的条件了？
  * 回答：这回 $j$ 指向的不是查找目标，如果还加 $i==j$ 条件，就意味着 $j$ 指向的还会再次比较，找不到时，会死循环
* 如果某次要缩小右边界，那么 $j=m$，因为此时的 $m$ 已经**不是**查找目标了



## 1.4 时间复杂度

**时间复杂度**

下面的查找算法也能得出与之前二分查找一样的结果，那你能说出它差在哪里吗？

```java
public static int search(int[] a, int k) {
    for (
        int i = 0;
        i < a.length;
        i++
    ) {
        if (a[i] == k) {
            return i;
        }
    }
    return -1;
}
```

考虑最坏情况下（没找到）例如 `[1,2,3,4]` 查找 5

* `int i = 0` 只执行一次
* `i < a.length` 受数组元素个数 $n$ 的影响，比较 $n+1$ 次
* `i++` 受数组元素个数 $n$ 的影响，自增 $n$ 次
* `a[i] == k` 受元素个数 $n$ 的影响，比较 $n$ 次
* `return -1`，执行一次

粗略认为每行代码执行时间是 $t$，假设 $n=4$ 那么

* 总执行时间是 $(1+4+1+4+4+1)*t = 15t$
* 可以推导出更一般地公式为，$T = (3*n+3)t$ 

如果套用二分查找算法，还是 `[1,2,3,4]` 查找 5

```java
public static int binarySearch(int[] a, int target) {
    int i = 0, j = a.length - 1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {			// 在左边
            j = m - 1;
        } else if (a[m] < target) {		// 在右边
            i = m + 1;
        } else {
            return m;
        }
    }
    return - 1;
}
```

* `int i = 0, j = a.length - 1` 各执行 1 次
* `i <= j` 比较 $floor(\log_{2}(n)+1)$ 再加 1 次, 比较不成立退出循环（总结规律得出）
* `(i + j) >>> 1` 计算 $floor(\log_{2}(n)+1)$ 次
* 接下来 `if() else if() else` 会执行 $3* floor(\log_{2}(n)+1)$ 次，分别为
* （以下都是考虑最坏的情况）
  * if 比较
  * else if 比较
  * else if 比较成立后的赋值语句

* `return -1`，执行一次

结果：

* 总执行时间为 $(2 + (1+3) + 3 + 3 * 3 +1)*t = 19t$
* 更一般地公式为 $(4 + 5 * floor(\log_{2}(n)+1))*t$

> **注意：**
>
> 左侧未找到和右侧未找到结果不一样，这里不做分析



两个算法比较，可以看到 $n$ 在较小的时候，二者花费的次数差不多

<img src=".\imgs\image-20221108095747933.png" alt="image-20221108095747933" style="zoom:50%;" />

但随着 $n$ 越来越大，比如说 $n=1000$ 时，用二分查找算法（红色）也就是 $54t$，而蓝色算法则需要 $3003t$

<img src=".\imgs\image-20221108100014451.png" alt="image-20221108100014451" style="zoom:50%;" />



> 画图采用的是 [Desmos | 图形计算器](https://www.desmos.com/calculator?lang=zh-CN)



计算机科学中，**时间复杂度**是用来衡量：一个算法的执行，随数据规模增大，而增长的时间成本

* 不依赖于环境因素

如何表示时间复杂度呢？

* 假设算法要处理的**数据规模是 $n$，代码总的执行行数用函数 $f(n)$** 来表示，例如：
  * 线性查找算法的函数 $f(n) = 3*n + 3$
  * 二分查找算法的函数 $f(n) = (floor(log_2(n)) + 1) * 5 + 4$

* 为了对 $f(n)$ 进行化简，应当抓住主要矛盾，找到一个变化趋势与之相近的表示法

### 1.4.1**大 $O$ 表示法[^4]**

![微信截图_20250301152801.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250301152801.png)
其中
* $c, c_1, c_2$ 都为一个常数
* $f(n)$ 是实际执行代码行数与 n 的函数(y轴越高执行越多，算法效率越低)
* $g(n)$ 是经过化简，变化趋势与 $f(n)$ 一致的 n 的函数



####  **渐进上界**

渐进上界（asymptotic upper bound）：从某个常数 $n_0$开始，$c*g(n)$ 总是位于 $f(n)$ 上方，那么记作 $O(g(n))$

*代表算法执行的==最差情况==

例1
* $f(n) = 3*n+3$ 
* $g(n) = n$
* 取 $c=4$，在$n_0=3$ 之后，$g(n)$ 可以作为 $f(n)$ 的渐进上界，因此表示法写作 $O(n)$

例2
* $f(n) = 5*floor(log_2(n)) + 9$
* $g(n) = log_2(n)$
* $O(log_2(n))$

已知 $f(n)$ 来说，求 $g(n)$

* 表达式中相乘的常量，可以省略，如
  * $f(n) = 100*n^2$ 中的 $100$
* 多项式中数量规模更小（低次项）的表达式，如
  * $f(n)=n^2+n$ 中的 $n$
  * $f(n) = n^3 + n^2$ 中的 $n^2$
* 不同底数的对数，渐进上界可以用一个对数函数 $\log n$ 表示
  * 例如：$log_2(n)$ 可以替换为 $log_{10}(n)$，因为 $log_2(n) = \frac{log_{10}(n)}{log_{10}(2)}$，相乘的常量 $\frac{1}{log_{10}(2)}$ 可以省略
* 类似的，对数的常数次幂可省略
  * 如：$log(n^c) = c * log(n)$ 
#### **常见大 $O$ 表示法**
![微信截图_20250301155333.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250301155333.png)

 按时间复杂度从低到高

* 黑色横线 $O(1)$，常量时间，意味着算法时间并不随数据规模而变化
* 绿色 $O(log(n))$，对数时间
* 蓝色 $O(n)$，线性时间，算法时间与数据规模成正比
* 橙色 $O(n*log(n))$，拟线性时间
* 红色 $O(n^2)$ 平方时间
* 黑色朝上 $O(2^n)$ 指数时间
* 没画出来的 $O(n!)$



#### **渐进下界**

渐进下界（asymptotic lower bound）：从某个常数 $n_0$开始，$c*g(n)$ 总是位于 $f(n)$ 下方，那么记作 $\Omega(g(n))$

*表示算法执行的最佳情况


#### **渐进紧界**

渐进紧界（asymptotic tight bounds）：从某个常数 $n_0$开始，$f(n)$ 总是在 $c_1*g(n)$ 和 $c_2*g(n)$ 之间，那么记作 $\Theta(g(n))$
*既能代表最佳情况，也能代表最差

#### **空间复杂度**

与时间复杂度类似，一般也使用大 $O$ 表示法来衡量：一个算法执行随数据规模增大，而增长的**额外**空间成本

```java
public static int binarySearchBasic(int[] a, int target) {
    int i = 0, j = a.length - 1;    // 设置指针和初值
    while (i <= j) {                // i~j 范围内有东西
        int m = (i + j) >>> 1;
        if(target < a[m]) {         // 目标在左边
            j = m - 1;
        } else if (a[m] < target) { // 目标在右边
            i = m + 1;
        } else {                    // 找到了
            return m;
        }
    }
    return -1;
}
```



**二分查找性能**

下面分析二分查找算法的性能

时间复杂度

* 最坏情况：$O(\log n)$
* 最好情况：如果待查找元素恰好在数组中央，只需要循环一次 $O(1)$ //常数都用1表示
* **在这里，并不能用渐进紧届替换渐进上界，因为在渐进紧届，最好情况和最坏情况必须有相同函数g(n)

空间复杂度

* 需要常数个指针 $i,j,m$，因此额外占用的空间是 $O(1)$ 

## 1.5 回到初版的二分查找

```python
public class BinarySearch {  
    public static int binarySearchBasic(int[] a, int target) {  
        int i = 0, j = a.length - 1;  
        //假设WHILE循环执行了L次，如果target在最左边，那么if成立，只需执行L次
        //如果元素在最右边，那么需要执行if 和 else if两项，2L次  
        while (i <= j) {  
            int m = (i + j) / 2; 
            if (a[m] < target) {  
                i = m + 1;  
            }  
            else if (target < a[m]) {  
                j = m - 1;  
            } else {  
                return m;  
            }  
        }  
        return -1;  
    }
```

### 1) 平衡版

```java
public static int binarySearchBalanced(int[] a, int target) {  
    int i = 0, j = a.length ; // i 可能指向目标，j在边界外  
    while (1 < j - i) {  
        int m = (i + j) >>> 1;  
        if (target < a[m]) {  
            j = m ;  
        } else {  
            i = m ;  
        }  
    }  
    if (a[i] == target) {  
        return i;  
    } else {  
        return -1;  
    }  
}
```
![微信截图_20250301165828.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250301165828.png)

思考：

1. 左闭右开的区间，$i$ 指向的可能是目标，而 $j$ 指向的不是目标
2. 不奢望循环内通过 $m$ 找出目标, 缩小区间直至剩 1 个, 剩下的这个可能就是要找的（通过 $i$）
   * $j - i > 1$ 的含义是，在范围内待比较的元素个数 > 1
3. 改变 $i$ 边界时，它指向的可能是目标，因此不能 $m+1$
4. 循环内的平均比较次数减少了
5. 时间复杂度 $\Theta(log(n))$


### 2) Java 版

```java
private static int binarySearch0(long[] a, int fromIndex, int toIndex,
                                     long key) {
    int low = fromIndex;
    int high = toIndex - 1;

    while (low <= high) {//有=，因为low和high都在边界内
        int mid = (low + high) >>> 1;
        long midVal = a[mid];

        if (midVal < key)
            low = mid + 1;
        else if (midVal > key)
            high = mid - 1;
        else
            return mid; // key found
    }
    return -(low + 1);  // key not found.
}
```

* 例如 $[1,3,5,6]$ 要插入 $2$ 那么就是找到一个位置，这个位置左侧元素都比它小
  * 等循环结束，若没找到，low 左侧元素肯定都比 target 小，因此 low 即插入点
* 插入点取负是为了与找到情况区分
* -1 是为了把索引 0 位置的插入点与找到的情况进行区分
  *java 中 0 == -0, 比如以下图中，想插入5，如果直接返回-low则是 -0 ，但是实际数组中9才是索引0的数
![微信截图_20250301211255.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250301211255.png)

#### 如何利用Arrays.binarySearch插入数？
![微信截图_20250301214822.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250301214822.png)

```java
import java.util.Arrays;  
public class TestBinarySearch {  
  
    public static void main(String[] args) {  
        test8();  // ✅ 直接调用静态方法  
    }  
    public static void test8() {  
        int[] a = {2,5,8};  
        int target = 4;  
        
        int i = Arrays.binarySearch(a,target); //java自带的二分查找, i = -插入点 - 1       
        System.out.println(i);  
  
        if (i < 0) { // !如果i是负数，说明target不在原始数组a中  
            int insertIndex = Math.abs(i + 1); //i = -插入点 - 1  因此有 插入点 = abs(i+1)            
            int[] b = new int[a.length + 1]; //[0,0,0,0]  
            System.arraycopy(a,0,b,0,insertIndex);  
            b[insertIndex] = target;  
            System.arraycopy(a, insertIndex, b, insertIndex + 1, a.length - insertIndex);  
            System.out.println(Arrays.toString(b));  
        }  
    }  
}
```


### 3) Leftmost 与 Rightmost
#### Leftmost1

有时我们希望返回的是最左侧的重复元素，如果用 Basic 二分查找

* 对于数组 $[1, 2, 3, 4, 4, 5, 6, 7]$，查找元素4，结果是索引3

* 对于数组 $[1, 2, 4, 4, 4, 5, 6, 7]$，查找元素4，结果也是索引3，并不是最左侧的元素

```java
public static int binarySearchLeftmost1(int[] a, int target) {
    int i = 0, j = a.length - 1;
    int candidate = -1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m - 1;
        } else if (a[m] < target) {
            i = m + 1;
        } else {
            candidate = m; // 记录候选位置
            j = m - 1;     // 继续向左
        }
    }
    return candidate;
}
```

#### Rightmost1
如果希望返回的是最右侧元素

```java
public static int binarySearchRightmost1(int[] a, int target) {
    int i = 0, j = a.length - 1;
    int candidate = -1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m - 1;
        } else if (a[m] < target) {
            i = m + 1;
        } else {
            candidate = m; // 记录候选位置
            i = m + 1;	   // 继续向右
        }
    }
    return candidate;
}
```

**应用**
对于 Leftmost 与 Rightmost，可以返回一个比 -1 更有用的值
#### Leftmost2

```java
/**  
 * * @param a  
 * @param target  
 * @return 返回 >= target的最靠左索引位置  
 */
public static int binarySearchLeftmost(int[] a, int target) {
    int i = 0, j = a.length - 1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target <= a[m]) {
            j = m - 1;
        } else {
            i = m + 1;
        }
    }
    return i; 
}
```

* leftmost 返回值的另一层含义：$\lt target$ 的元素个数
* 小于等于中间值，都要向左找
#### Rightmost2

```java
/**  
 * * @param a  
 * @param target  
 * @return <= target的最靠右索引位置  
 */
public static int binarySearchRightmost(int[] a, int target) {
    int i = 0, j = a.length - 1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m - 1;
        } else {
            i = m + 1;
        }
    }
    return i - 1;//这里写j也可以
}
```

- 大于等于中间值，都要向右找
- 目标：找到最后一个 <= target 的位置。
- i 的更新逻辑：只有当 `target >= a[m]` 时，`i` 才会被推到 `m+1`。
- 这表示 `i` 始终在 **向右推进**，试图找到第一个 `> target` 的位置。
-  **循环终止时**，`i` 和 `j` 的关系是 `i = j + 1`（如示例中的 `i=3`，`j=2`）。
- **`i` 的最终位置**：在最后一次更新中，`i` 被推到了 `m+1`


- **`j` 的更新逻辑**：当 `target < a[m]` 时，`j` 回退到 `m-1`。
- 这表示 `j` 在 **向左收缩**，确保它始终指向 `<= target` 的最后一个位置。

#### =决定了代码的方向性
几个名词

![image-20221125174155058](./imgs/image-20221125174155058.png)

**范围查询**：

* 查询 $x \lt 4$，$0 .. leftmost(4) - 1$
* 查询 $x \leq 4$，$0 .. rightmost(4)$
* 查询 $4 \lt x$，$rightmost(4) + 1 .. \infty $
* 查询 $4 \leq x$， $leftmost(4) .. \infty$
* 查询 $4 \leq x \leq 7$，$leftmost(4) .. rightmost(7)$
* 查询 $4 \lt x \lt 7$，$rightmost(4)+1 .. leftmost(7)-1$

**求排名**：$leftmost(target) + 1$

* $target$ 可以不存在，如：$leftmost(5)+1 = 6$
* $target$ 也可以存在，如：$leftmost(4)+1 = 3$

**求前任（predecessor）**：$leftmost(target) - 1$

* $leftmost(3) - 1 = 1$，前任 $a_1 = 2$
* $leftmost(4) - 1 = 1$，前任 $a_1 = 2$

**求后任（successor）**：$rightmost(target)+1$

* $rightmost(5) + 1 = 5$，后任 $a_5 = 7$
* $rightmost(4) + 1 = 5$，后任 $a_5 = 7$

**求最近邻居**：

* 前任和后任距离更近者



## 习题

### 1) 时间复杂度估算

用函数 $f(n)$ 表示算法效率与数据规模的关系，假设每次解决问题需要 1 微秒（$10^{-6}$ 秒），进行估算：

1. 如果 $f(n) = n^2$ 那么 1 秒能解决多少次问题？1 天呢？
2. 如果 $f(n) = log_2(n)$  那么 1 秒能解决多少次问题？1 天呢？
3. 如果 $f(n) = n!$ 那么 1 秒能解决多少次问题？1 天呢？



**参考解答**

1. 1秒 $\sqrt{10^6} = 1000$ 次，1 天 $\sqrt{10^6 * 3600 * 24} \approx 293938$ 次
2. 1秒 $2^{1,000,000} $ 次，一天 $2^{86,400,000,000}$
3. 推算如下
   * $10! = 3,628,800$ 1秒能解决 $1,000,000$ 次，因此次数为 9 次
   * $14!=87,178,291,200$，一天能解决 $86,400,000,000$ 次，因此次数为 13 次



### 2) 耗时估算

一台机器对200个单词进行排序花了200秒(使用冒泡排序)，那么花费800秒，大概可以对多少个单词进行排序

a. 400

b. 600

c. 800

d. 1600

**答案**

* a

**解释**

* 冒泡排序时间复杂度是 $O(N^2)$
* 时间增长 4 倍，而因此能处理的数据量是原来的 $\sqrt{4} = 2$ 倍



### 3) E01. 二分查找-Leetcode 704

**要点**：减而治之，可以用递归或非递归实现

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1

例如

```java
输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
    
输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1    
```

**参考答案**：略，可以用讲过的任意一种二分求解



### 4) E02. 搜索插入位置-Leetcode 35

**要点**：理解谁代表插入位置

给定一个排序数组和一个目标值

* 在数组中找到目标值，并返回其索引
* 如果目标值不存在于数组中，返回它将会被按顺序插入的位置

例如

```
输入: nums = [1,3,5,6], target = 5
输出: 2

输入: nums = [1,3,5,6], target = 2
输出: 1

输入: nums = [1,3,5,6], target = 7
输出: 4
```



#### 二分查找基础版
代码改写，基础版中，找到返回 m，没找到 i 代表插入点，因此有

```java
public int searchInsert(int[] a, int target) {
    int i = 0, j = a.length - 1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m - 1;
        } else if (a[m] < target) {
            i = m + 1;
        } else {
            return m;
        }
    }
    return i; // 二分查找最终会停在 `i > j`
}
```



#### 二分查找平衡版

* 如果 target == a[i] 返回 i 表示找到
* 如果 target < a[i]，例如 target = 2，a[i] = 3，这时就应该在 i 位置插入 2
* 如果 a[i] < target，例如 a[i] = 3，target = 4，这时就应该在 i+1 位置插入 4

```java
public static int searchInsert(int[] a, int target) {
    int i = 0, j = a.length;
    while (1 < j - i) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m; //
        } else {
            i = m;
        }
    }
    return (target <= a[i]) ? i : i + 1;
}
```



#### 用 leftmost 版本解
返回值即为插入位置（并能处理元素重复的情况）

```java
public int searchInsert(int[] a, int target) {
    int i = 0, j = a.length - 1;
    while(i <= j) {
        int m = (i + j) >>> 1;
        if(target <= a[m]) {
            j = m - 1;
        } else {
            i = m + 1;
        } 
    }
    return i;
}
```

#### 为什么不能用rightmost

| 方法                             | `target` 存在       | `target` 不存在              | 为什么？                                                                    |
| :----------------------------- | :---------------- | :------------------------ | :---------------------------------------------------------------------- |
| `leftmost` (`return i`) ✅      | 找到 `target` 的最左索引 | 找到 `target` **应该插入的位置** ✅ | `i` 终止时指向 **第一个 `>= target` 的位置**，**直接插入正确！**                           |
| `rightmost` (`return i - 1`) ✅ | 找到 `target` 的最右索引 | 可能返回比 `target` 小的索引 ❌     | `i - 1` 终止时指向 **最后一个 `<= target` 的位置**，如果 `target` 不存在，它就“顶替”前一个值，导致错误！ |


### 5) E03. 搜索开始结束位置-Leetcode 34

给你一个按照非递减顺序排列的整数数组 nums，和一个目标值 target。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

你必须设计并实现时间复杂度为 O(log n) 的算法解决此问题



例如

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]

输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]

输入：nums = [], target = 0
输出：[-1,-1]
```



**参考答案**

```java
public static int left(int[] a, int target) {
    int i = 0, j = a.length - 1;
    int candidate = -1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m - 1;
        } else if (a[m] < target) {
            i = m + 1;
        } else {
            candidate = m;
            j = m - 1;
        }
    }
    return candidate;
}

public static int right(int[] a, int target) {
    int i = 0, j = a.length - 1;
    int candidate = -1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m - 1;
        } else if (a[m] < target) {
            i = m + 1;
        } else {
            candidate = m;
            i = m + 1;
        }
    }
    return candidate;
}

public static int[] searchRange(int[] nums, int target) {
    int x = left(nums, target);
    if(x == -1) {
        return new int[] {-1, -1};
    } else {
        return new int[] {x, right(nums, target)};
    }
}
```



# 二. 数据结构

## 2.1 数组

### 1) 概述

**定义**

在计算机科学中，数组是由一组元素（值或变量）组成的数据结构，每个元素有至少一个索引或键来标识

>In computer science, an **array** is a data structure consisting of a collection of *elements* (values or variables), each identified by at least one *array index* or *key*



因为数组内的元素是**连续存储**的，所以数组中元素的地址，可以通过其索引计算出来，例如：

```java
int[] array = {1,2,3,4,5}
```



知道了数组的**数据**起始地址 $BaseAddress$，就可以由公式 $BaseAddress + i * size$ 计算出索引 $i$ 元素的地址

* $i$ 即索引，在 Java、C 等语言都是从 0 开始
* $size$ 是每个元素占用字节，例如 $int$ 占 $4$，$double$ 占 $8$



**小测试**

```java
byte[] array = {1,2,3,4,5}
```

已知 array 的**数据**的起始地址是 0x7138f94c8，那么元素 3 的地址是什么？

> 答：0x7138f94c8 + 2 * 1 = 0x7138f94ca，byte数组每个数字只占一个字节



**空间占用**

Java 中数组结构为

* 8 字节 markword
* 4 字节 class 指针（压缩 class 指针的情况）
* 4 字节 数组大小（决定了数组最大容量是 $2^{32}$）
* 数组元素 + 对齐字节（java 中所有对象大小都是 8 字节的整数倍[^12]，不足的要用对齐字节补足）

例如

```java
int[] array = {1, 2, 3, 4, 5};
```

的大小为 40 个字节，组成如下

```
8 + 4 + 4 + 5*4 + 4(alignment)
```



**随机访问性能**

即根据索引查找元素，时间复杂度是 $O(1)$



### 2) 动态数组

**java 版本**

```java
public class DynamicArray implements Iterable<Integer> {
    private int size = 0; // 逻辑大小
    private int capacity = 8; // 容量
    private int[] array = {};


    /**
     * 向最后位置 [size] 添加元素
     *
     * @param element 待添加元素
     */
    public void addLast(int element) {
        add(size, element);
    }

    /**
     * 向 [0 .. size] 位置添加元素
     *
     * @param index   索引位置
     * @param element 待添加元素
     */
    public void add(int index, int element) {
        checkAndGrow();

        // 添加逻辑
        if (index >= 0 && index < size) {
            // 向后挪动, 空出待插入位置
            System.arraycopy(array, index,
                    array, index + 1, size - index);
        }
        array[index] = element;
        size++;
    }

    private void checkAndGrow() {
        // 容量检查
        if (size == 0) {
            array = new int[capacity];
        } else if (size == capacity) {
            // 进行扩容, 1.5 1.618 2
            capacity += capacity >> 1;
            int[] newArray = new int[capacity];
            System.arraycopy(array, 0,
                    newArray, 0, size);
            array = newArray;
        }
    }

    /**
     * 从 [0 .. size) 范围删除元素
     *
     * @param index 索引位置
     * @return 被删除元素
     */
    public int remove(int index) { // [0..size)
        int removed = array[index];
        if (index < size - 1) {
            // 向前挪动
            System.arraycopy(array, index + 1,
                    array, index, size - index - 1);
        }
        size--;
        return removed;
    }


    /**
     * 查询元素
     *
     * @param index 索引位置, 在 [0..size) 区间内
     * @return 该索引位置的元素
     */
    public int get(int index) {
        return array[index];
    }

    /**
     * 遍历方法1
     *
     * @param consumer 遍历要执行的操作, 入参: 每个元素
     */
    public void foreach(Consumer<Integer> consumer) {
        for (int i = 0; i < size; i++) {
            // 提供 array[i]
            // 返回 void
            consumer.accept(array[i]);
        }
    }

    /**
     * 遍历方法2 - 迭代器遍历
     */
    @Override
    public Iterator<Integer> iterator() {
        return new Iterator<Integer>() {
            int i = 0;

            @Override
            public boolean hasNext() { // 有没有下一个元素
                return i < size;
            }

            @Override
            public Integer next() { // 返回当前元素,并移动到下一个元素
                return array[i++];
            }
        };
    }

    /**
     * 遍历方法3 - stream 遍历
     *
     * @return stream 流
     */
    public IntStream stream() {
        return IntStream.of(Arrays.copyOfRange(array, 0, size));
    }
}
```

* 这些方法实现，都简化了 index 的有效性判断，假设输入的 index 都是合法的



**插入或删除性能**

头部位置，时间复杂度是 $O(n)$

中间位置，时间复杂度是 $O(n)$

尾部位置，时间复杂度是 $O(1)$（均摊来说）

插入或删除时的“
移动”含义
比如你要在下标 `1` 插入一个新值 `15`，你希望得到：
`[10, 15, 20, 30, 40, 50]`
你得先把 **从下标 1 开始的所有元素**，往后搬一格：
```java
array[5] = array[4];
array[4] = array[3];
array[3] = array[2];
array[2] = array[1];
```
**搬了 5 - 1 = 4 次操作**

这些操作每一步都花费 $O(1)$，但你需要做 $n$ 次（最坏情况是整个数组搬动），所以总的时间是 **$O(n)$**。
> **数组必须保证元素在内存中是连续的，插入/删除会打破这种连续性，因此需要大量复制。

>而每次复制操作是 $O(1)$，复制了 $k$ 次，就是 $O(k)$。最坏情况下 $k = n$，所以是 $O(n)$。


### 3) 二维数组

```java
int[][] array = {
    {11, 12, 13, 14, 15},
    {21, 22, 23, 24, 25},
    {31, 32, 33, 34, 35},
};
```

内存图如下

![微信截图_20250502202257.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250502202257.png)
cp classpointer
* 二维数组占 32 个字节，其中 array[0]，array[1]，array[2] 三个元素分别保存了指向三个一维数组的引用

* 三个一维数组各占 40 个字节

* 它们在内层布局上是**连续**的

更一般的，对一个二维数组 $Array[m][n]$

* $m$ 是外层数组的长度，可以看作 row 行
* $n$ 是内层数组的长度，可以看作 column 列
* 当访问 $Array[i][j]$，$0\leq i \lt m, 0\leq j \lt n$时，就相当于
  * 先找到第 $i$ 个内层数组（行）
  * 再找到此内层数组中第 $j$ 个元素（列）



**小测试**

Java 环境下（不考虑类指针和引用压缩，此为默认情况），有下面的二维数组

```java
byte[][] array = {
    {11, 12, 13, 14, 15},
    {21, 22, 23, 24, 25},
    {31, 32, 33, 34, 35},
};
```

已知 array **对象**起始地址是 0x1000，那么 23 这个元素的地址是什么？

> 答：
>
> * 起始地址 0x1000
> * 外层数组大小：16字节对象头 + 3元素 * 每个引用4字节 + 4 对齐字节 = 32 = 0x20
> * 第一个内层数组大小：16字节对象头 + 5元素 * 每个byte1字节 + 3 对齐字节 = 24 = 0x18
> * 第二个内层数组，16字节对象头 = 0x10，待查找元素索引为 2
> * 最后结果 = 0x1000 + 0x20 + 0x18 + 0x10 + 2*1 = 0x104a



### 4) 局部性原理

这里只讨论空间局部性

* cpu 读取内存（速度慢）数据后，会将其放入高速缓存（速度快）当中，如果后来的计算再用到此数据，在缓存中能读到的话，就不必读内存了
* 缓存的最小存储单位是缓存行（cache line），一般是 64 bytes，一次读的数据少了不划算啊，因此最少读 64 bytes 填满一个缓存行，因此读入某个数据时也会读取其**临近的数据**，这就是所谓**空间局部性**



**对效率的影响**

比较下面 ij 和 ji 两个方法的执行效率

```java
int rows = 1000000;
int columns = 14;
int[][] a = new int[rows][columns];

StopWatch sw = new StopWatch();
sw.start("ij");
ij(a, rows, columns);
sw.stop();
sw.start("ji");
ji(a, rows, columns);
sw.stop();
System.out.println(sw.prettyPrint());
```

ij 方法

```java
public static void ij(int[][] a, int rows, int columns) {
    long sum = 0L;
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < columns; j++) {
            sum += a[i][j];
        }
    }
    System.out.println(sum);
}
```

ji 方法

```java
public static void ji(int[][] a, int rows, int columns) {
    long sum = 0L;
    for (int j = 0; j < columns; j++) {
        for (int i = 0; i < rows; i++) {
            sum += a[i][j];
        }
    }
    System.out.println(sum);
}
```

执行结果

```
0
0
StopWatch '': running time = 96283300 ns
---------------------------------------------
ns         %     Task name
---------------------------------------------
016196200  017%  ij
080087100  083%  ji
```

可以看到 ij 的效率比 ji 快很多，为什么呢？

* 缓存是有限的，当新数据来了后，一些旧的缓存行数据就会被覆盖
* 如果不能充分利用缓存的数据，就会造成效率低下

以 ji 执行为例，第一次内循环要读入 $[0,0]$ 这条数据，由于局部性原理，读入 $[0,0]$ 的同时也读入了 $[0,1] ... [0,13]$，如图所示

![image-20221104164329026](.\imgs\image-20221104164329026.png)

但很遗憾，第二次内循环要的是 $[1,0]$ 这条数据，缓存中没有，于是再读入了下图的数据

![image-20221104164716282](.\imgs\image-20221104164716282.png)

这显然是一种浪费，因为 $[0,1] ... [0,13]$ 包括 $[1,1] ... [1,13]$ 这些数据虽然读入了缓存，却没有及时用上，而缓存的大小是有限的，等执行到第九次内循环时

![image-20221104164947154](.\imgs\image-20221104164947154.png)

缓存的第一行数据已经被新的数据 $[8,0] ... [8,13]$ 覆盖掉了，以后如果再想读，比如 $[0,1]$，又得到内存去读了

同理可以分析 ij 函数则能充分利用局部性原理加载到的缓存数据

**举一反三**

1. I/O 读写时同样可以体现局部性原理

2. 数组可以充分利用局部性原理，那么链表呢？

   > 答：链表不行，因为链表的元素并非相邻存储



### 5) 越界检查

java 中对数组元素的读写都有越界检查，类似于下面的代码

```c++
bool is_within_bounds(int index) const        
{ 
    return 0 <= index && index < length(); 
}
```

* 代码位置：`openjdk\src\hotspot\share\oops\arrayOop.hpp`

只不过此检查代码，不需要由程序员自己来调用，JVM 会帮我们调用

### 习题

#### E01. 合并有序数组 - 对应 Leetcode 88

将数组内两个区间内的有序元素合并

例

```
[1, 5, 6, 2, 4, 10, 11]
```

可以视作两个有序区间

```
[1, 5, 6] 和 [2, 4, 10, 11]
```

合并后，结果仍存储于原有空间

```
[1, 2, 4, 5, 6, 10, 11]
```



**方法1**

递归

* 每次递归把更小的元素复制到结果数组

```java
merge(left=[1,5,6],right=[2,4,10,11],a2=[]){
    merge(left=[5,6],right=[2,4,10,11],a2=[1]){
        merge(left=[5,6],right=[4,10,11],a2=[1,2]){
            merge(left=[5,6],right=[10,11],a2=[1,2,4]){
                merge(left=[6],right=[10,11],a2=[1,2,4,5]){
                    merge(left=[],right=[10,11],a2=[1,2,4,5,6]){
						// 拷贝10，11
                    }
                }
            }
        }
    }
}
```

代码

```java
public static void merge(int[] a1, int i, int iEnd, int j, int jEnd,
                              int[] a2, int k) {
    if (i > iEnd) {
        System.arraycopy(a1, j, a2, k, jEnd - j + 1);
        return;
    }
    if (j > jEnd) {
        System.arraycopy(a1, i, a2, k, iEnd - i + 1);
        return;
    }
    if (a1[i] < a1[j]) {
        a2[k] = a1[i];
        merge(a1, i + 1, iEnd, j, jEnd, a2, k + 1);
    } else {
        a2[k] = a1[j];
        merge(a1, i, iEnd, j + 1, jEnd, a2, k + 1);
    }
}
```

测试

```java
int[] a1 = {1, 5, 6, 2, 4, 10, 11};
int[] a2 = new int[a1.length];
merge(a1, 0, 2, 3, 6, a2, 0);
```



**方法2**

代码

```java
public static void merge(int[] a1, int i, int iEnd,
                             int j, int jEnd,
                             int[] a2) {
    int k = i;
    while (i <= iEnd && j <= jEnd) {
        if (a1[i] < a1[j]) {
            a2[k] = a1[i];
            i++;
        } else {
            a2[k] = a1[j];
            j++;
        }
        k++;
    }
    if (i > iEnd) {
        System.arraycopy(a1, j, a2, k, jEnd - j + 1);
    }
    if (j > jEnd) {
        System.arraycopy(a1, i, a2, k, iEnd - i + 1);
    }
}
```

测试

```java
int[] a1 = {1, 5, 6, 2, 4, 10, 11};
int[] a2 = new int[a3.length];
merge(a1, 0, 2, 3, 6, a2);
```

#### leetcode 27 Remove Element
如何删除数组中的一个元素
- 数组的元素在内存地址中是连续的，不能单独删除数组中的某个元素，只能覆盖，后面的元素前移

#### leetcode59 [Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/)
关键点：
1.每条边处理原则都是左闭右开 -- 循环不变量
2.总共需要转几圈？ -- while (loop <=n/2)
因为转一圈四条边，平均到长度和宽度，各消耗2
3.

需要记录的：
1.每一圈的起始点
2.每一圈减去的长度
3.当前圈数

#### leetcode54 [spiral matrix](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0054.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5.md)
## 2.2 链表

### 1) 概述

**定义**

在计算机科学中，链表是数据元素的线性集合，其每个元素都指向下一个元素，元素存储上并不连续

> In computer science, a **linked list** is a linear collection of data elements whose order is not given by their physical placement in memory. Instead, each element points to the next. 



可以分类为[^5]

* 单向链表，每个元素只知道其下一个元素是谁

* 双向链表，每个元素知道其上一个元素和下一个元素

* 循环链表，通常的链表尾节点 tail 指向的都是 null，而循环链表的 tail 指向的是头节点 head

![l1.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/l1.PNG)

链表内还有一种特殊的节点称为哨兵（Sentinel）节点，也叫做哑元（ Dummy）节点，它不存储数据，通常用作头尾，用来简化边界判断，如下图所示

![image-20221110084611550.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20221110084611550.png)

**随机访问性能**

根据 index 查找，时间复杂度 $O(n)$

**插入或删除性能**

* 起始位置：$O(1)$
* 结束位置：如果已知 tail 尾节点（双向链表）是 $O(1)$，不知道 tail 尾节点（单项链表）是 $O(n)$
* 中间位置：根据 index 查找时间 + $O(1)$

![L2.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/L2.PNG)
链表的插入操作，只需要改变插入部分指针的位置，和插入顺序没有关系，因此时间复杂度是O(1)

而数组的插入删除
- 数组在内存中是**连续存储**的。
- 如果你在中间插入一个元素（比如索引 2 处），那么你需要将后面的所有元素往后挪一位，时间复杂度是 $O(n)$。
- 删除元素也一样，需要把后面的元素往前挪。

| 操作位置 | 数组复杂度         | 链表复杂度               |
| ---- | ------------- | ------------------- |
| 末尾插入 | $O(1)$（有预留空间） | $O(1)$（如果有 tail）    |
| 开头插入 | $O(n)$        | $O(1)$              |
| 中间插入 | $O(n)$（元素移动）  | $O(n) + O(1)$（先找再插） |
### 2) 单向链表

根据单向链表的定义，首先定义一个存储 value 和 next 指针的类 Node，和一个描述头部节点的引用
#### addFirst
1.链表为空
![l3.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/l3.PNG)

2.链表非空
![l4.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/l4.PNG)

```java
package com.itheima.datastructure;  
  
public class SinglyLinkedList {  
    private Node head = null;// head指针  
  
    private static class Node {  
        int value;  
        Node next; //下一个节点指针  
  
        //节点类  
        private Node(int value, Node next) {  
            this.value = value;  
            this.next = next;  
        }  
    }  
  
    public void addFirst(int value) {  
        // 1.链表为空  
//        head = new Node(value, null);  
        // 2.链表非空  
        head = new Node(value, head);  
    }  
}
```

* Node 定义为内部类，是为了对外**隐藏**实现细节，没必要让类的使用者关心 Node 结构
* 定义为 static 内部类，是因为 Node **不需要**与 SinglyLinkedList 实例相关，多个 SinglyLinkedList实例能共用 Node 类定义
* **head 指的是链表的“头指针”，它本身指向第一个节点 node1。**  
    所以：
    - `head` 是 `node1`
    - `head.next` 是 `node1.next`，也就是 `node2`

#### 遍历

```java
public class SinglyLinkedList implements Iterable<Integer>{  
    private Node head = null;// head指针  
//1.迭代器遍历
    @Override  
    public Iterator<Integer> iterator() {  
        return new Iterator<Integer>() {  
            Node p = head;  
  
            @Override  
            public boolean hasNext() {  
                return p != null;  
            }  
  
            @Override  
            public Integer next() {  
                int v = p.value;  
                p = p.next;  
                return v;  
            }  
        };  
    }  
  
    private static class Node {  
        int value;  
        Node next; //下一个节点指针  
  
        //节点类  
        private Node(int value, Node next) {  
            this.value = value;  
            this.next = next;  
        }  
    }  
  
    public void addFirst(int value) {  
        // 1.链表为空  
//        head = new Node(value, null);  
        // 2.链表非空  
        head = new Node(value, head);  
    }  
//2.while遍历
    public void loop1(Consumer<Integer> consumer) {  
        Node p = head;  
        while (p != null) {  
            consumer.accept(p.value);  
            p = p.next;  
        }  
    }  

//3.for遍历
    public void loop2(Consumer<Integer> consumer) {  
        for(Node p = head; p != null; p = p.next) {  
            consumer.accept(p.value);  
        }  
    }  
}
```

```java
public class TestSinglyLinkedList {  
    @Test  
    public void test1() {  
        SinglyLinkedList list = new SinglyLinkedList();  
        list.addFirst(1);  
        list.addFirst(2);  
        list.addFirst(3);  
        list.addFirst(4);  
  
        list.loop2(value -> System.out.println(value));
        /*list.loop1(new Consumer<Integer>() {// 相当于  
        @Override    
        public void accept(Integer value) {        
        System.out.println(value);    
        }});*/  

  
    @Test  
    public void test2() {  
        SinglyLinkedList list = new SinglyLinkedList();  
        list.addFirst(1);  
        list.addFirst(2);  
        list.addFirst(3);  
        list.addFirst(4);  
  
        for (Integer value : list) {  
            System.out.println(value);  
        }  
    }  
}
```

#### 递归遍历

```java
public class SinglyLinkedList implements Iterable<Integer> {
    // ...
    public void loop() {
        recursion(this.head);
    }

    private void recursion(Node curr) {
        if (curr == null) {
            return;
        }
        // 前面做些事
        recursion(curr.next);
        // 后面做些事
    }
}
```



#### 尾部添加

```java
public class SinglyLinkedList {
    // ...
    private Node findLast() {
        if (this.head == null) {
            return null;
        }
        Node curr;
        for (curr = this.head; curr.next != null; ) {
            curr = curr.next;
        }
        return curr;
    }
    
    public void addLast(int value) {
        Node last = findLast();
        if (last == null) {
            addFirst(value);
            return;
        }
        last.next = new Node(value, null);
    }
}
```

* 注意，找最后一个节点，终止条件是 curr.next == null 
* 分成两个方法是为了代码清晰，而且 findLast() 之后还能复用



#### 尾部添加多个

```java
public class SinglyLinkedList {
    // ...
	public void addLast(int first, int... rest) {
        
        Node sublist = new Node(first, null);
        Node curr = sublist;
        for (int value : rest) {
            curr.next = new Node(value, null);
            curr = curr.next;
        }
        
        Node last = findLast();
        if (last == null) {
            this.head = sublist;
            return;
        }
        last.next = sublist;
    }
}
```
* 先串成一串 sublist
* 再作为一个整体添加
#### 根据索引获取

```java
public class SinglyLinkedList {
    // ...
	private Node findNode(int index) {
        int i = 0;
        for (Node curr = this.head; curr != null; curr = curr.next, i++) {
            if (index == i) {
                return curr;
            }
        }
        return null;
    }
    
    private IllegalArgumentException illegalIndex(int index) {
        return new IllegalArgumentException(String.format("index [%d] 不合法%n", index));
    }
    
    public int get(int index) {
        Node node = findNode(index);
        if (node != null) {
            return node.value;
        }
        throw illegalIndex(index);
    }
}
```

* 同样，分方法可以实现复用



#### 插入

```java
public class SinglyLinkedList {
    // ...
	public void insert(int index, int value) {
        if (index == 0) {
            addFirst(value);
            return;
        }
        Node prev = findNode(index - 1); // 找到上一个节点
        if (prev == null) { // 找不到
            throw illegalIndex(index);
        }
        prev.next = new Node(value, prev.next);
    }
}
```

* 插入包括下面的删除，都必须找到上一个节点


#### removefirst
```java
public void removeFirst() {  
    if (head == null) {  
        throw illegalIndex(0);  
    }  
    head = head.next;  
}
```
#### remove

```java
public class SinglyLinkedList {
    // ...
	public void remove(int index) {
        if (index == 0) {
            if (this.head != null) {
                this.head = this.head.next;
                return;
            } else {
                throw illegalIndex(index);
            }
        }
        Node prev = findNode(index - 1);
        Node curr;
        if (prev != null && (curr = prev.next) != null) {
            prev.next = curr.next;
        } else {
            throw illegalIndex(index);
        }
    }
}
```

* 第一个 if 块对应着 removeFirst 情况
* 最后一个 if 块对应着至少得两个节点的情况
  * 不仅仅判断上一个节点非空，还要保证当前节点非空



### 3) 单向链表（带哨兵）

观察之前单向链表的实现，发现每个方法内几乎都有判断是不是 head 这样的代码，能不能简化呢？

用一个不参与数据存储的特殊 Node 作为哨兵，它一般被称为哨兵或哑元，拥有哨兵节点的链表称为带头链表

```java
public class SinglyLinkedListSentinel {
    // ...
    private Node head = new Node(Integer.MIN_VALUE, null);
}
```

* 具体存什么值无所谓，因为不会用到它的值

加入哨兵节点后，代码会变得比较简单，先看几个工具方法

```java
public class SinglyLinkedListSentinel {
    // ...
    
    // 根据索引获取节点
    private Node findNode(int index) {
        int i = -1;
        for (Node curr = this.head; curr != null; curr = curr.next, i++) {
            if (i == index) {
                return curr;
            }
        }
        return null;
    }
    
    // 获取最后一个节点
    private Node findLast() {
        Node curr;
        for (curr = this.head; curr.next != null; ) {
            curr = curr.next;
        }
        return curr;
    }
}
```

* findNode 与之前类似，只是 i 初始值设置为 -1 对应哨兵，实际传入的 index 也是 $[-1, \infty)$
* findLast 绝不会返回 null 了，就算没有其它节点，也会返回哨兵作为最后一个节点



这样，代码简化为

```java
public class SinglyLinkedListSentinel {
    // ...
    
    public void addLast(int value) {
        Node last = findLast();
        /*
        改动前
        if (last == null) {
            this.head = new Node(value, null);
            return;
        }
        */
        last.next = new Node(value, null);
    }
    
    public void insert(int index, int value) {
        /*
        改动前
        if (index == 0) {
            this.head = new Node(value, this.head);
            return;
        }
        */
        // index 传入 0 时，返回的是哨兵
        Node prev = findNode(index - 1);
        if (prev != null) {
            prev.next = new Node(value, prev.next);
        } else {
            throw illegalIndex(index);
        }
    }
    
    public void remove(int index) {
        /*
        改动前
        if (index == 0) {
            if (this.head != null) {
                this.head = this.head.next;
                return;
            } else {
                throw illegalIndex(index);
            }
        }
        */
        // index 传入 0 时，返回的是哨兵
        Node prev = findNode(index - 1);
        Node curr;
        if (prev != null && (curr = prev.next) != null) {
            prev.next = curr.next;
        } else {
            throw illegalIndex(index);
        }
    }
    
    public void addFirst(int value) {
        /*
        改动前
        this.head = new Node(value, this.head);
        */
		this.head.next = new Node(value, this.head.next);
        // 也可以视为 insert 的特例, 即 insert(0, value);
    }
}
```

* 对于删除，前面说了【最后一个 if 块对应着至少得两个节点的情况】，现在有了哨兵，就凑足了两个节点



### 4) 双向链表（带哨兵）

```java
public class DoublyLinkedListSentinel implements Iterable<Integer> {

    private final Node head;
    private final Node tail;

    public DoublyLinkedListSentinel() {
        head = new Node(null, 666, null);
        tail = new Node(null, 888, null);
        head.next = tail;
        tail.prev = head;
    }

    private Node findNode(int index) {
        int i = -1;
        for (Node p = head; p != tail; p = p.next, i++) {
            if (i == index) {
                return p;
            }
        }
        return null;
    }

    public void addFirst(int value) {
        insert(0, value);
    }

    public void removeFirst() {
        remove(0);
    }

    public void addLast(int value) {
        Node prev = tail.prev;
        Node added = new Node(prev, value, tail);
        prev.next = added;
        tail.prev = added;
    }

    public void removeLast() {
        Node removed = tail.prev;
        if (removed == head) {
            throw illegalIndex(0);
        }
        Node prev = removed.prev;
        prev.next = tail;
        tail.prev = prev;
    }

    public void insert(int index, int value) {
        Node prev = findNode(index - 1);
        if (prev == null) {
            throw illegalIndex(index);
        }
        Node next = prev.next;
        Node inserted = new Node(prev, value, next);
        prev.next = inserted;
        next.prev = inserted;
    }

    public void remove(int index) {
        Node prev = findNode(index - 1);
        if (prev == null) {
            throw illegalIndex(index);
        }
        Node removed = prev.next;
        if (removed == tail) {
            throw illegalIndex(index);
        }
        Node next = removed.next;
        prev.next = next;
        next.prev = prev;
    }

    private IllegalArgumentException illegalIndex(int index) {
        return new IllegalArgumentException(
                String.format("index [%d] 不合法%n", index));
    }

    @Override
    public Iterator<Integer> iterator() {
        return new Iterator<Integer>() {
            Node p = head.next;

            @Override
            public boolean hasNext() {
                return p != tail;
            }

            @Override
            public Integer next() {
                int value = p.value;
                p = p.next;
                return value;
            }
        };
    }

    static class Node {
        Node prev;
        int value;
        Node next;

        public Node(Node prev, int value, Node next) {
            this.prev = prev;
            this.value = value;
            this.next = next;
        }
    }
}
```



### 5) 环形链表（带哨兵）

双向环形链表带哨兵，这时哨兵**既作为头，也作为尾**

![image-20221229144232651](./imgs/image-20221229144232651.png)

![image-20221229143756065](./imgs/image-20221229143756065.png)

![image-20221229153338425](./imgs/image-20221229153338425.png)

![image-20221229154248800](./imgs/image-20221229154248800.png)

参考实现

```java
public class DoublyLinkedListSentinel implements Iterable<Integer> {

    @Override
    public Iterator<Integer> iterator() {
        return new Iterator<>() {
            Node p = sentinel.next;

            @Override
            public boolean hasNext() {
                return p != sentinel;
            }

            @Override
            public Integer next() {
                int value = p.value;
                p = p.next;
                return value;
            }
        };
    }

    static class Node {
        Node prev;
        int value;
        Node next;

        public Node(Node prev, int value, Node next) {
            this.prev = prev;
            this.value = value;
            this.next = next;
        }
    }

    private final Node sentinel = new Node(null, -1, null); // 哨兵

    public DoublyLinkedListSentinel() {
        sentinel.next = sentinel;
        sentinel.prev = sentinel;
    }

    /**
     * 添加到第一个
     * @param value 待添加值
     */
    public void addFirst(int value) {
        Node next = sentinel.next;
        Node prev = sentinel;
        Node added = new Node(prev, value, next);
        prev.next = added;
        next.prev = added;
    }

    /**
     * 添加到最后一个
     * @param value 待添加值
     */
    public void addLast(int value) {
        Node prev = sentinel.prev;
        Node next = sentinel;
        Node added = new Node(prev, value, next);
        prev.next = added;
        next.prev = added;
    }
    
    /**
     * 删除第一个
     */
    public void removeFirst() {
        Node removed = sentinel.next;
        if (removed == sentinel) {
            throw new IllegalArgumentException("非法");
        }
        Node a = sentinel;
        Node b = removed.next;
        a.next = b;
        b.prev = a;
    }

    /**
     * 删除最后一个
     */
    public void removeLast() {
        Node removed = sentinel.prev;
        if (removed == sentinel) {
            throw new IllegalArgumentException("非法");
        }
        Node a = removed.prev;
        Node b = sentinel;
        a.next = b;
        b.prev = a;
    }

    /**
     * 根据值删除节点
     * <p>假定 value 在链表中作为 key, 有唯一性</p>
     * @param value 待删除值
     */
    public void removeByValue(int value) {
        Node removed = findNodeByValue(value);
        if (removed != null) {
            Node prev = removed.prev;
            Node next = removed.next;
            prev.next = next;
            next.prev = prev;
        }
    }

    private Node findNodeByValue(int value) {
        Node p = sentinel.next;
        while (p != sentinel) {
            if (p.value == value) {
                return p;
            }
            p = p.next;
        }
        return null;
    }
}
```

### 习题
#### 学会自己定义ListNode
```java
public class ListNode {
    // 结点的值
    int val;

    // 下一个结点
    ListNode next;

    // 节点的构造函数(无参)
    public ListNode() {
    }

    // 节点的构造函数(有一个参数)
    public ListNode(int val) {
        this.val = val;
    }

    // 节点的构造函数(有两个参数)
    public ListNode(int val, ListNode next) {
        this.val = val;
        this.next = next;
    }
}
```
#### 707 design linked list

```java
class MyLinkedList {

    class ListNode {
        int val;
        ListNode next;
        ListNode(int val) {
            this.val = val;
        }
    }

    private ListNode dummyhead; // 虚拟头节点
    private int size;           // 当前链表长度

    public MyLinkedList() {
        this.dummyhead = new ListNode(0); // dummyhead 不存值，只做哨兵节点
        this.size = 0;
    }

    // 获取第 index 个节点的值（0-based），非法返回 -1
    public int get(int index) {
        if (index < 0 || index >= size) return -1;

        ListNode cur = dummyhead.next;
        while (index > 0) {
            cur = cur.next;
            index--;
        }
        return cur.val;
    }

    // 在头部插入
    public void addAtHead(int val) {
        ListNode newnode = new ListNode(val);
        newnode.next = dummyhead.next;
        dummyhead.next = newnode;
        size++;
    }

    // 在尾部插入
    public void addAtTail(int val) {
        ListNode cur = dummyhead;
        while (cur.next != null) {
            cur = cur.next;
        }
        cur.next = new ListNode(val);
        size++;
    }

    // 在第 index 个位置插入节点（0-based）
    // 若 index == size，则相当于尾插；若 index < 0，则相当于头插
    public void addAtIndex(int index, int val) {
        if (index < 0 || index > size) return;

        ListNode cur = dummyhead;
        while (index > 0) {
            cur = cur.next;
            index--;
        }
        ListNode newnode = new ListNode(val);
        newnode.next = cur.next;
        cur.next = newnode;
        size++;
    }

    // 删除第 index 个节点
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) return;

        ListNode cur = dummyhead;
        while (index > 0) {
            cur = cur.next;
            index--;
        }
        cur.next = cur.next.next;
        size--;
    }
}
```
#### 反转单向链表-Leetcode 206


对应力扣题目 [206. 反转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list/)

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]

输入：[1,2]
输出：[2,1]

输入：[]
输出：[]
```
##### ListNode
题中给的定义
```java
public class ListNode {
    int val; // 当前节点值
    ListNode next; // ❓为什么它的类型是自己 ListNode？
}
```
这是一个**链表节点**类，每个节点需要**知道“下一个节点”是谁**，所以就用同样类型的字段 `next` 来指向下一个 `ListNode`。
✅这叫**递归数据结构**,定义里包含自己的类型，这在数据结构里很常见

❓会不会造成无限递归/死循环？
不会的，因为每个字段只是**一个引用（指针）**，不是直接嵌套一个新对象。

##### 标准方法1 双指针
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode cur = head;
        ListNode pre = null;
        ListNode temp = null;
        while(cur != null) {
            temp = cur.next; // 先保存下一步临时指针要走的方向
            cur.next = pre; // 反转链表，改变当前节点的方向，不影响temp里存着的下一个节点
            pre = cur; // 一定是先移动pre
            cur = temp; //再移动cur
        }
        return pre;
    }
}
```

##### 标准方法2 递归
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        return reverse(head,null);
    }
    private ListNode reverse(ListNode cur, ListNode pre) {
        if (cur == null) return pre;
        ListNode temp = cur.next;
        cur.next = pre;
        return reverse(temp, cur);
    }
}
```

#### 根据值删除节点-Leetcode 203

例如

```
输入：head = [1,2,6,3,6], val = 6
输出：[1,2,3]

输入：head = [], val = 1
输出：[]

输入：head = [7,7,7,7], val = 7
输出：[]
```

##### 方法1 原链表删除元素
```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
    // 删除头节点，直接下移就行
        while (head != null && head.val == val) {
            head = head.next; 
        }

        ListNode cur = head;//定义一个指针指向head
        while (cur != null && cur.next != null) {//如果cur = null,cur.next会抛异常
            if (cur.next.val == val) {
                cur.next = cur.next.next;
            } else {
                cur = cur.next;
            }
        }
        return head;
    }
}
```
##### 方法2 使用虚拟头结点
```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummyhead = new ListNode();
        dummyhead.next = head;
        ListNode cur = dummyhead;
        while (cur.next != null) {
            if (cur.next.val == val) {
               cur.next = cur.next.next;
            } else {
                cur = cur.next;
            }
        }
        return dummyhead.next;  
    }
}
```

#### leetcode142环形链表
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;

            if (fast == slow) {
                ListNode index1 = fast;
                ListNode index2 = head;

                while (index1 != index2) {
                    index1 = index1.next;
                    index2 = index2.next;
                }
                return index1;
            } 
        }

        return null; // ✅ 循环跑完了都没相遇，说明没有环
    }
}
```

#### leetcode24 swap every two adjacent nodes

思路： 
两两分组
虚拟头节点 cur指向虚拟头节点，这样才能操作指向第一个节点的指针
什么时候结束 接下来两个都为空
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */

class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummyhead = new ListNode();
        dummyhead.next = head;
        ListNode cur = dummyhead;
        while (cur.next != null && cur.next.next != null) { 
        //要保证有一对节点可以交换，即当前 cur后面至少还有两个节点，而且必须cur.next.next写在后面，这样万一cur.next已经为空，cur.next.next会报空指针异常
            ListNode first = cur.next;
            ListNode second = cur.next.next;
            ListNode third = second.next;

            // swap
            cur.next = second;
            second.next = first;
            first.next = third;
            cur = first; // 继续处理下一对
        }

        return dummyhead.next;
    }
}
```

## 2.3哈希表
如果在做面试题目的时候遇到需要判断一个元素是否出现过的场景也应该第一时间想到哈希法！
### 习题
#### leetcode 349 返回两个数组的交集 hashset

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        // 把 nums1 的所有数字放进 set1
        Set<Integer> set1 = new HashSet<>();
        Set<Integer> resultSet = new HashSet<>();
        for (int i : nums1) {
            set1.add(i);
        }
        // 如果 nums2 中的数字在 nums1 中出现过，加入结果集
        for (int j :nums2) {
            if(set1.contains(j)) {
                resultSet.add(j);
            }
        }
        // 把结果集转成 int[] 数组返回
        int[] result = new int[resultSet.size()];
        int i = 0;
        for(int num : resultSet) {
            result[i] = num;
            i++;
        }
        return result;   
    }
}
```
#### leetcode [2824. Count Pairs Whose Sum is Less than Target](https://leetcode.com/problems/count-pairs-whose-sum-is-less-than-target/)
##### 双指针法

```java
class Solution {
    public int countPairs(List<Integer> nums, int target) {
        int res = 0;
        Collections.sort(nums);
        int size = nums.size();
        int l = 0;
        int r = size - 1;
        while (l < r) {
            int sum = nums.get(l) + nums.get(r);
            if (sum < target) {
                res += r - l;
                l++;
            } else {
                r--;
            }
        }
        return res;
    }
}
```

##### 如果去重，可以使用hashset去重

```java
class Solution {
    public int countPairs(List<Integer> nums, int target) {
        Collections.sort(nums);
        Set<List<Integer>> set = new HashSet<>();
        int l = 0, r = nums.size() - 1;

        while (l < r) {
            int sum = nums.get(l) + nums.get(r);
            if (sum < target) {
                set.add(Arrays.asList(nums.get(l), nums.get(r)));
                l++;
            } else {
                r--;
            }
        }
        return set.size(); // 返回去重后的有效 pair 数量
    }
}
```

为什么选择`Set<List<Integer>>`
- `List<Integer>` 表示一个“数对”，如 `[nums[i], nums[j]]`，例如：
```java
[1, 2]
[3, 4]
```
- `Set` 可以自动去掉重复的 List，比如 `[1,2]` 和 `[1,2]` 只保留一个,
```java
[
  [1, 2],
  [3, 4]
]
```

和 Set<int[ ]> 有什么区别？
```java
Set<int[]> set = new HashSet<>();
set.add(new int[]{1, 2});
set.add(new int[]{1, 2});
System.out.println(set.size()); // ❌ 是 2，不是 1！
```
这是因为 `int[]` 没有实现 `.equals()` 和 `.hashCode()`，所以两个数组即使内容一样也不会被认为是“重复”。
而 `List<Integer>`（如 `ArrayList`）已经帮我们实现了正确的比较。

#### leetcode 350 返回两个数组的交集 hashmap
```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Map<Integer, Integer> map = new HashMap<>();
        // 统计nums1中数字出现的次数,key是数字，value是出现次数
        for (int i : nums1) {
            if (map.containsKey(i)) {
                map.put(i, map.get(i) + 1);
            } else {
                map.put(i, 1);
            }
        }
        // ArrayList 内部可以自动扩容，而数组不行
        List<Integer> result = new ArrayList<>();
        //检测nums2,每次遇到一个在nums1中还没“用完”的元素，就放到结果里，减少一次配额
        for (int j : nums2) {
            if (map.containsKey(j) && map.get(j) > 0) {
                result.add(j);
                map.put(j, map.get(j) - 1);
            }
        }
  
        int[] resArray = new int[result.size()];
        for (int k = 0; k < resArray.length; k++) {
            resArray[k] = result.get(k);
        }

        return resArray;
    }
}
```

#### leetcode 454四数相加Ⅱ
```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        int res = 0;
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        //统计两个数组中的元素之和，同时统计出现的次数，放入map
        for (int i : nums1) {
            for (int j : nums2) {
                int sum = i + j;
                map.put(sum, map.getOrDefault(sum, 0) + 1);
            }
        }
        //统计剩余的两个元素的和，在map中找是否存在相加为0的情况，同时记录次数
        for (int i : nums3) {
            for (int j : nums4) {
                res += map.getOrDefault(0 - i - j, 0);
            }
        }
        return res;
    }
}
```

为什么最后能直接返回res?

#### leetcode[15. 3Sum](https://leetcode.com/problems/3sum/)
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);
        int n = nums.length;
        for (int i = 0; i < n - 2 ; i++) {
          //最后两个位置留给left和right
          int x = nums[i];
          if (i > 0 && x == nums[i - 1]) continue; //对第一个数字去重
          if (x + nums[i + 1] + nums[i + 2] > 0) break;
          if (x + nums[n - 2] + nums[n - 1] < 0) continue;
          int l = i + 1;
          int r = n - 1;
          while (l < r) {
            int s = x + nums[l] + nums[r];
            if (s > 0) {
                r--;
            } else if (s < 0) {
                l++;
            } else {
                res.add(List.of(x, nums[l], nums[r]));
                while(r > l && nums[r] == nums[r - 1]) r--; // 去重r
                while(r > l && nums[l] == nums[l + 1]) l++; // 去重l
                l++;//继续找新的可能
                r--;
            }
          }
        }
        return res;
    }
}
```





2.定义两遍 r > l
“外层 while”只管整个双指针循环，而现在是在 while 循环体内处理“重复值去重”**，这时 `l` 和 `r` 的值已经变动过了，**必须重新判断是否交叉（l >= r）**，否则就可能越界。
#### leetcode18 四数之和

| 项目           | 三数之和 `3Sum`                          | 四数之和 `4Sum`                                                  | 共通点说明             |
| ------------ | ------------------------------------ | ------------------------------------------------------------ | ----------------- |
| **排序**       | 需要排序                                 | 需要排序                                                         | 排序是前提，确保双指针能工作    |
| **目标和**      | 固定一个数 `x = nums[i]`，再找 `b + c == -x` | 固定两个数 `x = nums[k], y = nums[i]`，找 `c + d == target - x - y` | 多固定一个数            |
| **双指针**      | 一层 for + 一层 while(left < right)      | 两层 for + 一层 while(left < right)                              | 外部 for 固定，内部双指针推进 |
| **去重逻辑**     | 对 `i`、`left`、`right` 去重              | 对 `k`、`i`、`left`、`right` 去重                                  | 全部都需要跳过重复值        |
| **时间复杂度**    | `O(n²)`                              | `O(n³)`                                                      | 多嵌套一层循环           |
| **剪枝优化（可选）** | 提前终止不可能满足条件的分支                       | 同样可剪枝（例如：当前值 + 最小值组合 > target）                               | 排序后可做剪枝判断         |
```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> res = new ArrayList<>();
        int n = nums.length;
        Arrays.sort(nums);
        for (int k = 0; k < n - 3; k++) { // 第1层：固定 nums[k]
            if (nums[k] > target && target >= 0) break;
            if (k > 0 && nums[k] == nums[k - 1]) continue;
            
            for (int i = k + 1; i < n - 2; i++) {// 第2层：固定 nums[i]
                if (nums[k] + nums[i] > target && target >= 0) break;
                if (i > k + 1 && nums[i] == nums[i - 1]) continue;
            int l = i + 1;
            int r = n - 1;
  
            while (l < r) { // 第3层：双指针夹逼
                long s = (long)nums[k] + nums[i] + nums[l] + nums[r];//特别注意，防止溢出
                if (s > target) {
                    r--;
                } else if (s < target){
                    l++;
                } else {
                    res.add(List.of(nums[k],nums[i],nums[l],nums[r]));
                    while (r > l && nums[r] == nums[r - 1]) r--;
                    while (r > l && nums[l] == nums[l + 1]) l++;
                    l++; // 下一轮组合
                    r--;
                }
            }  
        }
     }
        return res;
    }
}
```

总结：“**前两位定死，后两位夹逼，找到就去重，再推进搜索**”

#### leetcode [16. 3Sum Closest](https://leetcode.com/problems/3sum-closest/)
```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int n = nums.length;
        int minDiff = Integer.MAX_VALUE;
        int closestSum = 0;

        for (int i = 0; i < n - 2; i++) {
            int x = nums[i];
            int l = i + 1;
            int r = n - 1;

            while (l < r) {
                int s = x + nums[l] + nums[r];
                int diff = Math.abs(s - target);
  

                if (diff < minDiff) {
                    minDiff = diff;
                    closestSum = s;
                }

                if (s < target) {
                    l++;
                } else if (s > target) {
                    r--;
                } else {
                    return target;
                }
            }
        }
        return closestSum;
    }

}
```

## 2.4字符串
### char和string
char 是基本类型，单个字符
string是引用类型，多个字符，**不可变**
char[]是引用类型，是数组，无论是 `char[]`、`int[]`、`String[]` 都是。char[] arr = {'a', 'b', 'c'};

| 类型                                  | 类别   | 示例     |
| ----------------------------------- | ---- | ------ |
| `int`, `char`, `boolean`            | 基本类型 | 值本身存栈上 |
| `int[]`, `char[]`, `String`, `List` | 引用类型 | 存的是堆地址 |
char和string的转化
```java
// ✅ char → String
String s1 = String.valueOf('a'); // 推荐
String s2 = Character.toString('b');

// String → char（单个字符）
char c1 = "abc".charAt(0); //取第一个字符

// ✅String → char[]（转数组）
char[] arr = "hello".toCharArray();

// ✅char[] → String （数组变string）
String s4 = new String(arr);
String s5 = String.valueOf(arr);
```

### 引用类型和基本类型的返回
1.如果我们想定义一个reverse
```java
public void reverse(char[] ch, int i, int j) {
    for (; i < j; i++, j--) {
        char temp  = ch[i];
        ch[i] = ch[j];
        ch[j] = temp;
    }
```
✔️ 因为数组是引用类型，函数体内的修改会影响原数组，所以可以 `void`，不返回也没问题。

 2. 基本类型 + `void`（就不行？❌）
**其实也可以用 `void`，只是值传递不会改变原值**：
```java
public void changeInt(int x) {
    x = 10; // 改变的是副本，对原始变量无影响
}
```
这个时候不返回 `x` 就等于什么都没发生，调用方根本拿不到新值：
```java
int a = 5;
changeInt(a); // a 仍然是 5
```
所以对于基本类型，如果你想“改变值”，**必须返回结果**，比如：
```java
public int changeInt(int x) {
    return x + 1;
}
```
3.总结
> 是否能用 `void`，**取决于你有没有需要返回的结果**，而不是参数是不是引用类型。
- 引用类型可以 `void`，因为你改的就是原数据。
- 基本类型用 `void` 会“白改”，因为是值拷贝，不会生效，所以通常会返回新值。
### 习题
#### leetcode 344 reverse string
```java
class Solution {
    public void reverseString(char[] s) {
        int n = s.length;
        int l = 0, r = n - 1;
        while(l < r) {
           char temp = s[l];
           s[l] = s[r];
           s[r] = temp;
           l++;
           r--;
        }
    }
}
```

#### leetcode 541 reverse stringⅡ
##### 分析思路：
1. string不可变，先转换为char[ ]
2. 编写reverse函数，双指针交换，反转指定区间。这里的char[]可以变，无须返回对象
3. 开始让i遍历。在遍历字符串的过程中，只要让 i += (2 * k)，i 每次移动 2 * k 就可以了，判断是否能反转 `k` 个字符，如果不足 `k`，就反转到末尾
4. 把 `char[]` 转回 `String` 返回

##### 数字的`数组的 i 到 i+k-1` 区间模版
```java
Math.min(i + k - 1, arr.length - 1) 
```
**只在最后一次** 有可能触发 `ch.length - 1`，其余时候就是 `i + k - 1`。这样就可以对每段区间做判断，避免讨论整个ch

##### 代码：
```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] ch = s.toCharArray();
        for (int i = 0; i < ch.length; i += 2*k) {
            int end = Math.min(i + k - 1, ch.length - 1);
            reverse(ch, i, end);
        }
        return new String(ch);
    }

    public void reverse(char[] ch, int i, int j) {
        for (; i < j; i++, j--) {
            char temp = ch[i];
            ch[i] = ch[j];
            ch[j] = temp;
        }
    }
}
```

#### 541变种题
对于字符串 `s` 和整数 `k`，**每隔 k 个字符就反转 k 个字符**（不是反转前 k，而是 **跳过 k，再反转 k**，如此反复）
例如
```java
s = "abcdefghijk", k = 2

操作过程：
跳过 a b → 反转 c d → 跳过 e f → 反转 g h → 跳过 i j → 反转 k

结果是："abdcfehgjik"
```
从下标 `i = k` 开始，每隔 `2k` 步，反转一段长度为 `k` 的字符。
```java
for (int i = k; i < ch.length; i += 2 * k) {
    int end = Math.min(i + k - 1, ch.length - 1);
    reverse(ch, i, end);
}
```

#### leetcode[151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

快慢指针
fast遍历整个数组，读取字符
slow把有效字符写入前面位置

```java
class Solution {
    public String reverseWords(String s) {
        // Step 1: Convert to char array and remove extra spaces
        char[] ch = s.toCharArray();
        int len = removeExtraSpaces(ch);// 原地修改 ch，会直接影响原来的ch数组本身

        // Step 2: Reverse the whole valid part of the array
        reverse(ch, 0, len - 1);//后面可能跟垃圾空格，表示从数组 `ch` 的下标 `0` 开始，提取 `len` 个字符，构造字符串。

        // Step 3: Reverse each word
        int start = 0;
        for (int i = 0; i <= len; i++) {
            if (i == len || ch[i] == ' ') {
                reverse(ch, start, i - 1);
                start = i + 1;
            }
        }
        return new String(ch, 0, len);
    }

    // Removes leading, trailing, and multiple spaces. Returns new valid length.
    private int removeExtraSpaces(char[] ch) {
        int n = ch.length;
        int fast = 0, slow = 0;

        // Skip leading spaces 跳过开头的空格，指向第一个有内容的字母
        while (fast < n && ch[fast] == ' ') fast++;

        while (fast < n) {
            // Copy non-space characters
            if (ch[fast] != ' ') {
                ch[slow] = ch[fast];
                slow++;
                fast++;
            } else {
                // Only keep one space between words
                ch[slow++] = ' ';// 遇到单词间的冗余空格，先写一个空格
                while (fast < n && ch[fast] == ' ') fast++;// fast再跳过所有空格，直到指向有内容的字母
            }
        }

        // Remove trailing space 删除末尾空格
        if (slow > 0 && ch[slow - 1] == ' ') slow--;//防止全空的数组，防御性编程
        return slow; // New valid length
    }

    // In-place reverse from l to r
    private void reverse(char[] ch, int l, int r) {
        while (l < r) {
            char temp = ch[l];
            ch[l++] = ch[r];
            ch[r--] = temp;
        }
    }
}
```

## 2.5KMP算法
https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0028.%E5%AE%9E%E7%8E%B0strStr.md
### next数组
#### next数组原理 -- 我们要构造什么
对于模式串 `s[0...i]`，我们要找出：
```java
s[0...k-1] == s[i-k+1...i]
```
即：**前缀 == 后缀**，但不能等于整个字符串。

我们需要一个**算法**去“自动”找出这个 `k`
KMP 的构造过程用了一个关键策略：
>如果我们已经知道 `s[0...i-1]` 的最长相等前后缀是 `j`，那我们尝试直接扩展这个前缀，看看能不能让 `s[0...i]` 也匹配。

我们设：
- `i` 是当前考察的字符位置，尝试构造 `next[i]`
- `j` 是 `s[0...i-1]` 的最长相等前后缀的“末尾位置”

**我们希望通过 判断 `s[i] == s[j+1]` 来扩展之前已知的前后缀。**

合法性的关键：
如果 `s[i] == s[j + 1]`，说明：

- 原来已知的前缀 `"s[0...j]"` 和后缀 `"s[i-j...i-1]"` 是一样的；???
- 现在 `s[i] == s[j+1]`，意味着：
    - 可以把前缀和后缀各扩展一个字符；
    - 所以新的最长相等前后缀就是 `j + 1`

如果 `s[i] != s[j + 1]`
那就**回退** `j` 到上一次可能的匹配长度：
`j = next[j]; // 更短的前缀`


#### 构造next数组
**构造next数组是计算模式串s前缀表的过程。** 主要有如下三步：

1. 初始化
2. 处理前后缀不相同的情况
3. 处理前后缀相同的情况

**1.初始化：**
定义两个指针`i`和`j`，
**`j`指向前缀末尾位置 -- 最长匹配前缀长度为 `j + 1`，即末尾位置是 `j`**
**`i`指向后缀末尾位置 -- 当前正在处理的位置，用于构造 `next[i]`**
然后还要对next数组进行初始化赋值，如下：
`int j = -1 // 按统一减一写法`
`next[0] = j // 固定写法`
对于 `i = 0`，也就是只有一个字符的情况：
如`s = "a"`
**问：这个长度为 1 的字符串，有没有相等的前后缀？**
- 没有。
- 所以最长相等前后缀长度是 0 ——但我们定义的是 `j = -1`，所以就有：
- `next[0] = -1;`

**2.处理过程**
因为j初始化为-1，那么i就从1开始，进行s[i] 与 s[j+1]的比较。
Step 1: `i = 1`, `j = -1`
```java
s     =  a   b   a   b   c
index =  0   1   2   3   4
             ↑
             i
j = -1 → s[j+1] = s[0] = a

s[1] = b ≠ s[0] = a → 不匹配，无法回退
当 `j == -1` 时，`while` 条件不成立，不进入循环
→ 直接设置 next[1] = -1
更新后：
next = [-1, -1, ?, ?, ?]
```

Step 2: `i = 2`, `j = -1`
```java
s     =  a   b   a   b   c
index =  0   1   2   3   4
                 ↑
                 i
j = -1 → s[j+1] = s[0] = a

s[2] = a == s[0] ✅ 匹配
→ j++, j = 0
→ 设置 next[2] = j = 0
更新后：
next = [-1, -1, 0, ?, ?]
```

Step 3: `i = 3`, `j = 0`
当前比较：`s[3]` 和 `s[1]`
```java
s     =  a   b   a   b   c
index =  0   1   2   3   4
                     ↑
                     i
j = 0 → s[j+1] = s[1] = b

s[3] = b == s[1] ✅ 匹配
→ j++, j = 1
→ 设置 next[3] = j = 1
更新后：
next = [-1, -1, 0, 1, ?]
```

Step 4: `i = 4`, `j = 1`
当前比较：`s[4]` 和 `s[2]`
```java
s     =  a   b   a   b   c
index =  0   1   2   3   4
                         ↑
                         i
j = 1 → s[j+1] = s[2] = a

s[4] = c ≠ s[2] = a ❌ 不匹配
→ 回退 j = next[1] = -1

现在 j = -1，再比较 s[4] 和 s[0]

s[4] = c ≠ s[0] = a ❌ 不匹配
→ 设置 next[4] = -1
next = [-1, -1, 0, 1, -1]
```

代码部分：
```java
    public static int[] getNext(String s) {
        int[] next = new int[s.length()];
        next[0] = -1;
        int j = -1;

        for (int i = 1; i < s.length(); i++) {
            while (j >= 0 && s.charAt(i) != s.charAt(j + 1)) {
                j = next[j];
            }
            if (s.charAt(i) == s.charAt(j + 1)) {
                j++;
            }
            next[i] = j;
        }

        return next;
    }
```
口诀：
    固定 -1 开始  
    i 从 1 开始  
    不匹配就回退  
    匹配就 j++  
    把 j 存进 next[i]

### 用 KMP 在**文本串 `s` 中查找模式串 `t`
`i` 是当前在**文本串**中匹配到的位置（最后一个匹配字符的位置）
`j` 是当前在**模式串**中的匹配位置
我们用 KMP 在**文本串 `s` 中查找模式串 `t`**，如果匹配成功：
> `j` 会刚好指向模式串 `t` 的末尾，即 `j == t.length() - 1`

此时我们要返回**匹配的起始位置**，即：
```java
return i - t.length() + 1;
```

### 全部的代码
```java
public class KMP {
    /**
     * 构造 next 数组（前缀表）
     * next[i] 表示：t[0..i] 这一段中，最长相等前后缀的末尾位置（长度为 j+1）
     */
    public static int[] getNext(String t) {
        int[] next = new int[t.length()];
        next[0] = -1; // 固定写法：t[0] 没有前缀，设为 -1
        int j = -1;   // j 表示当前前缀的末尾位置

        for (int i = 1; i < t.length(); i++) {
            // 如果当前字符和前缀不匹配，回退 j
            while (j >= 0 && t.charAt(i) != t.charAt(j + 1)) {
                j = next[j];  // 不匹配就跳到上一个可能的前缀位置
            }

            // 如果匹配，扩展前缀长度
            if (t.charAt(i) == t.charAt(j + 1)) {
                j++;
            }

            // 记录当前 i 对应的最长前后缀长度（末尾下标）
            next[i] = j;
        }

        return next;
    }

    /**
     * KMP 主算法：在主串 s 中查找模式串 t 的第一次出现位置
     * 若找到，返回起始下标；否则返回 -1
     */
    public static int kmpSearch(String s, String t) {
        if (t.length() == 0) return 0;  // 空模式串默认返回 0

        int[] next = getNext(t);  // 构造前缀表
        int j = -1;  // 模式串指针

        for (int i = 0; i < s.length(); i++) {
            // 当前不匹配就回退 j
            while (j >= 0 && s.charAt(i) != t.charAt(j + 1)) {
                j = next[j];
            }

            // 匹配成功则 j++
            if (s.charAt(i) == t.charAt(j + 1)) {
                j++;
            }

            // 模式串匹配完毕，返回匹配起点
            if (j == t.length() - 1) {
                return i - t.length() + 1;
            }
        }

        return -1; // 没有匹配成功
    }

    // 测试样例
    public static void main(String[] args) {
        String s = "ababababc";   // 主串
        String t = "ababc";       // 模式串

        int pos = kmpSearch(s, t);

        System.out.println("模式串第一次出现的位置: " + pos);  // 应输出 4
    }
}

```

## 2.6 队列queue
### 概述

计算机科学中，queue 是以顺序的方式维护的一组数据集合，在一端添加数据，从另一端移除数据。习惯来说，添加的一端称为**尾**，移除的一端称为**头**，就如同生活中的排队买商品

> a **queue** is a collection of entities that are maintained in a sequence and can be modified by  the addition of entities at one end of the sequence and the removal of  entities from the other end of the sequence

| 类型               | 能否 new？                   | 举例                                 |
| ---------------- | ------------------------- | ---------------------------------- |
| 接口 (`interface`) | ❌ 不能直接 new                | `Queue`, `List`, `Map`, `Set`      |
| 类 (`class`)      | ✅ 可以直接 new（如果不是 abstract） | `Stack`, `LinkedList`, `ArrayList` |
### 基础操作
```java
public interface Queue<E> {

    boolean offer(E value);
     * offer - 向队列尾插入值
     * @param value 待插入值
     * @return 插入成功返回 true, 插入失败返回 false
    
    E poll();
     * poll - 从对列头获取值, 并移除
     * @return 如果队列非空返回对头值, 否则返回 null
     
    E peek();
     * peek - 从对列头获取值, 不移除
     * @return 如果队列非空返回对头值, 否则返回 null
     
    boolean isEmpty();
     * isEmpty - 检查队列是否为空
     * @return 空返回 true, 否则返回 false
     
    boolean isFull();
     * isFull - 检查队列是否已满
     * @return 满返回 true, 否则返回 false
     
}
```
### 常见实现类
- `LinkedList`：可以当作队列用（也实现了 `Deque`）
- `PriorityQueue`：优先队列，按优先级自动排序
- `ArrayDeque`：更高效的双端队列实现（推荐）
### Deque 接口（Queue 的子接口）
> 因为 `Queue` 是**单端队列接口**，只支持：
- `offer()`、`poll()`、`peek()`：这些都是在**队头或队尾单向操作**

> `Deque` 继承自 `Queue`，`Deque` 是双端队列接口，支持两端操作；
- `getLast()` → 访问**队尾** 
- `removeLast()` → 移除**队尾**

### 常见的deque方法列表

| 方法名             | 操作位置 | 描述                        |
| --------------- | ---- | ------------------------- |
| `add(val)`      | 队尾   | 添加元素到队尾（等同 `addLast`）     |
| `addLast(val)`  | 队尾   | 添加元素到队尾（等同`offerLast`）    |
| `addFirst(val)` | 队头   | 添加元素到队头                   |
| `poll()`        | 队头   | 弹出队头元素（等同 `pollFirst`）    |
| `pollFirst()`   | 队头   | 弹出队头元素                    |
| `pollLast()`    | 队尾   | 弹出队尾元素                    |
| `peek()`        | 队头   | 查看队头元素（不移除）               |
| `peekFirst()`   | 队头   | 查看队头元素                    |
| `peekLast()`    | 队尾   | 查看队尾元素                    |
| `getLast()`     | 队尾   | 获取队尾元素（不移除）               |
| `removeLast()`  | 队尾   | 移除队尾元素（等同 pollLast 但会抛异常） |
### leetcode 239滑动窗口最大值 (模版难记)

使用单调队列——**从队尾开始**，把所有比 `val` 小的元素都移除掉，然后将 `val` 加到队尾。  这样可以保证队列从队头到队尾是**单调递减**的（即最大值在队头）。
#### 步骤：
1. 遍历数组 `nums`，用一个 **双端队列 deque** 来维护窗口。
    - 队列里存的是 **下标 i**，不是元素值；
    - 通过 `nums[deque.peekFirst()]` 和 `nums[deque.peekLast()]` 访问队列里的对应值。
    
2. 向队列中加入一个新元素 `i`, 加入前进行清理操作：
    -加入前，判断队尾元素 `nums[deque.peekLast()]` 是否 **小于当前元素 `nums[i]`**：
    - 如果小于 → 把队尾元素移除（`pollLast()`），因为它不可能再成为当前窗口或后续窗口的最大值；
    - 如果大于等于当前元素 → 保留队尾，直接将当前元素下标 `i` 加入队尾；
        - 哪怕当前元素不是最大值，也要保留在队尾，因为 **将来可能轮到它成为窗口最大值**（比如 [1,3,1,2,0,5]）。
    
 3. 当滑动窗口移动（i >= k 时），需要检查窗口左边是否有元素滑出窗口范围：
    - 窗口左边界是 **`i - k`**；
    - 如果队头元素下标 `deque.peekFirst()` 恰好等于 `i - k`，说明它对应的元素已经滑出窗口 → 要将它移除（`pollFirst()`）；
         - 队头 → 当前窗口内的最大值，如果队头元素的下标 **已经滑出窗口**，它虽然仍然是最大的那个元素，但是它**不属于当前窗口了**，不能作为答案。**如果不移除**，你就会：错误地把「已经滑出窗口的元素」当作当前窗口最大值 → 答案就错了 ❌。
    - 如果队头元素下标不等于 `i - k`，说明它仍在窗口内 → 什么都不做；
        - ⚠️ 不是用「值是否相等」判断是否滑出，而是用 **下标是否滑出** 判断。
    
4. 当前窗口形成之后（i >= k - 1），可以直接将当前窗口最大值加入结果集：
    - 当前窗口的最大值就是队头对应的 `nums[deque.peekFirst()]`；
    - 将该值加入结果数组。
    
```java
import java.util.*;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        // 结果数组
        int n = nums.length;
        int[] res = new int[n - k + 1];

        // 双端队列，存储的是**下标**，队列中的值对应 nums[下标]，保持单调递减
        Deque<Integer> deque = new LinkedList<>();

        for (int i = 0; i < n; i++) {
            // Step 1: 移除滑出窗口元素（窗口左边界是 i - k）
            if (!deque.isEmpty() && deque.peekFirst() == i - k) {
                deque.pollFirst();
            }

            // Step 2: 保持单调性，移除小于当前元素的队尾元素
            while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
                deque.pollLast();
            }

            // Step 3: 当前元素入队
            deque.offerLast(i);

            // Step 4: 从 i >= k - 1 开始，记录窗口的最大值（队头元素）
            if (i >= k - 1) {
                res[i - k + 1] = nums[deque.peekFirst()];
            }
        }

        return res;
    }
}
```

## 2.7栈stack
### 概述
计算机科学中，**stack** 是一种线性的数据结构，只能在其一端添加数据和移除数据。习惯来说，这一端称之为**栈顶**，另一端不能操作数据的称之为**栈底**，就如同生活中的一摞书

### 基础操作
```java
public interface Stack<E> {
    boolean push(E value);
     * push - 向栈顶压入元素
     * @param value 待压入值
     * @return 压入成功返回 true, 否则返回 false
    
    E pop();
     * pop - 从栈顶弹出元素
     * @return 栈非空返回栈顶元素, 栈为空返回 null
     
    E peek();
     * peek - 返回栈顶元素, 不弹出
     * @return 栈非空返回栈顶元素, 栈为空返回 null

    boolean isEmpty();
     * isEmpty - 判断栈是否为空
     * @return 空返回 true, 否则返回 false

    boolean isFull();
     * isFull - 判断栈是否已满
     * @return 满返回 true, 否则返回 false
}
```

### 实现栈
#### 使用deque
Deque（双端队列）是一种可以兼容“栈”和“队列”操作的通用容器。

>因为 `Deque` 接口提供了双端操作能力：
- 可以**只在一端进出**，就相当于用它来实现栈（Stack）。
- 也可以**一端进、一端出**，就相当于用它来实现队列（Queue）。

```java
Deque<Integer> stack = new ArrayDeque<>(); // ✅ 推荐
Deque<Integer> stack = new LinkedList<>(); // 也可以，但不如 ArrayDeque 高效
Stack<Integer> stack = new ArrayDeque<>(); ❌ 错误，但ArrayDeque并不是 Stack 的子类；
```

虽然 `LinkedList` 同时实现了多个接口，包括 `List` 和 `Deque`，但是：

| 用法                                             | 本质类型   | 是否能 `pop()`   |
| ---------------------------------------------- | ------ | ------------- |
| `Deque<Character> stack = new LinkedList<>();` | ✅ 栈用法  | ✅ 可以 `pop()`  |
| `List<Character> list = new LinkedList<>();`   | ✅ 列表用法 | ❌ 不可以 `pop()` |

相邻字符处理请用栈
### leetcode232 implement queue using stacks
```java
class MyQueue {

    //定义两个栈

    Stack<Integer> stackIn;
    Stack<Integer> stackOut;

  

    public MyQueue() {
        stackIn = new Stack<>();
        stackOut = new Stack<>();
    }

    public void push(int x) {
        stackIn.push(x);
    }

    public int pop() {
        dumpstackIn();
        return stackOut.pop();  
    }

    public int peek() {
        dumpstackIn();
        return stackOut.peek();
    }

    public boolean empty() {
        return stackIn.isEmpty() && stackOut.isEmpty();
    }

    //如果stackOut为空，那么stackIn元素全放到stackOut里
    private void dumpstackIn() {
        if(!stackOut.isEmpty()) return;//如果出栈的stackOut非空，结束，直接用stackOut
        while (!stackIn.isEmpty()) {
            stackOut.push(stackIn.pop());
        };
    }
}
```

### leetcode 20 valid parentheses
```java
class Solution {
    public boolean isValid(String s) {
        Deque<Character> stack = new LinkedList<>();
        char[] ch = s.toCharArray();
        if (ch.length % 2 != 0) return false;
        for (char i : ch) {
            // 每次遇到一个“左括号”，就把它对应的“右括号”放入栈中
            if (i == '(') {
                stack.push(')');
            }  else if (i == '{') {
                stack.push('}');
            } else if (i == '[') {
                stack.push(']');
            } else if (stack.isEmpty() || stack.peek() != i) {//开始判断右括号
                return false;
            } else {
                stack.pop();//匹配成功移除它
            }
        }
        return stack.isEmpty();
    }
}
```

### leetcode 1047 remove all adjacent duplicates in string
```java
class Solution {
    public String removeDuplicates(String s) {
        Deque<Character> stack = new LinkedList<>();
        char[] ch = s.toCharArray();
        for (char i : ch) {
            if (stack.isEmpty() || i != stack.peek()) {
                stack.push(i);
            } else {
                stack.pop();
            }
        }

        String str = ""; //必须是双引号 且中间不能有空格
        while (!stack.isEmpty()) {
            str = stack.pop() + str; //注意加的顺序要和取的顺序对应
        }
        return str;
    }
}
```

### leetcode 150 逆波特兰表达式
RPN 的规则：

- 遇到数字，压入栈；
- 遇到运算符，从栈中弹出**两个数**，先弹出的为**右操作数**，后弹出的为**左操作数**，然后将结果压入栈中。
```java
int b = stack.pop(); // 右操作数
int a = stack.pop(); // 左操作数
int result = a / b;  // 左除右
```

```java
class Solution {
    public int evalRPN(String[] tokens) { //这里是一个字符串数组，不用再转char
          Deque<Integer> stack = new LinkedList<>();
          for (String s : tokens) {
            if ("+".equals(s)) { 
            * 注意只有equals才能比较字符串内容，== 只能比较地址
            * s.equals("+")如果s为空会报错
                stack.push(stack.pop() + stack.pop());
            } else if ("-".equals(s)) {
                stack.push(- stack.pop() + stack.pop());
            } else if ("/".equals(s)) {
                int temp1 = stack.pop();
                int temp2 = stack.pop();
                stack.push(temp2 / temp1);
            } else if("*".equals(s)) {
                stack.push(stack.pop() * stack.pop());
            } else {
                stack.push(Integer.valueOf(s));
            }
          }
          return stack.pop();
    }
}
```

## 2.8堆 heap

- **大顶堆（Max Heap）**：父节点 ≥ 子节点
- **小顶堆（Min Heap）**：父节点 ≤ 子节点
### 建堆：
1. 找到最后一个非叶子节点
2. 从后向前，对每个节点执行下潜
#### 时间复杂度o(n)
![image-20230213114024607.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20230213114024607.png)
下面看交换次数的推导：设节点高度为 3

|         | 本层节点数 | 高度  | 下潜最多交换次数（高度-1） |
| ------- | ----- | --- | -------------- |
| 4567 这层 | 4     | 1   | 0              |
| 23这层    | 2     | 2   | 1              |
| 1这层     | 1     | 3   | 2              |
每一层的交换次数为：$节点个数*此节点交换次数$，总的交换次数为
$$
\begin{aligned}
& 4 * 0 + 2 * 1 + 1 * 2 \\

& \frac{8}{2}*0 + \frac{8}{4}*1 + \frac{8}{8}*2 \\

& \frac{8}{2^1}*0 + \frac{8}{2^2}*1 + \frac{8}{2^3}*2\\

\end{aligned}
$$
在 https://www.wolframalpha.com/ 输入
```
Sum[\(40)Divide[Power[2,x],Power[2,i]]*\(40)i-1\(41)\(41),{i,1,x}]
```
推导出 
$$
2^h -h -1
$$
其中 $2^h \approx n$，$h \approx \log_2{n}$，因此有时间复杂度 $O(n)$

#### 代码
```java
public class MaxHeap {
    int[] array;
    int size;

    public MaxHeap(int capacity) {
        this.array = new int[capacity];
    }

    /**
     * 获取堆顶元素
     *
     * @return 堆顶元素
     */
    public int peek() {
        return array[0];
    }

    /**
     * 删除堆顶元素
     *
     * @return 堆顶元素
     */
    public int poll() {
        int top = array[0];
        swap(0, size - 1);
        size--;
        down(0);
        return top;
    }

    /**
     * 删除指定索引处元素
     *
     * @param index 索引
     * @return 被删除元素
     */
    public int poll(int index) {
        int deleted = array[index];
        up(Integer.MAX_VALUE, index);
        poll();
        return deleted;
    }

    /**
     * 替换堆顶元素
     *
     * @param replaced 新元素
     */
    public void replace(int replaced) {
        array[0] = replaced;
        down(0);
    }

    /**
     * 堆的尾部添加元素
     *
     * @param offered 新元素
     * @return 是否添加成功
     */
    public boolean offer(int offered) {
        if (size == array.length) {
            return false;
        }
        up(offered, size);
        size++;
        return true;
    }

    * 上浮 offered：要插入的新元素，index:该元素插入的位置
    private void up(int offered, int index) {
        int child = index;
        while (child > 0) {
            int parent = (child - 1) / 2;
            if (offered > array[parent]) {
                array[child] = array[parent];
            } else {
                break;
            }
            child = parent;
        }
        array[child] = offered;
    }

    public MaxHeap(int[] array) {
        this.array = array;
        this.size = array.length;
        heapify();
    }

    // 建堆
    private void heapify() {
        // 如何找到最后这个非叶子节点  size / 2 - 1
        for (int i = size / 2 - 1; i >= 0; i--) {
            down(i);
        }
    }

    * 将 parent 索引处的元素下潜: 与两个孩子较大者交换, 直至没孩子或孩子没它大
    private void down(int parent) {
        int left = parent * 2 + 1;
        int right = left + 1;
        int max = parent;
        if (left < size && array[left] > array[max]) {
            max = left;
        }
        if (right < size && array[right] > array[max]) {
            max = right;
        }
        if (max != parent) { // 找到了更大的孩子
            swap(max, parent);
            down(max);
        }
    }

    // 交换两个索引处的元素
    private void swap(int i, int j) {
        int t = array[i];
        array[i] = array[j];
        array[j] = t;
    }

    public static void main(String[] args) {

        int[] array = {2, 3, 1, 7, 6, 4, 5};
        MaxHeap heap = new MaxHeap(array);
        System.out.println(Arrays.toString(heap.array));

        while (heap.size > 1) {
            heap.swap(0, heap.size - 1);
            heap.size--;
            heap.down(0);
        }

        System.out.println(Arrays.toString(heap.array));
    }
}
```

### 常见操作
#### 下沉操作（heapify down / sift down）
从某个节点出发，比较其与子节点的大小（大顶堆取最大，小顶堆取最小），交换后继续向下调整。

### 优先级队列
Java 的 `PriorityQueue`，Java 的 `PriorityQueue` **已经封装好了堆的所有核心操作**，你可以把它当作一个现成的 “**带优先级的堆结构**” 来使用：
- 底层使用堆结构实现（默认是最小堆）
- 本质是一个用数组实现的**完全二叉树**
- 插入时自动进行上浮（`siftUp`），删除时进行下沉（`siftDown`）

默认小顶堆，小的元素优先出队
```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
```

自定义大顶堆
```java
PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
```
或使用 `Comparator.reverseOrder()`：
```java
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Comparator.reverseOrder());
```

像leetcode 347中这样定义顺序：
```java
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[1] - a[1]);
```
这是一个**降序排列**的比较器（大顶堆）：
- 如果 `b[1] > a[1]`，返回正数，说明 `b` 应该排在 `a` 前面（也就是 `b` 优先级更高）
- 所以：**优先级高的（频率高的）排在堆顶**

### leetcode347 topK frequent elements

方法一：大顶堆法
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        *看到统计频率时请想到map*
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        *构建大顶堆*
        PriorityQueue<Map.Entry<Integer, Integer>> pq = new PriorityQueue<>((e1, e2) -> e2.getValue() - e1.getValue());
        
        *统统放入*
        pq.addAll(map.entrySet());
  
        *大顶堆依次从队头弹出k个，就是出现频率前K高的元素*
        int[] res = new int[k];
        for (int i = 0; i < k; i++) {
            res[i] = pq.poll().getKey();
        }
        return res;
    }
}
```

方法二：小顶堆法
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }

        PriorityQueue<Map.Entry<Integer, Integer>> pq = new PriorityQueue<>((e1, e2) -> e1.getValue() - e2.getValue()); // 小顶堆依然需要指定排序方法，否则默认按key元素升序排列

        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            if (pq.size() < k) {
                pq.add(entry);
            } else if (entry.getValue() > pq.peek().getValue()) {//看的是优先级最高的元素，堆顶，即最小值
                pq.poll();// 移除优先级最高的
                pq.add(entry); // 加入位置既不是队头也不是队尾，而是按照排序规则自动堆化，确保堆顶始终是优先级最高的元素
            }
        }

        int[] res = new int[k];
        for (int i = 0; i < k; i++) { //如果想由高到低输出，就写int i = k - 1; i >= 0; i--
            res[i] = pq.poll().getKey();
        }
        return res;
    }
}
```

## ArrayList/LinkedList/ArrayDeque容器选择

| 容器           | 底层结构 | 访问元素    | 插入/删除  | 内存使用 | 适合场景举例       |
| ------------ | ---- | ------- | ------ | ---- | ------------ |
| `ArrayList`  | 数组   | 快 O(1)  | 慢 O(n) | 少    | 支持快速随机访问、改得少 |
| `LinkedList` | 双向链表 | 慢 O(n)  | 快 O(1) | 多    | 插删多：链表、队列    |
| `ArrayDeque` | 循环数组 | 不支持随机访问 | 快 O(1) | 少    | 栈/队列/双端队列    |
## 2.9 二叉树 binary tree
### 二叉树理论基础篇
#### 题目分类


题目分类大纲如下：

<img src='https://file.kamacoder.com/pics/20210219190809451.png' width=600 alt='二叉树大纲'> </img></div>

说到二叉树，大家对于二叉树其实都很熟悉了，本文呢我也不想教科书式的把二叉树的基础内容再啰嗦一遍，所以以下我讲的都是一些比较重点的内容。

相信只要耐心看完，都会有所收获。

#### 二叉树的种类

在我们解题过程中二叉树有两种主要的形式：满二叉树和完全二叉树。

##### 满二叉树

满二叉树：如果一棵二叉树只有度为0的结点和度为2的结点，并且度为0的结点在同一层上，则这棵二叉树为满二叉树。

如图所示：

<img src='https://file.kamacoder.com/pics/20200806185805576.png' width=600> </img></div>

这棵二叉树为满二叉树，也可以说深度为k，有2^k-1个节点的二叉树。


##### 完全二叉树

什么是完全二叉树？

完全二叉树的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层（h从1开始），则该层包含 1~ 2^(h-1)  个节点。

**大家要自己看完全二叉树的定义，很多同学对完全二叉树其实不是真正的懂了。**

我来举一个典型的例子如题：

<img src='https://file.kamacoder.com/pics/20200920221638903.png' width=600> </img></div>

相信不少同学最后一个二叉树是不是完全二叉树都中招了。

**之前我们刚刚讲过优先级队列其实是一个堆，堆就是一棵完全二叉树，同时保证父子节点的顺序关系。**

##### 二叉搜索树

前面介绍的树，都没有数值的，而二叉搜索树是有数值的了，**二叉搜索树是一个有序树**。


* 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
* 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
* 它的左、右子树也分别为二叉排序树

下面这两棵树都是搜索树

<img src='https://file.kamacoder.com/pics/20200806190304693.png' width=600> </img></div>


##### 平衡二叉搜索树

平衡二叉搜索树：又被称为AVL（Adelson-Velsky and Landis）树，且具有以下性质：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。

如图：

<img src='https://file.kamacoder.com/pics/20200806190511967.png' width=600> </img></div>

最后一棵 不是平衡二叉树，因为它的左右两个子树的高度差的绝对值超过了1。

**C++中map、set、multimap，multiset的底层实现都是平衡二叉搜索树**，所以map、set的增删操作时间时间复杂度是logn，注意我这里没有说unordered_map、unordered_set，unordered_map、unordered_set底层实现是哈希表。

**所以大家使用自己熟悉的编程语言写算法，一定要知道常用的容器底层都是如何实现的，最基本的就是map、set等等，否则自己写的代码，自己对其性能分析都分析不清楚！**


#### 二叉树的存储方式

**二叉树可以链式存储，也可以顺序存储。**

那么链式存储方式就用指针， 顺序存储的方式就是用数组。

顾名思义就是顺序存储的元素在内存是连续分布的，而链式存储则是通过指针把分布在各个地址的节点串联一起。

链式存储如图：

<img src='https://file.kamacoder.com/pics/2020092019554618.png' width=600> </img></div>

链式存储是大家很熟悉的一种方式，那么我们来看看如何顺序存储呢？

其实就是用数组来存储二叉树，顺序存储的方式如图：

<img src='https://file.kamacoder.com/pics/20200920200429452.png' width=600> </img></div>

用数组来存储二叉树如何遍历的呢？

**如果父节点的数组下标是 i，那么它的左孩子就是 i * 2 + 1，右孩子就是 i * 2 + 2。**

但是用链式表示的二叉树，更有利于我们理解，所以一般我们都是用链式存储二叉树。

**所以大家要了解，用数组依然可以表示二叉树。**

#### 二叉树的遍历方式

关于二叉树的遍历方式，要知道二叉树遍历的基本方式都有哪些。

一些同学用做了很多二叉树的题目了，可能知道前中后序遍历，可能知道层序遍历，但是却没有框架。

我这里把二叉树的几种遍历方式列出来，大家就可以一一串起来了。

二叉树主要有两种遍历方式：

1. 深度优先遍历：先往深走，遇到叶子节点再往回走。
2. 广度优先遍历：一层一层的去遍历。

**这两种遍历是图论中最基本的两种遍历方式**，后面在介绍图论的时候 还会介绍到。

那么从深度优先遍历和广度优先遍历进一步拓展，才有如下遍历方式：

* 深度优先遍历
  * 前序遍历（递归法，迭代法）
  * 中序遍历（递归法，迭代法）
  * 后序遍历（递归法，迭代法）
* 广度优先遍历
  * 层次遍历（迭代法）


在深度优先遍历中：有三个顺序，前中后序遍历， 有同学总分不清这三个顺序，经常搞混，我这里教大家一个技巧。

**这里前中后，其实指的就是中间节点的遍历顺序**，只要大家记住 前中后序指的就是中间节点的位置就可以了。

看如下中间节点的顺序，就可以发现，中间节点的顺序就是所谓的遍历方式

* 前序遍历：中左右
* 中序遍历：左中右
* 后序遍历：左右中

大家可以对着如下图，看看自己理解的前后中序有没有问题。

<img src='https://file.kamacoder.com/pics/20200806191109896.png' width=600> </img></div>

最后再说一说二叉树中深度优先和广度优先遍历实现方式，我们做二叉树相关题目，经常会使用递归的方式来实现深度优先遍历，也就是实现前中后序遍历，使用递归是比较方便的。

**之前我们讲栈与队列的时候，就说过栈其实就是递归的一种实现结构**，也就说前中后序遍历的逻辑其实都是可以借助栈使用递归的方式来实现的。

而广度优先遍历的实现一般使用队列来实现，这也是队列先进先出的特点所决定的，因为需要先进先出的结构，才能一层一层的来遍历二叉树。

**这里其实我们又了解了栈与队列的一个应用场景了。**

具体的实现我们后面都会讲的，这里大家先要清楚这些理论基础。

#### 二叉树的定义

刚刚我们说过了二叉树有两种存储方式顺序存储，和链式存储，顺序存储就是用数组来存，这个定义没啥可说的，我们来看看链式存储的二叉树节点的定义方式。
##### C++：

```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
```

大家会发现二叉树的定义 和链表是差不多的，相对于链表 ，二叉树的节点里多了一个指针， 有两个指针，指向左右孩子。
##### Java：

```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

##### Python：

```python
class TreeNode:
    def __init__(self, val, left = None, right = None):
        self.val = val
        self.left = left
        self.right = right
```



**每次写递归，都按照这三要素来写，可以保证大家写出正确的递归算法！**

1. **确定递归函数的参数和返回值：** 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回类型。
    
2. **确定终止条件：** 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。
    
3. **确定单层递归的逻辑：** 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。

**以下以前序遍历为例：**

1. **确定递归函数的参数和返回值**：因为要打印出前序遍历节点的数值，所以参数里需要传入vector来放节点的数值，不需要有返回值，所以递归函数返回类型就是void：
```java
void traversal(TreeNode* cur, vector<int>& vec)
```

2. **确定终止条件**：在递归的过程中，如何算是递归结束了呢，当然是当前遍历的节点是空了，那么本层递归就要结束了，所以如果当前遍历的这个节点是空，就直接return，代码如下：
```java
if (cur == NULL) return;
```

3. **确定单层递归的逻辑**：前序遍历是中左右的顺序，所以在单层递归的逻辑，是要先取中节点的数值，代码如下：
```java
vec.push_back(cur->val);    // 中
traversal(cur->left, vec);  // 左
traversal(cur->right, vec); // 右
```

#### 为什么递归遍历适合用 `ArrayList`？

- 遍历时 **只是在最后加元素**，没有中间插入或删除操作。
- 访问顺序是线性的，适合用数组存储。
- 不涉及多线程，不需要线程安全的集合。
- `ArrayList` 的空间是连续的，**缓存友好，性能更好**。
### leetcode144 preorder traversal 前序遍历
LeetCode 的二叉树数组是「层序遍历（BFS顺序）」构造的
这意味着数组按照每一层从左到右依次列出
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        preorder(root, result);
        return result;
    }

    public void preorder(TreeNode root, List<Integer> result) {
        if (root == null) {
            return;
        }
        result.add(root.val);
        preorder(root.left, result);
        preorder(root.right, result);
    }
}
```
### leetcode94 inorder traversal 中序遍历
### leetcode145 postorder traversal 后序遍历
### leetcode102 层序遍历
方法一 队列解决，先进先出
```java
class Solution {
    List<List<Integer>> resList = new ArrayList<>();
    public List<List<Integer>> levelOrder(TreeNode root) {
        checkFun(root);
        return resList;
    }

    public void checkFun(TreeNode node) {
        if (node == null)
            return;
            
        *建一个队列先放入根节点*
        Queue<TreeNode> que = new LinkedList<>();
        que.offer(node);
        
        while (!que.isEmpty()) {
        *每一层新建一个itemList，存入数值*
            List<Integer> itemList = new ArrayList<>();
            *每一层的len是上一层决定好的*
            int len = que.size();
            while (len > 0) {
                TreeNode tmpNode = que.poll();
                if (tmpNode.left != null) {
                    que.offer(tmpNode.left);
                }
                if (tmpNode.right != null) {
                    que.offer(tmpNode.right);
                }
                itemList.add(tmpNode.val);
                len--;
            }
            resList.add(itemList);
        }
    }
}
```
### leetcode107 层序遍历(反序)
方法一 直接reverse
```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> resList = new ArrayList<>();
        if (root == null) return resList;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> level = new ArrayList<>();

            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);

                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }

            resList.add(level); // 先正常加
        }

        Collections.reverse(resList); // ✅ 反转整个结果即可
        return resList;
    }
}

```

方法二 用LinkedList 每一层插到结果的头部
```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        LinkedList<List<Integer>> resList = new LinkedList<>();
        if (root == null) return resList;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> level = new ArrayList<>();

            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);

                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }

            resList.addFirst(level); // ✅ 关键：插到链表头部
        }

        return resList;
    }
}
```

### leetcode226 翻转二叉树(层次遍历)
https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0226.%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.md
```java
//DFS递归
class Solution {
   /**
     * 前后序遍历都可以
     * 中序不行，因为先左孩子交换孩子，再根交换孩子（做完后，右孩子已经变成了原来的左孩子），再右孩子交换孩子（此时其实是对原来的左孩子做交换）
     */
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        invertTree(root.left);
        invertTree(root.right);
        swapChildren(root);
        return root;
    }

    private void swapChildren(TreeNode root) {
        TreeNode tmp = root.left;
        root.left = root.right;
        root.right = tmp;
    }
}
```

```java
//BFS
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {return null;}
        ArrayDeque<TreeNode> deque = new ArrayDeque<>();
        deque.offer(root);
        while (!deque.isEmpty()) {
            int size = deque.size();
            while (size-- > 0) {
                TreeNode node = deque.poll();
                swap(node);
                if (node.left != null) deque.offer(node.left);
                if (node.right != null) deque.offer(node.right);
            }
        }
        return root;
    }

    public void swap(TreeNode root) {
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
    }
}
```

```java
TreeNode tmpNode = que.poll();
swap(tmpNode);  // 交换它的左右子树
```
你 poll 出来的是树的某一个节点的引用，比如根节点 root。
- `tmpNode` 是对树中真实节点的引用。
- 所以你 `swap(tmpNode)` 实际修改的是**这棵树本身的结构**。
- 无论这个节点是不是还在队列中，它都还在树中，内存地址并没变。

对 `tmpNode` 做 `swap()`，修改的是它的 `.left` 和 `.right` 指针：
```java
private void swap(TreeNode root) {
    TreeNode tmp = root.left;
    root.left = root.right;
    root.right = tmp;
}
```
✔️ 这一步是真正修改了树的结构。**不是修改队列的内容，而是通过引用修改树中节点的链接关系。**

| 疑问                     | 解答说明                                                |
| ---------------------- | --------------------------------------------------- |
| `poll()` 之后节点还“在树中”吗？  | ✅ 是的。你只是从队列中移除了引用，但节点仍然是树的一部分                       |
| 为什么对 `tmpNode` 修改能影响树？ | 因为 `tmpNode` 是树中真实节点的引用，对它的 `.left/.right` 改动就是原地修改 |
| 原地修改是什么意思？             | 不重新建树，直接改变原有节点的结构（如换左右子树）                           |


### 不同遍历选择不同方法

| 特性        | DFS（深度优先）          | BFS（广度优先）                              |
| --------- | ------------------ | -------------------------------------- |
| 全称        | Depth-First Search | Breadth-First Search                   |
| 中文        | 深度优先搜索             | 广度优先搜索                                 |
| 遍历顺序      | 走到底再回来             | 层序遍历是“一层层走”，**所有深度为1的节点访问完，才访问深度为2的**； |
| 使用的结构     | 栈（递归栈或手动栈）         | 队列                                     |
| 常见应用      | 树的前/中/后序遍历，DFS路径   | 层序遍历，最短路径，BFS图                         |
| 是否容易找最短路径 | ❌ 不一定              | ✅ 是                                    |

这两种思维方式是完全不同的。
凡是“走到底再回来”的遍历逻辑，都必须用栈（或者递归栈）来记录走过的路。
### 为什么递归能做DFS？

因为**递归本质上就用的是系统栈（call stack）**，你每调用一次函数，其实就相当于：
```java
stack.push(node);
```
所以你可以选择：
- 显式用 `Deque` 写栈（非递归做法）
- 或者用函数递归来走（让系统帮你维护栈）

### 为什么队列不能做DFS？

队列（Queue）是先进先出，走法是**平铺式的**，无法“走到底、记路径、再回来”。

比如你写的层序遍历：
```java
que.offer(node.left);
que.offer(node.right);
```
这会立刻把左右孩子都放进队尾，**等价于同时访问它们**，就不符合 DFS 的“深入”特点了。
### 572 subtree of another tree
```java
class Solution {
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        if (root == null) return false; // 递归终止条件
        if (compare(root, subRoot)) return true; // 如果一开始就匹配成功，不需要再递归
        return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot); //孩子节点也要递归地去 isSubtree，而不是 compare
    }

    private boolean compare(TreeNode left, TreeNode right) {
        if (left == null && right != null) {
            return false;
        }
        if (left != null && right == null) {
            return false;
        }

        if (left == null && right == null) {
            return true;
        }
        if (left.val != right.val) {
            return false;
        }
        // 比较外侧
        boolean compareOutside = compare(left.left, right.left);
        // 比较内侧
        boolean compareInside = compare(left.right, right.right);
        return compareOutside && compareInside;
    }
}
```
### 二叉树 n叉树 的深度和高度
![bs01.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/bs01.PNG)

求高度是 后序遍历（左右中）从下往上计数，叶子节点的高度返回给根节点
求深度是 前序遍历（中左右） 从上往下计数

然而法无定式 根节点的高度就是最大深度

- ✅ 后序适合求高度、路径长度
- 前序适合构造路径、先访问结构
- 中序用于排序类结构（如二叉搜索树）
### leetcode104二叉树的最大深度

方法一求根节点高度就是求最大深度
```java
class Solution {
    public int maxDepth(TreeNode root) {
        return getHeight(root);  
    }

    public int getHeight(TreeNode node) {
        if (node == null) return 0;
        int leftheight = getHeight(node.left);
        int rightheight = getHeight(node.right);
        int height = 1 + Math.max(leftheight, rightheight);
        return height;
    }
}
```

方法二 层序遍历法
```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        Deque<TreeNode> que = new LinkedList<>();
        que.offer(root);
        int depth = 0;
        while (!que.isEmpty()) {
            int size = que.size();
            depth++;
            while (size > 0) {
                TreeNode tmpnode = que.poll();
                if (tmpnode.left != null) {
                    que.offer(tmpnode.left);
                }
                if (tmpnode.right != null) {
                    que.offer(tmpnode.right);
                }
                size--;
            }
        }
        return depth;
    }
}
```

### [559. Maximum Depth of N-ary Tree](https://leetcode.com/problems/maximum-depth-of-n-ary-tree/)

层序遍历法
```java
public int maxDepth(Node root) {
    if (root == null) return 0;
    Queue<Node> queue = new LinkedList<>();
    queue.offer(root);
    int depth = 0;

    while (!queue.isEmpty()) {
        int size = queue.size();
        depth++;
        for (int i = 0; i < size; i++) {
            Node node = queue.poll();
            for (Node child : node.children) {
                queue.offer(child);
            }
        }
    }
    return depth;
}
```

方法二 递归版
```java
class Solution {
    public int maxDepth(Node root) {
        return getHeight(root);
    }

    public int getHeight(Node node) {
        if (node == null) return 0;
        int depth = 0;
        if (node.children != null) {
            for (Node child : node.children) {
                int childHeight = getHeight(child);
                depth = Math.max(depth, childHeight);
            }
        }
        return 1 + depth;
    }
}
```

### leetcode111最小深度
方法一 递归
依然是左右中 后序遍历，最小深度转化为求最小高度
中途断掉的（左空右有或右空左有）不能作为最小深度比较的有效对象

```java
class Solution {
    /**
     * 递归法，相比求MaxDepth要复杂点
     * 因为最小深度是从根节点到最近**叶子节点**的最短路径上的节点数量
     */
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftDepth = minDepth(root.left);
        int rightDepth = minDepth(root.right);
        if (root.left == null) {
            return rightDepth + 1;
        }
        if (root.right == null) {
            return leftDepth + 1;
        }
        // 左右结点都不为null
        return Math.min(leftDepth, rightDepth) + 1;
    }
}
```

```java
class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) return 0;
        Queue<TreeNode> que = new LinkedList<>();
        int depth = 0;
        que.offer(root);
        while (!que.isEmpty()) {
            int size = que.size();
            while (size > 0) {
                TreeNode node = que.poll();
                if (node.left != null) {
                    que.offer(node.left);
                }
                if (node.right != null) {
                    que.offer(node.right);
                }
                if (node.left == null && node.right == null) {
                    return depth + 1;
                }
                size--;
            }
            depth++;
        }
        return depth;
    }
}
```

### leetcode222完全二叉树的节点数量
https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0222.%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%8A%82%E7%82%B9%E4%B8%AA%E6%95%B0.md

如何去判断一个左子树或者右子树是不是满二叉树呢？
在完全二叉树中，如果递归向左遍历的深度等于递归向右遍历的深度，那说明就是满二叉树。

```java
class Solution {
    public int countNodes(TreeNode root) {
        if (root == null)
            return 0;
        TreeNode leftNode = root.left;
        TreeNode rightNode = root.right;
        int leftDepth = 0, rightDepth = 0;
        while (leftNode != null) {
            leftNode = leftNode.left;
            leftDepth++;
        }
        while (rightNode != null) {
            rightNode = rightNode.right;
            rightDepth++;
        }
        if (leftDepth == rightDepth) {
            return (1 << (leftDepth + 1)) - 1; // 递归到子树数深度，加一是常态；从根开始数深度，就不用补。
        }
        * 以上都是递归终止条件
        int leftnum = countNodes(root.left);
        int rightnum = countNodes(root.right);
        return leftnum + rightnum + 1;
    }
}
```

### leetcode 110 判断是否是平衡二叉树

📌 关键点：**判断一棵树是否平衡，不能只看“当前层”**

你得确保：

- **当前节点的左右子树高度差 <= 1 ✅**
- **同时左右子树本身也是平衡树！ ✅✅**

🎯 问题转化为一个递归结构判断：
我们需要：
```java
boolean isBalanced(node) = 
    isBalanced(node.left) && 
    isBalanced(node.right) && 
    |height(left) - height(right)| <= 1
```
如果你用“单独一层的高度差”来判断，就只能看最后一层，**之前出问题的子树就会被漏掉。**

✅ 为什么剪枝（返回 -1）才是更优的？
```java
// 假设 node.left 的某一层已经不平衡 → getHeight 返回 -1
int left = getHeight(node.left);
if (left == -1) return -1;  // ✅ 提前终止
```
不加这句就会继续去递归右子树 → 白计算  
并且你最后的 `Math.abs(left - right)` 还在拿“错误数据”作比较 → ❌ 得出错误结果
🔁 没有剪枝的错误做法可能会误判的例子：
```markdown
        1
       / \
      2   2
     /     \
    3       3
   /         \
  4           4
```

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        int outcome = getHeight(root);
        if (outcome == -1) {
            return false;
        } else {
            return true;
        }
    }

    public int getHeight(TreeNode node) {
        if (node == null)
            return 0;
        int leftheight = getHeight(node.left);
        if (leftheight == -1)
            return -1;
        int rightheight = getHeight(node.right);
        if (rightheight == -1)
             return -1;
        if (Math.abs(leftheight - rightheight) > 1) {
            return -1;
        }
        return Math.max(leftheight, rightheight) + 1;
    }
}
```

### leetcode 257二叉树的所有路径（递归和回溯）
https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0257.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%89%80%E6%9C%89%E8%B7%AF%E5%BE%84.md

#### 用栈遍历
```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        //先建一个放结果的
        List<String> result = new ArrayList<>();
        //特殊情况
        if (root == null)
            return result;
        //建栈 入栈
        Deque<Object> stack = new ArrayDeque<>();
        stack.push(root);
        stack.push(root.val + ""); //把int转成string格式
        //开始遍历 节点 和 路径必须同步
        while (!stack.isEmpty()) {
            //出栈顺序要对应相反
            String path = (String) stack.pop(); // 强转
            TreeNode node = (TreeNode) stack.pop();

            if (node.left == null && node.right == null) {
                result.add(path);
            }

            if (node.right != null) {
                stack.push(node.right);
                stack.push(path + "->" + node.right.val);
            }

            if (node.left != null) {
                stack.push(node.left);
                stack.push(path + "->" + node.left.val);
            }
        }
        return result;
    }
}
```

 ✅ 为什么这道题推荐用栈？
我们要从根节点一直往下钻到叶子 → 再退回来 → 再走另一条分支

➡️ 这正是**深度优先遍历（DFS）**的特征  
➡️ 用**栈**可以完美模拟这个“深入-撤回-换路”的过程

每次将新节点压入栈时
```java
path + "->" + node.right.val
```
- 这个表达式生成了**新的字符串副本**，原来的 `path` 没变。
- Java 的 `String` 是不可变对象（immutable），所以每次拼接都是一个新的字符串，而不是在原来 `path` 上修改。

`栈（stack）` 是一个公共的结构，左右子树的路径信息是轮流压入同一个栈中，而不是分开两条栈。**✅所有路径信息都存在一个共享的栈里，但每条路径的信息是“独立封装”的。**

示例:
```java
    1
   / \
  2   3
   \
    5
```
假设你只写：
```java
stack.push(node.right);
```
没写：
```java
stack.push(path + "->" + node.right.val);
```
那你再遍历到 node.right 时，手里只有节点，没有路径信息。  
你就无法拼出 `"1->2->5"`，只能说“这是节点 5”，但不知道它的路径是什么。出栈的时候就**只剩下节点本身**，完全不知道它是从哪条路径来的，  
所有节点出栈时就**像“散装快递”一样混作一团**了。
#### 用队列遍历
```java
public List<String> binaryTreePaths(TreeNode root) {
    List<String> result = new ArrayList<>();
    if (root == null) return result;

    Queue<Object> queue = new LinkedList<>();

    queue.offer(root);// 入队 先node再path
    queue.offer(String.valueOf(root.val)); // 初始路径

    while (!queue.isEmpty()) {
        TreeNode node = (TreeNode) queue.poll();// 出队 先node再path
        String path = (String) queue.poll();

        if (node.left == null && node.right == null) {
            result.add(path);
        }

        if (node.left != null) {
            queue.offer(node.left);
            queue.offer(path + "->" + node.left.val);
        }

        if (node.right != null) {
            queue.offer(node.right);
            queue.offer(path + "->" + node.right.val);
        }
    }

    return result;
}
```
#### 回溯法
```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<>(); //存最终的结果
        if (root == null) {
            return res;
        }
        List<Integer> paths = new ArrayList<>(); //存单条路径
        traversal(root, paths, res);
        return res;
    }

    private void traversal(TreeNode root, List<Integer> paths, List<String> result) {
    *前序遍历*
        paths.add(root.val); // 中
        if (root.left == null && root.right == null) { // 终止条件
            int size = paths.size();
            String path = "";
            for (int i = 0; i < size - 1; i++) {
                path = path + paths.get(i) + "->";
            }
            path = path + paths.get(size - 1);
            result.add(path);
            return;
        }
        if (root.left != null) { *先走左边*
            traversal(root.left, paths, result);
            paths.remove(paths.size() - 1); *回溯的过程*
        }
        if (root.right != null) { *再试试走右边* 
            traversal(root.right, paths, result);
            paths.remove(paths.size() - 1);
        }
    }
}
```

| 说明              |
| --------------- |
| 添加1，走左子树        |
| 添加2，走右子树        |
| 添加5，叶子，保存路径     |
| 回溯5，paths变[1,2] |
| 回溯2，paths变[1]   |
| 走右子树            |
| 添加3，叶子，保存路径     |
| 回溯3，paths变[1]   |
| 所有路径都走完         |

### leetcode404 左叶子节点

什么是叶子节点 —— 左右孩子都为空
### leetcode 513 找树左下角的值（回溯）
```java
class Solution {
    private int maxDepth = Integer.MIN_VALUE;
    private int result;

    public void traversal(TreeNode root, int depth) {
    *当遇到叶子节点的时候，就需要统计一下最大的深度了*
        if (root.left == null && root.right == null) {
            if (depth > maxDepth) { 
            *这里必须是> 而不是>=, 否则会被depth相等的右边叶子节点覆盖掉*
                maxDepth = depth;
                result = root.val;
            }
            return; // 表示递归traversal部分执行完，不写也可以
        }

        if (root.left != null) { *左右子树的遍历顺序决定了在同一深度时先遇到谁，而题目要求的是最左边的节点，所以必须先遍历左子树。*
            depth++;
            traversal(root.left, depth);
            depth--; // 回溯
        }

        if (root.right != null) {
            depth++;
            traversal(root.right, depth);
            depth--; // 回溯
        }
    }

    public int findBottomLeftValue(TreeNode root) {
        traversal(root, 0);
        return result;
    }
}

```

左子树的过程（只关注这一边）：
```java
        1
      /   \
     2     3
    /     / \
   4     5   6
        /
       7
```
初始调用：
`traversal(1, 0)`

第一步：
`depth = 0 调用 traversal(2, 1)`

第二步：
`depth = 1 调用 traversal(4, 2)`

第三步：

`depth = 2 发现 4 是叶子节点！更新 maxDepth = 2，result = 4 return;`

此时我们 **开始回溯**：

`回到 traversal(2, 1)，depth-- 变回 1； 再检查右子树为空 -> return；`

再回溯：

`回到 traversal(1, 0)，depth-- 变回 0； 接着去右子树`

✅ **写法一：手动递增+回溯（你现在用的）**

```java
if (root.left != null) {
    depth++;
    traversal(root.left, depth);
    depth--; // 回溯恢复
}
```

- 这里**直接修改了当前函数的局部变量 `depth`**。
- Java 传的是值（不是引用），但对局部变量来说你确实改变了 `depth` 值。
- 所以递归回来后，**必须手动恢复原值**，否则接下来传递的深度就是错的。

✅ **写法二：表达式传参，不修改变量**
```java
// 递归法
class Solution {
    private int Deep = -1;
    private int value = 0;
    
    public int findBottomLeftValue(TreeNode root) {
        value = root.val;
        findLeftValue(root,0);
        return value;
    }

    private void findLeftValue (TreeNode root,int deep) {
        if (root == null) return;
        if (root.left == null && root.right == null) {
            if (deep > Deep) {
                value = root.val;
                Deep = deep;
            }
            //这里也可以加个return
        }
        if (root.left != null) findLeftValue(root.left,deep + 1);
        if (root.right != null) findLeftValue(root.right,deep + 1);
    }
}
```
- `depth + 1` 是一个**表达式**，它的结果是新的值，并没有修改 `depth` 本身。
- 所以原来的 `depth` 变量**始终是原值**，递归回来之后也不用恢复。

因为这写法背后等价于：
```java
int nextDepth = deep + 1;
findLeftValue(root.left, nextDepth);
```

- 假设当前 `deep = 1`
- 传给下一层的是 `2`
- 下一层执行完了回到这一层，`deep` 依然是 `1`，**没有变！**

### leetcode112 pathsum（回溯）

为什么栈适合并行路径？
**栈（Deque）之所以天然适合“路径并行”处理：**
- 栈的 push 操作，不是‘自动’将此次操作处理成一个路径，而是**人为地在 push 时绑定路径信息（currSum + val）**，从而完成了对该路径状态的封装。
- 所以天然支持多个路径在栈中“平行共存”而不互相干扰

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) return false;

        Deque<TreeNode> nodeStack = new ArrayDeque<>();
        List<Integer> sumList = new ArrayDeque<>();
        nodeStack.push(root);
        sumStack.push(root.val);

        while(!nodeStack.isEmpty()) {
            TreeNode node = nodeStack.pop(); 
            int currSum = sumStack.pop(); 
            if (node.left == null && node.right == null && 
            currSum == targetSum) {
                return true;
            }
            if (node.left != null) {
                nodeStack.push(node.left); 
                sumStack.push(currSum + node.left.val);
            }
            if (node.right != null) {
                nodeStack.push(node.right);
                sumStack.push(currSum + node.right.val);
            }
        }
        return false;
    }
}
```

回溯法
```java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) return false;
        return traversal(root, targetSum - root.val);
    }

    private boolean traversal(TreeNode node, int count) {
        if (node == null) return false;
        // 终止条件
        if (node.left == null && node.right == null && count == 0) {
            return true;
        }
        if (node.left == null && node.right == null && count != 0) {
            return false;
        }
        if (node.left != null) {
            // 如果这条路有true，要继续向上return
            if (traversal(node.left, count - node.left.val)) return true;
        }

        if (node.right != null) {
            return traversal(node.right, count - node.right.val);
        }
        return false;
    }
}
```

|场景|用不用 `return`|举例|说明|
|---|---|---|---|
|遍历所有节点、打印值|❌ 不用 return|前序/中序/后序遍历|要求访问全部|
|只找“是否存在一条满足条件的路径”|✅ 必须 return|本题、二叉树路径和|找到就停止返回|
### leetcode106从中序和后序遍历序列构造二叉树

```java
class Solution {
    private Map<Integer, Integer> indexMap;  // 用于快速查找中序位置 

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        int n = inorder.length;
        if (n == 0) return null;

        indexMap = new HashMap<>();
        for (int i = 0; i < n; i++) {
            indexMap.put(inorder[i], i);  // 数值 -> 中序索引
        }

        return build(inorder, 0, n, postorder, 0, n);
    }

    private TreeNode build(int[] in, int inL, int inR, int[] post, int postL, int postR) {
        if (postL == postR) return null;

        int rootVal = post[postR - 1];
        TreeNode root = new TreeNode(rootVal);

        int mid = indexMap.get(rootVal);
        int leftSize = mid - inL;

        root.left = build(in, inL, mid, post, postL, postL + leftSize);
        root.right = build(in, mid + 1, inR, post, postL + leftSize, postR - 1);

        return root;
    }
}
```

#### hashmap
建 `HashMap` 是为了 **用 O(1)** 的时间 **快速找到某个元素在中序遍历中的下标**，避免每次都用 `for` 循环去遍历找。
如果不建hashmap,就得写
```java
// 找 rootVal 在 inorder 中的位置
for (int i = inL; i < inR; i++) {
    if (inorder[i] == rootVal) {
        index = i;
        break;
    }
}
```
这段代码是 O(n) 的操作（在最坏情况下你要从头扫到尾），**而你在每一层递归都会执行一次！**
🚨 如果你不用 HashMap：

- 每层递归找一次中序位置；
    
- 假设树是平衡的，高度是 logN，有 N 层；
    
- 每层找位置都要 O(n) → 总体就是 **O(n²)**；
如果你用 HashMap

你就能直接用：
`int index = indexMap.get(rootVal); // O(1) 查找`

这样每次查找变成 O(1)，总共调用 O(n) 次 → **总时间复杂度降为 O(n)**！

#### 递归部分
给定
```java
inorder   = [A, B, C, D, E]
postorder = [A, C, B, E, D]
```
✅ 当前函数的参数：
```java
build(in, inL, inR, post, postL, postR)
```
表示我们正在构建的子树是：
- 中序遍历：`in[inL ... inR)`
- 后序遍历：`post[postL ... postR)`

✅ 第一步：找出根节点
```java
int rootVal = post[postR - 1]; // 后序的最后一个元素是当前根
TreeNode root = new TreeNode(rootVal);
```
✅ 第二步：找出 root 在中序中的位置
```java
int mid = indexMap.get(rootVal); // D 在中序的位置是 3
```
✅ 第三步：计算左子树长度
```java
int leftSize = mid - inL; // mid=3, inL=0 → leftSize = 3
```
🔍 关键这两行解释来啦：
```java
root.left = build(in, inL, mid, post, postL, postL + leftSize);
```
- **中序：[inL, mid)** → `[0, 3)` → `[A, B, C]`：左子树的结构
- **后序：[postL, postL + leftSize)** → `[0, 3)` → `[A, C, B]`：左子树的构建顺序
```java
root.right = build(in, mid + 1, inR, post, postL + leftSize, postR - 1);
```
- **中序：[mid + 1, inR)** → `[4, 5)` → `[E]`：右子树的结构
- **后序：[postL + leftSize, postR - 1)** → `[3, 4)` → `[E]`：右子树的构建顺序（别忘了 postR - 1 是根，不能包含）

是直接返回递归结果，而是把左右子树接在当前节点上，**再整体返回 root**。

### leetcode654最大二叉树（构造二叉树一定选前序遍历）
```java
class Solution {

    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return build(nums, 0, nums.length);
    }
    //构造树 一定选前序遍历
    //递归 中 左 右
    //根节点
    //左边
    //右边
    public TreeNode build(int[] nums, int indexL, int indexR) {
        if (indexR <= indexL) return null;
        if (indexR - indexL == 1) return new TreeNode(nums[indexL]);
        int maxval = 0;
        int maxindex = 0;
        for (int i = indexL; i < indexR; i++) {
            if (nums[i] > maxval) {
                maxval = nums[i];
                maxindex = i;
            }
        }

        TreeNode root = new TreeNode(maxval);
        root.left = build(nums, indexL, maxindex);
        root.right = build(nums, maxindex + 1, indexR);
        return root;
    }
}
```

|情况|表达式|含义|应该返回|
|---|---|---|---|
|子数组为空|`indexL >= indexR` 或 `indexR - indexL <= 0`|没有元素可构造|`return null`|
|子数组只剩一个元素|`indexR - indexL == 1`|构造叶子节点|`return new TreeNode(nums[indexL])`|
|子数组有多个元素|`indexR - indexL > 1`|继续分治构造左右子树|正常递归|
``
### leetcode617合并二叉树

```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        return build(root1,root2);
    }
    
    public TreeNode build(TreeNode root1, TreeNode root2) {
        if (root1 == null) return root2;
        if (root2 == null) return root1;

        TreeNode root = new TreeNode(root1.val + root2.val);
        root.left = build(root1.left, root2.left);
        root.right = build(root1.right, root2.right);
        return root;
    }
}
```

```java
class Solution {
    // 使用栈迭代
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null) {
            return root2;
        }
        if (root2 == null) {
            return root1;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root2);
        stack.push(root1);
        while (!stack.isEmpty()) {
            TreeNode node1 = stack.pop();
            TreeNode node2 = stack.pop();
            node1.val += node2.val;
            if (node2.right != null && node1.right != null) {
                stack.push(node2.right);
                stack.push(node1.right);
            } else {
                if (node1.right == null) {
                    node1.right = node2.right;
                }
            }
            if (node2.left != null && node1.left != null) {
                stack.push(node2.left);
                stack.push(node1.left);
            } else {
                if (node1.left == null) {
                    node1.left = node2.left;
                }
            }
        }
        return root1;
    }
}
```

### leetcode700二叉搜索树
二叉搜索树的特性 左子树比根节点小，根节点比右子树小
二叉搜索树自带顺序
二叉搜索树如果按中序，那么排出的数组一定是单调递增的
空树也是二叉搜索树
二叉搜索树不能有重复元素

**如果递归函数有返回值，并且希望从递归中“拿结果”，就必须 return。否则只是递归地“走一遍树”，不需要 return。**

写成 if-else 结构可以避免这个问题，因为 else 是排他的，编译器就能“安心”地知道所有路径都有 `return`：
```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null || root.val == val) return root;
        if(root.val < val) {
            return searchBST(root.right,val);
        } else {
            return searchBST(root.left,val);
        }
    }
}
```

一种错误的遍历写法：
```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        while (root != null) {
            if (root.val < val) {
                root = root.right;
            }if (root.val > val) {
                root = root.left;
            }
            return root;
        }
        return null;
    }
}
```

### leetcode98 validate binary search tree
#### 前序遍历
dfs 额外传入两个参数，分别表示从根到当前节点路径上的最小值和最大值。当前节点的值必须在最小值和最大值之间（不能等于）。
- 根节点在负无穷和正无穷内
- 左子树在负无穷和根节点内
- 右子树在根节点和正无穷呢
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    private boolean isValidBST(TreeNode node, long left, long right) {
        if (node == null) return true;
        long x = node.val;
        return left < x && x < right && isValidBST(node.left, left, x) && isValidBST(node.right, x, right);
    }
}
```
#### 中序遍历
二叉搜索树是单调递增的 
所以你只需要在中序遍历过程中：
- 每访问一个节点，都和**前一个访问的节点（max）**比较，
- 如果发现当前节点的值 `<=` 前一个节点的值，就不是合法 BST。

```java
class Solution {
    private long min = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        boolean validL = isValidBST(root.left);
        if (!validL) return false;
        if (root.val <= min) return false;
        min = root.val;
        boolean validR = isValidBST(root.right);
        if (!validR) return false;
        return true;
    }
}
```

一种较难的遍历
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode curr = root;
        long min = Long.MIN_VALUE;

        *注意判断条件，先把左边的树都压进栈里*
        while(curr != null || !stack.isEmpty()) {
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }

            curr = stack.pop();
            if (curr.val <= min) return false;
            min = curr.val;
            curr = curr.right;
        }
        return true;
    }
}
```

### leetcode530 minimum absolute difference inBST
暴力解法
1.递归转变
```java
class Solution {
    private List<Integer> list = new ArrayList<>();

    private void traversal(TreeNode root) {
        if (root == null) return;
        traversal(root.left);
        list.add(root.val); // 中序遍历得到有序数组
        traversal(root.right);
    }


    public int getMinimumDifference(TreeNode root) {
        list.clear();
        traversal(root);
        int result = Integer.MAX_VALUE;
        for (int i = 1; i < list.size(); i++) {
            result = Math.min(result, list.get(i) - list.get(i - 1));
        }
        return result;
    }
}
```
2.栈遍历迭代
```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode curr = root;

    while (curr != null || !stack.isEmpty()) {
        // 先把左边一路压栈
        while (curr != null) {
            stack.push(curr);
            curr = curr.left;
        }
        // 到达最左下节点，开始回溯
        curr = stack.pop();
        result.add(curr.val); // 访问节点
        curr = curr.right;    // 然后访问右子树
    }

    return result;
}
```

双指针递归
```java
class Solution {
    TreeNode pre = null;
    int min = Integer.MAX_VALUE;

    public int getMinimumDifference(TreeNode root) {
        compare(root);
        return min;
    }

    private void compare(TreeNode cur) {
        if (cur == null) return;
        compare(cur.left);
        if (pre != null) {
            min = Math.min(min, cur.val - pre.val);
        }
        pre = cur;
        compare(cur.right);
    }
}

```

### leetcode501二叉搜索树中的众数

##### 暴力解法

```java
class Solution {
    private void searchBST(TreeNode node, Map<Integer, Integer> map) {
        if (node == null) return;
        map.put(node.val, map.getOrDefault(node.val, 0) + 1); // 统计频率
        searchBST(node.left, map);
        searchBST(node.right, map);
    }

    public int[] findMode(TreeNode root) {
        Map<Integer, Integer> map = new HashMap<>();
        List<Integer> resultList = new ArrayList<>();
        if (root == null) return new int[0];

        searchBST(root, map);

        // 找出最大频率
        int maxFreq = Collections.max(map.values());

        // 收集所有频率等于 maxFreq 的 key
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            if (entry.getValue() == maxFreq) {
                resultList.add(entry.getKey());
            }
        }

        // List<Integer> 转 int[]
        int[] res = new int[resultList.size()];
        for (int i = 0; i < resultList.size(); i++) {
            res[i] = resultList.get(i);
        }
        return res;
    }
}

```

#### 推荐！！双指针递归法

弄一个指针指向前一个节点，这样每次cur（当前节点）才能和pre（前一个节点）作比较。
而且初始化的时候pre = NULL，这样当pre为NULL时候，我们就知道这是比较的第一个元素。

```java
class Solution {
    ArrayList<Integer> resList;
    int maxCount;
    int count;
    TreeNode pre;

    public int[] findMode(TreeNode root) {
        resList = new ArrayList<>();
        maxCount = 0;
        count = 0;
        pre = null;
        findMode1(root);
        int[] res = new int[resList.size()];
        for (int i = 0; i < resList.size(); i++) {
            res[i] = resList.get(i);
        }
        return res;
    }

    public void findMode1(TreeNode root) {
        if (root == null) {
            return;
        }
        findMode1(root.left); //左
        // 中
        if (pre == null || root.val != pre.val) {
            count = 1;
        } else { //与前一个节点值相同
            count++;
        }
        // 更新结果以及maxCount
        if (count > maxCount) {
            resList.clear();
            resList.add(root.val);
            maxCount = count;
        } else if (count == maxCount) {
            resList.add(root.val);
        }
        pre = root;//更新上一个节点

        findMode1(root.right);
    }
}
```

双指针迭代法
```java
class Solution {
    public int[] findMode(TreeNode root) {
        List<Integer> list = find(root);
        int[] res = new int[list.size()];
        for (int i = 0; i < list.size(); i++) {
            res[i] = list.get(i);
        }
        return res;
    }

    public List<Integer> find(TreeNode root) {
        List<Integer> resList = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode curr = root;
        int maxCount = 0;
        int count = 0;
        TreeNode pre = null;

        while (curr != null || !stack.isEmpty()) {
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }

            curr = stack.pop();

            if (pre == null || curr.val != pre.val) {
                count = 1;
            } else {
                count++;
            }

            if (count > maxCount) {
                resList.clear();
                resList.add(curr.val);
                maxCount = count;
            } else if (count == maxCount) {
                resList.add(curr.val);
            }

            pre = curr; // ✅ 修复这里
            curr = curr.right;
        }

        return resList;
    }
}
```

### leetcode236寻找最近公共祖先
https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0236.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.md

首先，这道题是后序遍历

搜索一条边的写法：（235二叉搜索数）
```c
if (递归函数(root->left)) return ;
if (递归函数(root->right)) return ;
```

搜索整个树写法：（本题）
```c
left = 递归函数(root->left);  // 左
right = 递归函数(root->right); // 右
left与right的逻辑处理;         // 中 
```

**在递归函数有返回值的情况下：
如果要搜索一条边，递归函数返回值不为空的时候，立刻返回，如果搜索整个树，直接用一个变量left、right接住返回值，
如果这个left、right后序还有逻辑处理的需要，也就是后序遍历中处理中间节点的逻辑（也是回溯）。

我们给定的终止逻辑为：
```java
if (root == null || root == p || root == q) {
    return root;
}
```
- 如果当前节点 `root` 是空，说明遍历到了底部，没有找到，返回 `null`。
- 如果当前节点 `root` 正好是 `p` 或 `q`，那就返回这个节点。也就是说，**只要你遇到 `p` 或 `q`，你就立刻返回这个节点，向上传递这个“我找到了”的信息。**


那么先用left和right接住左子树和右子树的返回值，代码如下：

```java
TreeNode left = lowestCommonAncestor(root.left, p, q);
TreeNode right = lowestCommonAncestor(root.right, p, q);
```

你分别在左子树和右子树中去找 `p` 和 `q`。根据刚才说的：
- 如果在左子树中找到了 `p` 或 `q`，`left` 就不为 `null`，这个返回值一定是 `p` 或 `q`。
- 如果在右子树中找到了 `p` 或 `q`，`right` 就不为 `null`，返回值也是 `p` 或 `q`。

也就是说，这个递归保证了：**返回的非空值一定是 `p` 或 `q` 本身，或者在它的子树中找到了 `p` 或 `q` 的公共祖先。**

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) { // 递归结束条件
            return root;
        }

        // 后序遍历
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        if(left == null && right == null) { // 若未找到节点 p 或 q
            return null;
        }else if(left == null && right != null) { // 若找到一个节点
            return right;
        }else if(left != null && right == null) { // 若找到一个节点
            return left;
        }else { // 若找到两个节点
            return root;
        }
    }
}
```

### leetcode235寻找二叉搜索数最近公共祖先

在**二叉搜索树（BST）**中，最关键的性质是：
> 左子树的所有节点 < 当前节点 < 右子树的所有节点
>所以**根本不需要遍历整棵树，只要比较 `p` 和 `q` 的值与当前节点 `cur` 的值，就能判断：
- 都比当前小 ⇒ LCA 一定在左子树
- 都比当前大 ⇒ LCA 一定在右子树
- 否则，当前节点就是分叉点，就是 

递归法
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        return traversal(root, p, q);
    }

    private TreeNode traversal(TreeNode cur, TreeNode p, TreeNode q) {
        if (cur == null) return null;

        // 左
        if (cur.val > p.val && cur.val > q.val) {
            TreeNode left = traversal(cur.left, p, q);
            if (left != null) {
                return left;
            }
        }

        // 右
        if (cur.val < p.val && cur.val < q.val) {
            TreeNode right = traversal(cur.right, p, q);
            if (right != null) {
                return right;
            }
        }

        // 中
        return cur;
    }
}
```

> 这道题里没有 `if (cur == p || cur == q)` 这种判断，却可以在 `left != null` 时直接 `return left;`，为什么？

这里的 `return left` 和 `return right` 并不是为了“上传找到的信息”，而是**立即在符合判断时终止递归**，是早停机制。**“返回 left/right” 在普通树中表示“我找到信息了要传上去”，而在 BST 中其实是“我决定往某个方向走”，语义不同**。

递归的精简版
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root.val > p.val && root.val > q.val) return lowestCommonAncestor(root.left, p, q);
        if (root.val < p.val && root.val < q.val) return lowestCommonAncestor(root.right, p, q);
        return root;
    }
```

迭代法
二叉树的迭代法都很简单，因为已经知道了搜索方向
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while (true) {
            if (root.val > p.val && root.val > q.val) {
                root = root.left;
            } else if (root.val < p.val && root.val < q.val) {
                root = root.right;
            } else {
                break;
            }
        }
        return root;
    }
}
```

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while (root != null) {
            if (root.val > p.val && root.val > q.val) {
                root = root.left;
            } else if (root.val < p.val && root.val < q.val) {
                root = root.right;
            } else {
                return root; // ← 找到最近公共祖先，直接返回
            }
        }
        return null; // 理论上不会触发
    }
}
```

### leetcode701 insert into a binary search tree
#### 方法一
```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        return insert(root, val); 
    }

    public TreeNode insert(TreeNode root, int val) {
        if (root == null) {
            return new TreeNode(val);// ✅ 找到了插入点创建新节点，然后一层层地返回，挂到前一层的 `.left` 或 `.right` 上。
        }
        if (val > root.val) {
            root.right = insert(root.right, val);// ✅ 把递归回来的结果挂到右边
        }
        if (val < root.val) {
            root.left = insert(root.left, val);// ✅ 把递归回来的结果挂到左边
        }
        return root;
    }
}
```

插入值 `5` 会触发如下：
- `insert(null, 5)` → 创建新节点
- 返回给 `insert(7, 5)` → 执行 `root.left = TreeNode(5)`
- 返回给 `insert(4, 5)` → 执行 `root.right = TreeNode(7)`（含新节点）

要修改树的结构（如插入、构建），就必须接住返回值！就是因为我们把 `insert(root.right, val)` 赋值给了 `root.right`，才会有层层返回；不赋值的话，就只是向下找位置，并没有真正修改原来的树结构。

|操作类型|是否需要 `return`|示例|
|---|---|---|
|✅ 修改/构建结构|是|插入节点、构建树、修剪、删除|
|❌ 只是访问/统计|否|求深度、打印路径、查找值、统计节点数|
```java
if (val > root.val) {
    return insert(root.right, val); // ❌
}
```
你这里是**递归返回新的节点，但你没有把它挂回 `root.right` 或 `root.left`**。  
你直接 `return insert(...)`，就把之前的整个 `root` 丢弃了。

#### 方法二
刚刚说了递归函数不用返回值也可以，找到插入的节点位置，直接让其父节点指向插入节点，结束递归，也是可以的。

那么递归函数定义如下：

```c
TreeNode* parent; // 记录遍历节点的父节点
void traversal(TreeNode* cur, int val)
```

没有返回值，需要记录上一个节点（parent），遇到空节点了，就让parent左孩子或者右孩子指向新插入的节点。然后结束递归。

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) return new TreeNode(val);
        TreeNode newRoot = root;
        TreeNode pre = root;
        while (root != null) {
            pre = root;
            if (root.val > val) {
                root = root.left;
            } else if (root.val < val) {
                root = root.right;
            } 
        }
        if (pre.val > val) {
            pre.left = new TreeNode(val);
        } else {
            pre.right = new TreeNode(val);
        }

        return newRoot;
    }
}
```
### leetcode 450 delete node in a BST
#### 遍历法
```java
// 解法1(最好理解的版本)
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) {
            return null;
        }
        if (root.val == key) {
            if (root.left == null && root.right == null) {
                return null;
            } else if (root.left == null && root.right != null) {
                return root.right;
            } else if (root.left != null && root.right == null) {
                return root.left;
            } else {
                TreeNode curr = root.right;
                while (curr.left != null) {
                    curr = curr.left; // 让root的左子树移到右边最小子节点
                }
                curr.left = root.left;
                return root.right;
            }
        }

        if (root.val > key) {
            root.left = deleteNode(root.left, key);
        }
        if (root.val < key) {
            root.right = deleteNode(root.right, key);
        }
        return root;
    }
}
```

为什么return null就能删掉节点？
```java
    5
   / 
  3
 /
2
```

你要删掉节点 2：

1. `deleteNode(root, 2)`
2. 进入递归 `deleteNode(root.left, 2)`，再进入 `deleteNode(root.left.left, 2)`
3. 找到 `root.val == 2`，它是叶子节点，于是返回 `null`
4. 上一层：`root.left = deleteNode(root.left, 2)` 就变成了 `root.left = null`
5. 节点 2 从树中“断开”，被删除

#### 普通二叉树的删除方式

为什么选靠近的最右节点？
    普通二叉树可以随便替换，但：
    - **右子树的最左节点**，在**结构上通常是离当前节点最“近”的一个替代点**（逻辑上易于理解）；
    - 如果是 BST，这是“中序后继”，删除时用它替换不会破坏 BST 的有序性；
    - 即便是普通二叉树，用这种策略也**统一了删除流程**：  
    - 不管有没有顺序，处理方式一致，有助于维护逻辑清晰、递归简洁。

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null; // 空树直接返回null

        if (root.val == key) {
            if (root.right == null) {//如果右子树为空，直接返回左子树，直接删除
                return root.left; 
            }
            
            // 如果右子树不空，交换当前节点和右子树最小节点的值
            TreeNode cur = root.right;
            while (cur.left != null) {
                cur = cur.left;
            }
            int temp = root.val;
            root.val = cur.val;
            cur.val = temp; // 这样就有了两个key值
        }
        
        root.left = deleteNode(root.left, key); // 这里的root还是原本的root节点，不管它的值变成了几
        root.right = deleteNode(root.right, key);
        
        return root;
    }
}
```

**是否删除取决于 —— 只有把 return null 赋值给父节点的 .left 或 .right，才会真正把那个子节点断掉。**


### leetcode669 修剪二叉搜索树
```java
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) return null;
        if (root.val < low) {
            return trimBST(root.right, low, high);
        }

        if (root.val > high) {
            return trimBST(root.left, low, high);
        }

        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);
        return root;
    }
}
```

### leetcode108 构造二叉平衡搜索树
```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return build(nums, 0, nums.length - 1);
    }

    private TreeNode build(int[]nums, int left, int right) {
        if (left > right) return null;//终止条件
        int mid = left + (right - left) / 2;//取的是动态区间的中点
        TreeNode root = new TreeNode(nums[mid]);
        root.left = build(nums, left, mid - 1);//每次递归数组不变，变化的只是左右边界来缩小范围
        root.right = build(nums, mid + 1, right);
        return root;
    }
}
```

### leetcode538 把BST变成累加树
#### 递归法
```java
class Solution {
    int pre = 0;
    public TreeNode convertBST(TreeNode root) {
        pre = 0;
        build(root);
        return root;
    }

    public void build(TreeNode curr) {
        if (curr == null) return;
        build(curr.right);
        curr.val += pre;
        pre = curr.val;
        build(curr.left);
    }
}
```

#### 迭代法 右中左
```java
class Solution {
    private int pre = 0; // 记录前一个累加值

    public TreeNode convertBST(TreeNode root) {
        TreeNode cur = root;
        Deque<TreeNode> stack = new ArrayDeque<>();

        while (cur != null || !stack.isEmpty()) {
            // 遍历右子树
            while (cur != null) {
                stack.push(cur);
                cur = cur.right;
            }

            // 处理当前节点
            cur = stack.pop();
            cur.val += pre;
            pre = cur.val;

            // 遍历左子树
            cur = cur.left;
        }

        return root;
    }
}
```

# 三.算法
## 回溯法模版
```java
结果集
List<String> res = new ArrayList<>();
中间路径
StringBuilder path = new StringBuilder();

void backtracking(参数) {
    if (终止条件) {
        存放结果
        res.add(path.toString());
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归进入下一层
        回溯，撤销处理结果
    }
}
```
## 3.1回溯算法
### leetcode77 组合

https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0077.%E7%BB%84%E5%90%88.md

回溯法的搜索过程就是一个树型结构的遍历过程，如下图，可以看出for循环用来横向遍历，递归的过程是纵向遍历。

![image.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/20250526210720.png)

```java
for (int i = startIndex; i <= n; i++) { // 控制树的横向遍历
    path.push_back(i); // 处理节点
    backtracking(n, k, i + 1); // 递归：控制树的纵向遍历，注意下一层搜索要从i+1开始
    path.pop_back(); // 回溯，撤销处理的节点
}
```

![a18a8fd7ac77698651fb99c65aefb8f.jpg](https://cdn.jsdelivr.net/gh/hoo01/image_auto/a18a8fd7ac77698651fb99c65aefb8f.jpg)


```java
class Solution {
    List<List<Integer>> result= new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n,k,1);
        return result;
    }

    public void backtracking(int n,int k,int startIndex){
        if (path.size() == k){
            result.add(new ArrayList<>(path));
            return;
        }
        
        for (int i = startIndex; i <= n; i++){
            path.add(i);
            backtracking(n, k, i + 1);
            path.removeLast();
        }
    }
}
```

### leetcode216 combinationⅢ
Find all valid combinations of `k` numbers that sum up to `n` such that the following conditions are true:

- Only numbers `1` through `9` are used.
- Each number is used **at most once**.

Return _a list of all possible valid combinations_. The list must not contain the same combination twice, and the combinations may be returned in any order.

**Example 1:**
**Input:** k = 3, n = 7
**Output:**`[[1,2,4]]`
**Explanation:**
1 + 2 + 4 = 7
There are no other valid combinations.
```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> combinationSum3(int k, int n) {
        backtracking(k, n, 0, 1);
        return result;
    }

    private void backtracking (int k, int n, int sum, int startIndex) {
        if (path.size() == k && sum == n) {
            result.add(new ArrayList<>(path));
            return;
        }
        if (path.size() > k || sum > n) return; 
        
        for (int i = startIndex; i <= 9; i++) {
            path.add(i);
            sum += i;
            backtracking(k, n, sum, i + 1);
            path.removeLast();
            sum -= i;
        }
    }
}
```

### break return 在回溯中的作用
```java
if (path.size() > k || sum > n) return; 
```
为什么这里不能用break?
- **path.size > k** 不是跟当前 i 有单调关系的。
- **sum > n** 也不是单调可剪的（i 没排序，1~9 都能选，组合型问题 sum 的变化不是固定递增的）。
- 所以 `break` 只能退出当前 for 层，**不能剪掉递归分支**。

 **🌟 关键点归纳一句话：**

👉 只有**for 中的剪枝**且后续 i 肯定不合法（通常靠**排序 + 单调性**）时，`break` 才能剪掉整条分支。
👉 如果是**当前 path 整体状态不合法**（和 i 无关），必须用 `return` 来剪掉当前递归分支

### `.length()` 和 `.size()` 的区别总结

| 用在哪种类型  | 用哪个方法          | 说明               | 示例                                        |
| ------- | -------------- | ---------------- | ----------------------------------------- |
| **数组**  | `array.length` | 属性，不是方法。表示数组的长度。 | `int[] nums = {1,2,3};` `nums.length = 3` |
| **字符串** | `str.length()` | 方法，表示字符串的字符个数。   | `"abc".length() = 3`                      |
| **集合类** | `list.size()`  | 方法，表示集合中元素的个数。   | `list.size()`                             |

### leetcode17 电话号码的数字组合
 ✅ 这题具体怎么套模板？
1. 明确问题：输入是 `"23"`，输出是所有可能的字母组合：
- 第一个数字：2 → "abc"
- 第二个数字：3 → "def"
也就是说，每个数字代表一层，每一层的所有可能是这个数字对应的字母。

2. “树的结构”是怎样的？来看图：
```r
           ""
        /  |  \
       a   b   c       <- 2 对应的 abc
     /|\  /|\  /|\
    d e f d e f d e f   <- 3 对应的 def
```
每一层 = 一个数字，每一层的“选择”就是这个数字映射的字母。

3. `String str = numString[digits.charAt(num) - '0'];`
这个操作的含义是：**将字符数字转成真正的整数数字。**

举个例子：
- `digits.charAt(num)` 取出来的是字符，比如 `'2'` 是一个字符，不是整数。
- `'2' - '0'` 的结果是 2。因为 Java 中字符是有 ASCII 编码值的：

| 字符  | ASCII 值 |
| --- | ------- |
| '0' | 48      |
| '2' | 50      |

所以 `'2' - '0' = 50 - 48 = 2`。
也就是说：**把字符 `'2'` 变成整数 `2`**。

```java
class Solution {
    List<String> result = new ArrayList<>();
    StringBuilder temp = new StringBuilder();
    String[] numString = { "", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz" };
  
    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0) {
            return result; // 边界检查
        }
        backtracking(digits, 0);
        return result;
    }

    private void backtracking(String digits, int startIndex) {
        //startIndex表示处理的是digits字符串中的第几位数字，如'2','3'
        if (temp.length() == digits.length()) {
            result.add(temp.toString());//StringBuilder不是String
            return;
        }

        String str = numString[digits.charAt(startIndex) - '0'];
        // i 表示该数字所对应字母表中的第几个字母（比如 'a', 'b', 'c' 中的某个）
        for (int i = 0; i < str.length(); i++) {
            temp.append(str.charAt(i));
            backtracking(digits, startIndex + 1);
            temp.deleteCharAt(temp.length() - 1);
        }
    }
}
```
### leetcode39 combination sum

**Example 1:**
**Input:** candidates = `[2,3,6,7]`, target = 7
**Output:**` [[2,2,3],[7]]`
**Explanation:**
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

```java
Level 0: sum=0, path=[]        startIndex=0
└─→ i=0 (选 2)
     ↘
  Level 1: sum=2, path=[2]     startIndex=0
  ├─→ i=0 (选 2)
  │    ↘
  │  Level 2: sum=4, path=[2,2]   startIndex=0
  │  ├─→ i=0 (选 2)
  │  │    ↘
  │  │  Level 3: sum=6, path=[2,2,2]   startIndex=0
  │  │  ├─→ i=0 (选 2) → sum=8 ❌ 剪枝 → return
  │  │  ├─→ i=1 (选 3) → sum=9 ❌ 剪枝 → return
  │  │  ├─→ i=2 (选 6) → sum=12 ❌ return
  │  │  ├─→ i=3 (选 7) → sum=13 ❌ return
  │  │  ↩ return to Level 2，恢复 path=[2,2]
  │
  │  ├─→ i=1 (选 3)
  │  │    ↘
  │  │  Level 3: sum=7, path=[2,2,3] ✅ 满足 → add result
  │  │  ↩ return to Level 2，恢复 path=[2,2]
  │
  │  ├─→ i=2 (选 6) → sum=10 ❌
  │  ├─→ i=3 (选 7) → sum=11 ❌
  │  ↩ return to Level 1，恢复 path=[2]
  │
  ├─→ i=1 (选 3)
  │    ↘
  │  Level 2: sum=5, path=[2,3]
  │  ├─→ i=1 (选 3) → sum=8 ❌
  │  ├─→ i=2 (选 6) → sum=11 ❌
  │  ├─→ i=3 (选 7) → sum=12 ❌
  │  ↩ return to Level 1，恢复 path=[2]
  │
  ├─→ i=2 (选 6) → sum=8 ❌
  ├─→ i=3 (选 7) → sum=9 ❌
  ↩ return to Level 0，恢复 path=[]

└─→ i=1 (选 3)
     ↘
  Level 1: sum=3, path=[3]     startIndex=1
  ├─→ i=1 (选 3)
  │    ↘
  │  Level 2: sum=6, path=[3,3]
  │  ├─→ i=1 (选 3) → sum=9 ❌
  │  ├─→ i=2 (选 6) → sum=12 ❌
  │  ├─→ i=3 (选 7) → sum=13 ❌
  │  ↩ return to Level 1，恢复 path=[3]
  │
  ├─→ i=2 (选 6) → sum=9 ❌
  ├─→ i=3 (选 7) → sum=10 ❌
  ↩ return to Level 0，恢复 path=[]

└─→ i=2 (选 6)
     ↘

  Level 1: sum=6, path=[6]     startIndex=2
  ├─→ i=2 (选 6) → sum=12 ❌
  ├─→ i=3 (选 7) → sum=13 ❌
  ↩ return to Level 0，恢复 path=[]

└─→ i=3 (选 7)
     ↘
  Level 1: sum=7, path=[7] ✅ 满足 → add result
  ↩ return to Level 0，恢复 path=[]
```

```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates); //先排序就可以剪枝
        backtracking(candidates, target, 0, 0);
        return result;
    }
  
    private void backtracking(int[] candidates, int target, int sum, int startIndex) {
        if (sum > target) {
            return;
        }
        if (sum == target) {
            result.add(new ArrayList<>(path));
            return;
        }
        for (int i = startIndex; i < candidates.length; i++) {
            if (sum + candidates[i] > target) break; // 一定要先判断再剪枝，不然就在判断加了两遍的candidates[i]，会导致合法路径被剪
            path.add(candidates[i]);
            // 直接 sum + candidates[i] 传下去，避免手动加减
            backtracking(candidates, target, sum + candidates[i], i);
            path.removeLast();
        }
    }
}
```

#### backtracking(sum + candidates[i])
1️⃣ 函数调用时「会压栈」：

每调用一次 `backtracking(...)`，Java 会把当前函数调用的**局部变量状态（包括 sum 的值）保存到栈帧里**。等递归返回的时候，会**恢复到上一次的状态**，所以「上一层 sum 的值**自然是递归前的 sum**」，不用你手动改。

```java
sum += candidates[i];
backtracking(...);
sum -= candidates[i];
```
这里 sum 是全局变量或者局部变量改动：sum += x 改了 sum。回来以后你必须 sum -= x 手动改回来，否则 sum 会错。容易出错，代码更复杂。

为什么 sum + candidates[i] 这种写法更优雅？
```java
backtracking(..., sum + candidates[i], i);
```
sum 是「值传递」，每一层递归收到的 sum 是「新计算出来的 sum」，不是外层 sum。
**所以不需要手动 sum -= x。栈帧自动帮你维护各层 sum 的值。**
### leetcode40 combination sum2
Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.
Each number in `candidates` may only be used **once** in the combination.
**Note:** The solution set must not contain duplicate combinations.

**Example 1:**
**Input:** candidates = [10,1,2,7,6,1,5], target = 8
**Output:** 
```java
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

**Example 2:**
**Input:** candidates = [2,5,2,1,2], target = 5
**Output:** 
```java
[
[1,2,2],
[5]
]
```

**Constraints:**
- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

回看一下题目，元素在同一个组合内是可以重复的，怎么重复都没事，但两个组合不能相同。
**所以我们要去重的是同一树层上的“使用过”，同一树枝上的都是一个组合里的元素，不用去重**。
为了理解去重我们来举一个例子，candidates = [1, 1, 2], target = 3，（方便起见candidates已经排序了）
**强调一下，树层去重的话，需要对数组排序！**

![image.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/20250528161546.png)

```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates); // 排序，排序好了就能判断去重
        backtracking(candidates, target, 0, 0);
        return result;
    }

    private void backtracking(int[] candidates, int target, int sum, int startIndex) {
        if (sum == target) {
            result.add(new ArrayList<>(path));
            return;
        }

        if (sum > target) {
            return;
        }

        for (int i = startIndex; i < candidates.length; i++) {
            if (i > startIndex && candidates[i] == candidates[i - 1]) {
                continue; // 只在“同一层”中跳过重复元素，不影响不同路径中选取相同的数！
            }
            if (sum + candidates[i] > target) {
                break; // break 当前纵向树枝，横向树层依然可以移动到 i++
            }
            sum += candidates[i];
            path.add(candidates[i]);
            backtracking(candidates, target, sum, i + 1);
            sum -= candidates[i];
            path.removeLast();
        }
    }
}
```


在普通for循环里，break可以终止整个for循环。这道题看似break后，还可以继续移动横向树层是因为：
这里的break:
- 假设当前递归层是 Level 2，正在 for 循环 i = 2...4。
- `break` 是在 `Level 2` 的 for 里触发的 ⇒ 只会终止这一个 for。
- 然后 `Level 2` 函数 return 了，回到了 `Level 1`
- 此时 `Level 1` 继续它的 `for (int i = startIndex; i < ...)`，所以你看到的“i++”是在 **上一层继续走横向**，**不是当前这层没被 break**。

### leetcode 494 target sum
You are given an integer array `nums` and an integer `target`.

You want to build an **expression** out of nums by adding one of the symbols `'+'` and `'-'` before each integer in nums and then concatenate all the integers.

- For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'` before `1` and concatenate them to build the expression `"+2-1"`.

Return the number of different **expressions** that you can build, which evaluates to `target`.

**Example 1:**

**Input:** nums = [1,1,1,1,1], target = 3
**Output:** 5
**Explanation:** There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3

```java
           sum = 0
            /    \
        +nums[0]  -nums[0]
         /   \      /    \
   +nums[1] -nums[1] +nums[1] -nums[1]
       ...         ...
```
- **39 题 Combination Sum 需要 for 循环**，因为要选多个 candidates 中的某些数，控制重复。
- **494 题 Target Sum 是固定每一个 nums[i] 前面加 + 或 -**，所以一层只处理一个元素，**不需要 for 循环**，直接递归处理当前 index。

如果用 `for`，相当于把 `nums[i] `之后的所有元素都遍历了一遍，但是 **应该只处理当前 index 的 + 和 - 两种分支** → 这就导致逻辑错了。

494 题的核心是：
```java
每一个 nums[i] 有两种选择：
  1️⃣ sum + nums[i]
  2️⃣ sum - nums[i]
```

```java
class Solution {
    int count = 0;

    public int findTargetSumWays(int[] nums, int target) {
        backtracking(nums, target, 0, 0);
        return count;
    }

    private void backtracking(int[] nums, int target, int index, int sum) {
        if (index == nums.length) {
            if (sum == target) {
                count++;
            }
            return;
        }

        // 加法分支
        backtracking(nums, target, index + 1, sum + nums[index]);

        // 减法分支
        backtracking(nums, target, index + 1, sum - nums[index]);
    }
}
```

### leetcode131 回文子串切割
#### 1.字符串能不能用LinkedList
对于 `LinkedList<String>` 类型：
```java
LinkedList<String> path = new LinkedList<>();
path.add("a");
path.removeLast(); // ✅ 正确，LinkedList 有 removeLast()
```
- `LinkedList` 是一个双向链表，确实提供了 `removeLast()` 方法，所以这个调用是合法的。
- 适用于你做回溯时希望快速地在尾部加/删元素的场景。

但是对 **`String` 类型** 本身（不是 List）：
```java
String s = "abc";
s.removeLast(); // ❌ 错误，String 是不可变对象，没有这个方法
```
#### 2.拼接字符串
string是不可变结构，`"a" + "b"` 会生成一个新的字符串，不会改变原来的字符串。
```java
String a = "a";
String b = a + "b";  // ✅ 虽然看起来拼接了，其实是创建了一个新字符串 "ab"，原来的 "a" 没变
```
如果在回溯中不断拼接 `char`，最终得到的是一个完整的新字符串（比如 "aab"），你可能会判断这个“完整的新字符串”是不是回文， ❗**而你没有判断它的“每一段”是不是回文。**

|问题|说明|
|---|---|
|❌ 没有“切割”的动作|拼接不会形成子串列表（如 ["a", "a", "b"]）|
|❌ 无法逐段判断回文|只能判断最终结果是不是回文|
|❌ 失去递归分支结构|回溯应该是“每一步选择一个片段”，而不是“一直往下拼”|
#### 3.切分逻辑

![image.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/20250528205323.png)

`String substr = s.substring(startIndex, i + 1);`
每一根切分线从左往右画，其实就是我们代码中 `i` 的变化！
在回溯中，我们的分割策略是：

> **每一层从某个“起点 `startIndex`”开始，尝试向右切出所有可能的子串：**

也就是说
```java
for (int i = startIndex; i < s.length(); i++) {
    String substr = s.substring(startIndex, i + 1);
}
```
等价于：

> 从位置 `startIndex` 切到 `i`（包含）

**横向遍历 = `i` 从 `startIndex` 向右走 → 横向尝试不同的切法**
        我站在某个 `startIndex` 上，往右边挨个试终点 i
        
**纵向递归 = `startIndex` 变成 `i + 1` → 进入下一段切法起点**
         从当前切掉的子串后的位置，作为下一次递归的起点

>**所以 `startIndex` 是当前层「切片的起点」，而 `i` 是切到的「终点」**

```java
class Solution {
    List<List<String>> result = new ArrayList<>();
    LinkedList<String> path = new LinkedList<>();
    public List<List<String>> partition(String s) {
        backtracking(s, 0);
        return result;
    }

    private void backtracking(String s, int startIndex) {
        if (startIndex == s.length()) {
            result.add(new ArrayList<>(path));
            return;
        }

        for (int i = startIndex; i < s.length(); i++) {
            String substr = s.substring(startIndex, i + 1);
            if (isValid(substr)) {
                path.add(substr);
                backtracking(s, i + 1);
                path.removeLast();
            } else continue;
        }
    }

    private boolean isValid(String s) {
        char[] sChar = s.toCharArray();
        for (int i = 0; i < sChar.length/2; i++) {
            if(sChar[i] != sChar[sChar.length - i - 1]) return false;
        }
        return true;
    }
}
```

```java
Level 0: startIndex=0, path=[]
│
├─→ i=0, substr="a" ✅ 是回文 → path=["a"]
│    ↓ backtrack(startIndex=1) → 进入 Level 1
│  Level 1: startIndex=1, path=["a"]
│  ├─→ i=1, substr="a" ✅ 是回文 → path=["a","a"]
│  │    ↓ backtrack(startIndex=2) → 进入 Level 2
│  │  Level 2: startIndex=2, path=["a","a"]
│  │  ├─→ i=2, substr="b" ✅ 是回文 → path=["a","a","b"]
│  │  │    ↓ backtrack(startIndex=3) → 进入 Level 3 ✅ 满足结束条件（startIndex == s.length）
│  │  │    ↩ 回溯到 Level 2，startIndex=2：remove "b" → path=["a","a"]
│  │  ↩ 回溯到 Level 1，startIndex=1：remove "a" → path=["a"]
│  ├─→ i=2, substr="ab" ❌ 不是回文 → 剪枝（注意：startIndex 仍然是 1）
│  ↩ 回溯到 Level 0，startIndex=0：remove "a" → path=[]
│
├─→ i=1, startIndex=0, substr="aa" ✅ 是回文 → path=["aa"]
│    ↓ backtrack(startIndex=2) → 进入 Level 1（新分支）
│  Level 1: startIndex=2, path=["aa"]
│  ├─→ i=2, substr="b" ✅ 是回文 → path=["aa", "b"]
│  │    ↓ backtrack(startIndex=3) → 进入 Level 2 ✅ 满足结束条件
│  │    ↩ 回溯到 Level 1，startIndex=2：remove "b" → path=["aa"]
│  ↩ 回溯到 Level 0，startIndex=0：remove "aa" → path=[]
│
├─→ i=2, substr="aab" ❌ 不是回文 → 剪枝（注意：startIndex 仍然是 0）

```

### leetcode93 复原ip地址

```java
class Solution {
    List<String> result = new ArrayList<>();
    //以下区间均为左闭右闭
    public List<String> restoreIpAddresses(String s) {
        StringBuilder sb = new StringBuilder(s);
        backtracking(sb,0,0);
        return result;
    }

    private void backtracking(StringBuilder s, int startIndex, int pointSum) {
        if (pointSum == 3) {
            if(isValid(s,startIndex,s.length() - 1)) {//判断第三个点之后的情况
                result.add(s.toString());
            }
            return;
            //必须放在这里才能打断最后一段非法的情况；不管合法还是非法，必须打完3个点就终止
        }

        for (int i = startIndex; i < s.length(); i++) {
            if (isValid(s, startIndex, i)) {
                s.insert(i + 1, ".");
                pointSum += 1;
                backtracking(s, i + 2, pointSum);//"."也占位置
                s.deleteCharAt(i + 1);
                pointSum -= 1;
            } else {
                break; // 打断for循环，终止横向展开，不影响纵向
                // 如果当前的[startIndex,i]不合法；那么更长的[startIndex,i+1]更不合法
            }
        }
    }

    private boolean isValid(StringBuilder s, int start, int end) {
        if (start > end)
            return false;
        if (s.charAt(start) == '0' && start != end)
            return false;
        int num = 0;
        for (int i = start; i <= end; i++) {
            int digit = s.charAt(i) - '0';
            num = num * 10 + digit;
            if (num > 255) {
                return false;
            }
        }
        return true;
    }
}
```
### leetcode90 subsets子集
```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        backtracking(nums, 0);
        return result;
    }
  
    private void backtracking(int[] nums, int startIndex) {
        result.add(new ArrayList<>(path));
        for (int i = startIndex; i < nums.length; i++) {
            if (i > startIndex && nums[i] == nums[i - 1]) continue;
            path.add(nums[i]);
            backtracking(nums, i + 1);
            path.removeLast();
        }
    }
}
```

### leetcode [491. Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/)

子集类问题都可以不写终止条件
![image.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/20250530110139.png)

```java
Level 0: startIndex=0, path=[]
│
├─→ i=0, nums[0]=4 ✅ 满足起点条件 → path=[4]
│    ↓ backtrack(startIndex=1)
│  Level 1: startIndex=1, path=[4]
│  ├─→ i=1, nums[1]=7 ✅ ≥4 → path=[4,7]
│  │    ↓ backtrack(startIndex=2)
│  │  Level 2: startIndex=2, path=[4,7]
│  │  ├─→ i=2, nums[2]=6 ❌ <7 → 剪枝
│  │  ├─→ i=3, nums[3]=7 ✅ ≥7 → path=[4,7,7]
│  │  │    ↓ backtrack(startIndex=4) ✅ path 长度≥2 → 加入结果
│  │  │    ↩ 回溯：移除 7 → path=[4,7]
│  │  ↩ 回溯：移除 7 → path=[4]
│  ├─→ i=2, nums[2]=6 ✅ ≥4 → path=[4,6]
│  │    ↓ backtrack(startIndex=3)
│  │  Level 2: startIndex=3, path=[4,6]
│  │  ├─→ i=3, nums[3]=7 ✅ ≥6 → path=[4,6,7]
│  │  │    ↓ backtrack(startIndex=4) ✅ 满足 → 加入结果
│  │  │    ↩ 回溯：移除 7 → path=[4,6]
│  │  ↩ 回溯：移除 6 → path=[4]
│  ├─→ i=3, nums[3]=7 ✅ 但本层已用过 7 → ❌ 横向剪枝
│  ↩ 回溯：移除 4 → path=[]
│
├─→ i=1, nums[1]=7 ✅ → path=[7]
│    ↓ backtrack(startIndex=2)
│  Level 1: startIndex=2, path=[7]
│  ├─→ i=2, nums[2]=6 ❌ <7 → 剪枝
│  ├─→ i=3, nums[3]=7 ✅ ≥7 → path=[7,7]
│  │    ↓ backtrack(startIndex=4) ✅ 满足 → 加入结果
│  │    ↩ 回溯：移除 7 → path=[7]
│  ↩ 回溯：移除 7 → path=[]
│
├─→ i=2, nums[2]=6 ✅ → path=[6]
│    ↓ backtrack(startIndex=3)
│  Level 1: startIndex=3, path=[6]
│  ├─→ i=3, nums[3]=7 ✅ ≥6 → path=[6,7]
│  │    ↓ backtrack(startIndex=4) ✅ 满足 → 加入结果
│  │    ↩ 回溯：移除 7 → path=[6]
│  ↩ 回溯：移除 6 → path=[]
│
├─→ i=3, nums[3]=7 ✅ → path=[7]
│    ↓ backtrack(startIndex=4)
│  Level 1: startIndex=4, path=[7]
│  ↩ 但 [7] 不满足长度 ≥2，结束 → 回溯：移除 7 → path=[]
```

```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> findSubsequences(int[] nums) {
        backtracking(nums, 0);
        return result;
    }

    private void backtracking(int[] nums, int startIndex) {
        if (path.size() >= 2) {
            result.add(new ArrayList<>(path));
        }
        HashSet<Integer> hs = new HashSet<>(); //每进新的一层纵向，新建一个hashset，因此纵向不去重；横向for增加共用一个hashset,对横向的数字去重
        for (int i = startIndex; i < nums.length; i++) {
            if (!path.isEmpty() && path.getLast() > nums[i] || hs.contains(nums[i]))
                continue;
            hs.add(nums[i]);
            path.add(nums[i]);
            backtracking(nums, i + 1);
            path.removeLast();
        }
    }
}
```

### 排列题、组合题和子集：
- ✅ 排列：P = Permutation = **Position matters**
- ✅ 组合：C = Combination = **Choose without order**
- ✅ 子集：S = Subset = **Subset of all elements (include empty)**
#### 排列题和组合题的不同
排列题（Permutations），一旦 `return`，就一路 `removeLast()` 回到 i=0；不像组合题，通常只回溯到 i=1。
#### 组合题
比如：**77 题 Combinations**，**216 题 Combination Sum III**
```java
for (int i = startIndex; i < n; i++) {
    path.add(i);
    backtrack(i + 1);
    path.removeLast();
}
```
- `i = startIndex` 是为了保证横向树层选的数字 **是从上一个选的数字往后选，保证组合不会重复、不会倒退**。
- 纵向树枝只枚举「**从当前位置开始往后的数**」，避免重复，所以递归时要传入 `i + 1`，保证下一个 for 循环从更大的 index 开始。
- 所以递归结束后，只需要 `removeLast()` 一次（回退到上一个选择）

#### **排列题（Permutations）**
比如：**46 题 Permutations**。
```java
for (int i = 0; i < nums.length; i++) {
    if (used[i]) continue;
    path.add(nums[i]);
    used[i] = true;
    backtrack();
    used[i] = false;
    path.removeLast();
}
```
- 排列问题是「顺序有关」 → 可以反复选所有位置 → 每一层递归仍然可以选任意未使用元素。
- 当前元素内部是完全展开的 → i=0 遍历全部可能性。
- 所以通常用 `boolean[] used` 标记哪些元素用过，而不是靠 `startIndex + 1`。
- 每一层 `for` 枚举的是「所有未被使用的数」
- 所以 **一条路径走到底（path 长度满）→ return**
- 然后会一路往回走，回到上层，再继续 `for` 中的 `i++`
- 直到把所有分支都走完，也就等于把 `path.removeLast()` 回溯到最顶层 `[]`
### startIndex i  选择问题

| 问题            | 关键判断             | 应该传什么？                                           |
| ------------- | ---------------- | ------------------------------------------------ |
| ❓ 能否重复选当前元素？  | ✅ 可以 → 多次选择同一个数字 | backtracking 传 `i`（保留当前）                         |
| ❌ 不可重复？       | 下一次必须往后选         | backtracking 传 `i + 1`（跳到下一个）                    |
| ❓ 需要排列出不同顺序吗？ | ✅ 要顺序，如全排列       | `for` 循环传入i = 0，不传 `startIndex`，用 `visited[]` 控制 |

### leetcode 46 permutations全排列

![image.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/20250530162405.png)

这道题的关键是
**回溯递归的每一层 `for (int i = 0; i < n; i++)` 都是一个新的循环环境，和上层的 `i` 没有关系！**
```java
├─→ i=0, nums[0]=1 → used[0]=true → path=[1]
│    ↓ backtracking()
│  Level 1: path=[1]
│  ├─→ i=0, nums[0]=1 → used[0]=true → ❌ 跳过
│  ├─→ i=1, nums[1]=2 → used[1]=false → path=[1,2]
│  │    ↓ backtracking()
│  │  Level 2: path=[1,2]
│  │  ├─→ i=0, used[0]=true → ❌ 跳过
│  │  ├─→ i=1, used[1]=true → ❌ 跳过
│  │  ├─→ i=2, nums[2]=3 → used[2]=false → path=[1,2,3] ✅
│  │  │    ↓ 加入结果 → result=[[1,2,3]]
│  │  │    ↩ 回溯：used[2]=false, removeLast() → path=[1,2]
│  │  ↩ 回溯：used[1]=false, removeLast() → path=[1]
│  ├─→ i=2, nums[2]=3 → used[2]=false → path=[1,3]
│  │    ↓ backtracking()
│  │  Level 2: path=[1,3]
│  │  ├─→ i=0, used[0]=true → ❌ 跳过
│  │  ├─→ i=1, nums[1]=2 → used[1]=false → path=[1,3,2] ✅
│  │  │    ↓ 加入结果 → result=[[1,2,3],[1,3,2]]
│  │  │    ↩ 回溯：used[1]=false, removeLast() → path=[1,3]
│  │  ├─→ i=2, used[2]=true → ❌ 跳过
│  │  ↩ 回溯：used[2]=false, removeLast() → path=[1]
│  ↩ 回溯：used[0]=false, removeLast() → path=[]
```

```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    boolean[] used;
    public List<List<Integer>> permute(int[] nums) {
        used = new boolean[nums.length];
        backtracking(nums);
        return result;
    }

    private void backtracking(int[] nums) {
        if (path.size() == nums.length) {
            result.add(new ArrayList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i] == true) continue;
            used[i] = true;
            path.add(nums[i]);
            backtracking(nums);
            used[i] = false;
            path.removeLast();
        }
    }
}
```

### leetcode47 全排列Ⅱ
Given a collection of numbers, `nums`, that might contain duplicates, return _all possible unique permutations **in any order**._

**Example 1:**

**Input:** nums = [1,1,2]
**Output:**
[[1,1,2],
 [1,2,1],
 [2,1,1]]

**Example 2:**

**Input:** nums = [1,2,3]
**Output:** [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

**Constraints:**

- `1 <= nums.length <= 8`
- `-10 <= nums[i] <= 10`

```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    boolean[] used;

    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums); //排序
        used = new boolean[nums.length];
        backtracking(nums);
        return result;
    }

    private void backtracking(int[] nums) {
        if (path.size() == nums.length) {
            result.add(new ArrayList<>(path));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (used[i] == true) continue;
            if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
                continue;
            }

            used[i] = true;
            path.add(nums[i]);
            backtracking(nums);
            path.removeLast();
            used[i] = false;
        }
    }
}
```
![image.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/20250530205205.png)

去重最为关键的代码为：

```c
if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
    continue;
}
```

**如果改成 `used[i - 1] == true`， 也是正确的!**，去重代码如下：

```c
if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == true) {
    continue;
}
```

这是为什么呢，就是上面我刚说的，如果要对树层中前一位去重，就用`used[i - 1] == false`，如果要对树枝前一位去重用`used[i - 1] == true`。

**对于排列问题，树层上去重和树枝上去重，都是可以的，但是树层上去重效率更高！**

这么说是不是有点抽象？

来来来，我就用输入: [1,1,1] 来举一个例子。

树层上去重(used[i - 1] == false)，的树形结构如下：
![image.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/20250530205429.png)

树枝上去重（used[i - 1] == true）的树型结构如下：
![image.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/20250530205639.png)
大家应该很清晰的看到，树层上对前一位去重非常彻底，效率很高，树枝上对前一位去重虽然最后可以得到答案，但是做了很多无用搜索。

这里可能大家又有疑惑，既然 `used[i - 1] == false`也行而`used[i - 1] == true`也行，那为什么还要写这个条件呢？

直接这样写 不就完事了？

```c
if (i > 0 && nums[i] == nums[i - 1]) {
    continue;
}
```

其实并不行，一定要加上 `used[i - 1] == false`或者`used[i - 1] == true`，因为 used[i - 1] 要一直是 true 或者一直是false 才可以，而不是 一会是true 一会又是false。 所以这个条件要写上。

是不是豁然开朗了！！

### 332 机票使用
会超时的做法

```java
class Solution {
    private LinkedList<String> res;
    private LinkedList<String> path = new LinkedList<>();

    public List<String> findItinerary(List<List<String>> tickets) {
        Collections.sort(tickets, (a, b) -> a.get(1).compareTo(b.get(1)));
        path.add("JFK");//起始地
        boolean[] used = new boolean[tickets.size()];
        backTracking((ArrayList) tickets, used);
        return res;
    }

    public boolean backTracking(ArrayList<List<String>> tickets, boolean[] used) {
        if (path.size() == tickets.size() + 1) {
            res = new LinkedList(path);
            return true;
        }

        for (int i = 0; i < tickets.size(); i++) {
            if (!used[i] && tickets.get(i).get(0).equals(path.getLast())) {
                path.add(tickets.get(i).get(1));
                used[i] = true;

                if (backTracking(tickets, used)) {
                    return true; //这一层收到前面的return true,再返给上一层
                }

                used[i] = false;
                path.removeLast();
            }
        }
        return false;
    }
}
```
#### 1. `Collections.sort(tickets, (a, b) -> a.get(1).compareTo(b.get(1)));`
我们要从某个机场（比如 `"JFK"`）出发，枚举所有可达目的地时要**优先走字典序最小的那条航线**，所以：
我们要把所有航班**按目的地升序排好**，这样回溯时就能一层层选出字典序最小的解。

举例
```java
原始 tickets:
[["JFK", "KUL"], ["JFK", "NRT"], ["NRT", "JFK"]]

按目的地排序后:
tickets = [
  ["NRT", "JFK"],  // i = 0
  ["JFK", "KUL"],  // i = 1
  ["JFK", "NRT"]   // i = 2
]
```

#### 2.`boolean[] used = new boolean[tickets.size()];`
构建一个布尔数组，长度为机票数量，用来标记每一张票是否已经使用过。

因为：
- 每张票只能使用一次；
- 有可能有重复的票（例如两张 `"JFK" -> "SFO"`），不能简单靠目的地判断；
- 所以必须**精确追踪每张票是否被用过**，即便内容相同的两张票也视为不同的。

示例：
`[["JFK", "ATL"], ["JFK", "ATL"], ["ATL", "SFO"]]`
这两张 `"JFK" -> "ATL"` 看起来一样，但实际上是两张票。
因此用 `used[0]` 和 `used[1]` 来分别标记它们是否用过。

#### 3.`res = new LinkedList<>(path); // 直接赋值，而不是 add
**这道题只需要找到“一个合法解”**，而不是“收集所有解”！

---
 ✅ 情况一：像排列、组合、子集问题，需要**收集所有结果**

我们通常写成：

`result.add(new ArrayList<>(path));`

代表：

- 每次找到一组合法的 path 就**加入到结果集合**中；
    
- 回溯过程会不断尝试所有分支；
    
- 所以你最后拿到的是一个 `List<List<String>>`，比如所有子集、所有排列等。

 ✅ 情况二：这道机票题，只需要**返回唯一一个解**

根据题目要求：

> 找出一条用完所有票的合法路径，若有多条则返回**字典序最小的那一条**

因此：
- 我们不需要保存多个解；
- 一旦找到第一个合法解（由于我们已经排序+回溯字典序最小优先），它就是最优解；
#### 4. return true
✅ 本题的目标是：
> **找到一条合法的完整路径**（只要一条），并在找到时立即停止递归（剪枝）。
- 这时候通过 `return true` 把“找到了”这个信号一直传递**返回上层**；
- 所有递归层都会接收到：
    `if (backTracking(...)) return true;`
然后也立刻返回，不再尝试其他分支；
- **最终剪枝成功！只找一条路径就结束了！**

![cc0d11feccf746107412375d2d9e1b8.jpg](https://cdn.jsdelivr.net/gh/hoo01/image_auto/cc0d11feccf746107412375d2d9e1b8.jpg)

### leetcode51 N-queens

```java
class Solution {
    List<List<String>> res = new ArrayList<>();

    public List<List<String>> solveNQueens(int n) {
        char[][] chessboard = new char[n][n];
        for (char[] c : chessboard) {
            Arrays.fill(c, '.');
        }
        backTrack(n, 0, chessboard);
        return res;
    }


    public void backTrack(int n, int row, char[][] chessboard) {
        if (row == n) {
            res.add(Array2List(chessboard));
            return;
        }

        for (int col = 0;col < n; ++col) {
            if (isValid (row, col, n, chessboard)) {
                chessboard[row][col] = 'Q';
                backTrack(n, row+1, chessboard);
                chessboard[row][col] = '.';
            }
        }

    }


    public List Array2List(char[][] chessboard) {
        List<String> list = new ArrayList<>();

        for (char[] c : chessboard) {
            list.add(String.copyValueOf(c));
        }
        return list;
    }


    public boolean isValid(int row, int col, int n, char[][] chessboard) {
        // 检查列
        for (int i=0; i<row; ++i) { // 相当于剪枝
            if (chessboard[i][col] == 'Q') {
                return false;
            }
        }

        // 检查45度对角线
        for (int i=row-1, j=col-1; i>=0 && j>=0; i--, j--) {
            if (chessboard[i][j] == 'Q') {
                return false;
            }
        }

        // 检查135度对角线
        for (int i=row-1, j=col+1; i>=0 && j<=n-1; i--, j++) {
            if (chessboard[i][j] == 'Q') {
                return false;
            }
        }
        return true;
    }
}
```

#### array2List
把 **一个棋盘**（二维字符数组 `char[][]`）转成 **一个解**（一维字符串列表 `List<String>`）
假设某个合法棋盘 `chessboard` 是：
```java
char[][] chessboard = {
    {'.', 'Q', '.', '.'},
    {'.', '.', '.', 'Q'},
    {'Q', '.', '.', '.'},
    {'.', '.', 'Q', '.'}
};
```

第一次循环：
- `char[] c = chessboard[0] = {'.', 'Q', '.', '.'}`
- `String.copyValueOf(c)` → `".Q.."` ✅
- 加入列表：`list = [".Q.."]`

最后得到列表`list = [".Q..", "...Q", "Q...", "..Q."]`

### leetcode37 sudoku solver
```java
class Solution {
    public void solveSudoku(char[][] board) {
        solveSudokuHelper(board);
    }

    private boolean solveSudokuHelper(char[][] board){
        //「一个for循环遍历棋盘的行，一个for循环遍历棋盘的列，
        // 一行一列确定下来之后，递归遍历这个位置放9个数字的可能性！」
        for (int i = 0; i < 9; i++){ // 遍历行
            for (int j = 0; j < 9; j++){ // 遍历列
                if (board[i][j] != '.'){ // 跳过原始数字
                    continue;
                }
                for (char k = '1'; k <= '9'; k++){ // (i, j) 这个位置放k是否合适
                    if (isValidSudoku(i, j, k, board)){
                        board[i][j] = k;
                        if (solveSudokuHelper(board)){ // 如果找到合适一组立刻返回
                            return true;
                        }
                        board[i][j] = '.';
                    }
                }
                // 9个数都试完了，都不行，那么就返回false
                return false;
                // 因为如果一行一列确定下来了，这里尝试了9个数都不行，说明这个棋盘找不到解决数独问题的解！
                // 那么会直接返回， 「这也就是为什么没有终止条件也不会永远填不满棋盘而无限递归下去！」
            }
        }
        // 遍历完没有返回false，说明找到了合适棋盘位置了
        return true;
    }

    /**
     * 判断棋盘是否合法有如下三个维度:
     *     同行是否重复
     *     同列是否重复
     *     9宫格里是否重复
     */
    private boolean isValidSudoku(int row, int col, char val, char[][] board){
        // 同行是否重复
        for (int i = 0; i < 9; i++){
            if (board[row][i] == val){
                return false;
            }
        }
        // 同列是否重复
        for (int j = 0; j < 9; j++){
            if (board[j][col] == val){
                return false;
            }
        }
        // 9宫格里是否重复
        int startRow = (row / 3) * 3;
        int startCol = (col / 3) * 3;
        for (int i = startRow; i < startRow + 3; i++){
            for (int j = startCol; j < startCol + 3; j++){
                if (board[i][j] == val){
                    return false;
                }
            }
        }
        return true;
    }
}
```

## 3.2贪心算法
### leetcode455
小饼干喂小同学
```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int g_index = 0;
        int count = 0;
        for (int s_start = 0; s_start < s.length && g_index < g.length; s_start++) {
            if (s[s_start] >= g[g_index]) {
                g_index++;
                count++;
            }
        }
        return count;
    }
}
```

### leetcode376wiggle subsequence
```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length == 1) {
            return nums.length;
        }
        int result = 1;//默认最右边有一个上坡
        int prediff = 0;
        int curdiff = 0;
        for (int i = 0; i < nums.length - 1; i++) {//最右边有一个上坡，所以减一。或者这里的-1和下面的nums[i + 1]对应更好理解
            curdiff = nums[i + 1] - nums[i];
            if ((prediff <=0 && curdiff > 0) || (prediff >=0 && curdiff < 0)) {
                result++;
                prediff = curdiff;
            }
        }
        return result;  
    }
}
```

#### ❌ 问题 1：循环越界

`for (int i = 0; i < nums.length; i++) {curdiff = nums[i + 1] - nums[i]; }`
这个循环写法 **会在最后一次 i == nums.length - 1 时，访问 nums[i + 1]，越界抛异常**！

#### ❌ 问题 2：`result = 0` 会漏掉第一个元素
即使你提前判断了长度为1的情况，但对于像 `[1, 2, 3]` 这种输入：
- 第一次出现上坡：`2 - 1 = +1`，符合条件，`result = 1`
- 但 `result` 是从 `0` 开始的，所以总共就只算一次，其实应该返回 `2`（包括初始元素）

### leetcode53 最大子序列和
连续子序列和是负数，就抛弃
但不是遇上负数就抛弃
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int result = nums[0];
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            if (sum < 0) {
                sum = 0;
            }
            sum += nums[i];
            if (sum > result) {
                result = sum;
            }
        }
        return result; 
    }
}
```

### leetcode122 Best Time to Buy and Sell Stock Ⅱ
```java
class Solution {
    int diff = 0;
    int sum = 0;
    public int maxProfit(int[] prices) {
        if (prices.length == 1) return 0;
        for (int i = 0; i < prices.length - 1; i++) {
            diff = prices[i + 1] - prices[i];
            if (diff < 0) {
                diff = 0;
            }
            sum += diff;
        }
        return sum;
    }
}
```

### leetcode55 jump game

破题点：最大覆盖范围
![image.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/20250603160821.png)

i 每次移动只能在 cover 的范围内移动，每移动一个元素，cover 得到该元素数值（新的覆盖范围）的补充，让 i 继续移动下去。而 cover 每次只取 max(该元素数值补充后的范围, cover 本身范围)。如果 cover 大于等于了终点下标，直接 return true 就可以了。

如果i > cover，说明跳不出来了，提前退出。

```java
class Solution {
    public boolean canJump(int[] nums) {
        int cover = 0;
        if (nums.length == 1) return true;
        for (int i = 0; i <= cover; i++) {
            cover = Math.max(i + nums[i], cover);
            if (cover + 1 >= nums.length) return true;
        }
        return false;
    }
}
```

### leetcode45 jump game Ⅱ(最小跳跃次数)

> “跳到终点**需要的最少次数**？”

不再关心能不能跳到了，题目已经保证**一定能跳到**终点，我们现在要**贪心地跳得少**。

所以要**遍历整条路**，直到接近终点：
`for (int i = 0; i < nums.length - 1; i++) {`
我们遍历到最后一个之前，因为一旦我们跳到最后一个位置，题目就完成了，没必要再走下去。

| 点         | Jump Game I                     | Jump Game II                        |
| --------- | ------------------------------- | ----------------------------------- |
| 是否一定能跳到终点 | ❌ 不一定（要判断）                      | ✅ 一定能跳到                             |
| 目标        | 能不能跳到最后                         | 最少跳几次                               |
| for 条件    | `i <= coverDistance`：只能走到能覆盖的位置 | `i < nums.length - 1`：遍历整个数组直到倒数第二个 |
| 是否中途退出    | 是，跳不过去就 return false            | 否，保证能跳到，要统计跳跃次数                     |
```java
class Solution {
    public int jump(int[] nums) {
        int maxcover = 0;
        int currcover = 0;
        int step = 0;
        if (nums.length == 1) return step;
        for (int i = 0; i < nums.length; i++) {
            maxcover = Math.max(maxcover, i + nums[i]);
            if (maxcover >= nums.length - 1) {
                step++;
                break;
            }

            if (i == currcover) {
                // 走到了最大范围的下标，需要更新
                currcover = maxcover;
                step++;
            }
        }
        return step;
    }
}
```

✅ 关键贪心思想：

> 每次在你当前跳跃范围内，找到**下一次能跳最远的位置**，只要走到边界，就跳一次。

也就是说：

- 我不急着每走一步就跳，而是看这一跳**最多能到哪里**；
- 然后走完这一跳的范围后，我再决定跳；
- 这叫做 **延迟跳跃**：尽可能多地覆盖范围，到了边界再跳。

### leetcode1005 k次取反后最大化的数组和

贪心的思路，局部最优：让绝对值大的负数变为正数，当前数值达到最大，整体最优：整个数组和达到最大。

局部最优可以推出全局最优。

那么如果将负数都转变为正数了，K依然大于0，此时的问题是一个有序正整数序列，如何转变K次正负，让 数组和 达到最大。

那么又是一个贪心：局部最优：只找数值最小的正整数进行反转，当前数值和可以达到最大（例如正整数数组{5, 3, 1}，反转1 得到-1 比 反转5得到的-5 大多了），全局最优：整个 数组和 达到最大。

虽然这道题目大家做的时候，可能都不会去想什么贪心算法，一鼓作气，就AC了。

**我这里其实是为了给大家展现出来 经常被大家忽略的贪心思路，这么一道简单题，就用了两次贪心！**

那么本题的解题步骤为：

- 第一步：将数组按照绝对值大小从大到小排序，**注意要按照绝对值的大小**
- 第二步：从前向后遍历，遇到负数将其变为正数，同时K--
- 第三步：如果K还大于0，那么反复转变数值最小的元素，将K用完
- 第四步：求和

```java
// 版本二：排序数组并贪心地尽可能将负数翻转为正数，再根据剩余的k值调整最小元素的符号，从而最大化数组的总和。
class Solution {
    public int largestSumAfterKNegations(int[] nums, int k) {
        if (nums.length == 1) return nums[0];

        // 排序：先把负数处理了
        Arrays.sort(nums); 

        for (int i = 0; i < nums.length && k > 0; i++) { // 贪心点, 通过负转正, 消耗尽可能多的k
            if (nums[i] < 0) {
                nums[i] = -nums[i];
                k--;
            }
        }

        // 退出循环, k > 0 || k < 0 (k消耗完了不用讨论)
        if (k % 2 == 1) { // k > 0 && k is odd：对于负数：负-正-负-正
            Arrays.sort(nums); // 再次排序得到剩余的负数，或者最小的正数
            nums[0] = -nums[0];
        }
        // k > 0 && k is even，flip数字不会产生影响: 对于负数: 负-正-负；对于正数：正-负-正 

        int sum = 0;
        for (int num : nums) { // 计算最大和
            sum += num;
        }
        return sum;
```

### leetcode134加油站
#### 方法一

直接从全局进行贪心选择，情况如下：

- 情况一：如果gas的总和小于cost总和，那么无论从哪里出发，一定是跑不了一圈的
    
- 情况二：`rest[i] = gas[i]-cost[i]`为一天剩下的油，i从0开始计算累加到最后一站，如果累加没有出现负数，说明从0出发，油就没有断过，那么0就是起点。
    
- 情况三：如果累加的最小值是负数，汽车就要从非0节点出发，从后向前，看哪个节点能把这个负数填平，能把这个负数填平的节点就是出发节点。

- `sum`：统计整个路程中 `总共赚了多少油`，如果 `sum < 0`，直接返回 -1，因为油不够跑一圈。
    
- `min`：记录这个过程中的 **最小累计油量**（即当前时刻最“缺油”的那一刻）。

```java
// 解法1
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int sum = 0;
        int min = 0;
        for (int i = 0; i < gas.length; i++) {
            sum += (gas[i] - cost[i]);
            min = Math.min(sum, min);
        }

        if (sum < 0) return -1;
        if (min >= 0) return 0;

        for (int i = gas.length - 1; i > 0; i--) {
            min += (gas[i] - cost[i]);
            if (min >= 0) return i;
        }

        return -1;
    }
}
```

#### 方法二
可以换一个思路，首先如果总油量减去总消耗大于等于零那么一定可以跑完一圈，说明 各个站点的加油站 剩油量`rest[i]`相加一定是大于等于零的。

每个加油站的剩余量`rest[i]为gas[i] - cost[i]`。

i从0开始累加`rest[i]`，和记为curSum，一旦curSum小于零，说明`[0, i]`区间都不能作为起始位置，因为这个区间选择任何一个位置作为起点，到i这里都会断油，那么起始位置从i+1算起，再从0计算curSum。

如图：
![image.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/20250604151135.png)
那么为什么一旦[0，i] 区间和为负数，起始位置就可以是i+1呢，i+1后面就不会出现更大的负数？

如果出现更大的负数，就是更新i，那么起始位置又变成新的i+1了。

### leetcode135 candy
先比较一边，再比较另一边
```java
class Solution {
    public int candy(int[] ratings) {
        int[] candies = new int[ratings.length];
        Arrays.fill(candies, 1);
        // 右边大于左边
        for (int i = 0; i < ratings.length - 1; i++) {
            if (ratings[i + 1] > ratings[i]) {
                candies[i + 1] = candies[i] + 1;
            }
        }

        // 左边大于右边
        for (int j = ratings.length - 1; j >= 1; j--) {
            if (ratings[j - 1] > ratings[j]) {
                candies[j - 1] = Math.max(candies[j - 1], candies[j] + 1);
            }
        }

        int result = 0;
        for (int c : candies) {
            result += c;
        }
        return result;
    }
}
```

### leetcode406 根据身高重建队列
多维排序，一定是每个维度分开排
`[h,k]`
先尝试按k的维度排，从左到右，从小到大排，如下：
![060401.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/060401.PNG)
尝试了发现不行
再按照h的维度排，从大到小排
![060402.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/060402.PNG)

按照h排序之后，按照每个人的 k 值插入到 index = k 的位置
后序插入节点也不会影响前面已经插入的节点，最终按照k的规则完成了队列。
![060403.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/060403.PNG)

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        // 身高从大到小排（身高相同k小的站前面）
        Arrays.sort(people, (a, b) -> {
            if (a[0] == b[0]) return Integer.compare(a[1], b[1]);   // a - b 是升序排列，故在a[0] == b[0]的狀況下，會根據k值升序排列
            return Integer.compare(b[0], a[0]);   //b - a 是降序排列，在a[0] != b[0]，的狀況會根據h值降序排列
        });

        LinkedList<int[]> que = new LinkedList<>();

        for (int[] p : people) {
            que.add(p[1],p);   //Linkedlist.add(index, value)可以随意插入，按每个人的K值插入到 index=k 的位置中
        }

        return que.toArray(new int[people.length][]);
    }
}
```

```java
return que.toArray(new int[people.length][]);
```

`que` 其实是一个**装着一维数组的列表**，本质上是二维结构，像这样
```java
que = [
    [5, 0],
    [7, 0],
    [5, 2],
    ...
]
```
- `que` 是 `LinkedList<int[]>`，传入的是一个 `int[][]` 类型的空数组模板，于是 `toArray(...)` 返回的是一个 `int[][]` 类型的二维数组，会自动把列表里的每个 `int[]` 拷贝到新的二维数组里。
- 如果直接`que.toArray()` 返回的是一个`Object[]`类型

### leetcode452 射气球

```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        Arrays.sort(points, (a, b) -> Integer.compare(a[0], b[0]));

        int count = 1;  // points 不为空至少需要一支箭
        for (int i = 1; i < points.length; i++) {
            if (points[i][0] > points[i - 1][1]) {  // 气球i和气球i-1不挨着，注意这里不是>=
                count++; // 需要一支箭
            } else {  // 气球i和气球i-1挨着
                points[i][1] = Math.min(points[i][1], points[i - 1][1]); // 更新重叠气球最小右边界
            }
        }
        return count;
    }
}
```
### leetcode435 non-overlapping intervals
![060501.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/060501.PNG)

当发现当前区间和前一个区间重叠时，我们要选择**两个区间中“较小的结束时间”，即选择保留结束时间较早的区间，这样可以保证尽量减少区间的重叠范围。尽量保留不重叠的区间，从而达到最小删除区间的目的。

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        int count = 0;
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i - 1][1] > intervals[i][0]) {
                count++;
                intervals[i][1] = Math.min(intervals[i - 1][1], intervals[i][1]);
            }
        }
        return count;
    }
}
```

### leetcode763 partition labels

```java
class Solution {
    public List<Integer> partitionLabels(String S) {
        int[] hash = new int[26]; 
        for (int i = 0; i < S.length(); i++) {
            hash[S.charAt(i) - 'a'] = i; // 记录每个字符最后出现的位置
        }

        List<Integer> result = new ArrayList<>();
        int left = 0;
        int right = 0;

        for (int i = 0; i < S.length(); i++) {
            right = Math.max(right, hash[S.charAt(i) - 'a']);
            if (i == right) {
                result.add(right - left + 1);
                left = i + 1;
            }
        }

        return result;
    }
}
```

- 统计每一个字符最后出现的位置，确定分界线
- 从头遍历字符，并更新字符的最远出现下标，如果找到字符最远出现位置下标和当前下标相等了，则找到了分割点
#### step1 预处理
```java
for (int i = 0; i < S.length(); i++) {
            hash[S.charAt(i) - 'a'] = i;
        }
```

以字符串s = "abaccbdeffed"为例
hash`[S.charAt(i) - 'a']` 用ASCII码差值替代字母，`hash[0] 代表 a, hash[1] 代表 b`
i = 0
`hash['a' - 'a'] = 0 -> hash[0] = 0`

i = 1
`hash['b' - 'a'] = 2 -> hash[1] = 1`

i = 2
`hash['a' - 'a'] = 0 -> hash[0] = 2
得到的hash数组

|字符|最后出现位置|
|---|---|
|a|2|
|b|5|
|c|4|
|d|11|
|e|10|
|f|9|
i 值不断更新每个字母的最远出现位置，这些信息被保存在 `hash` 数组里，用来辅助下面的逻辑。
#### step2动态更新分段
![fce9c262ee23a73d45000d306368f3e.jpg](https://cdn.jsdelivr.net/gh/hoo01/image_auto/fce9c262ee23a73d45000d306368f3e.jpg)

```java
        int left = 0;
        int right = 0;

        for (int i = 0; i < S.length(); i++) {
            right = Math.max(right, hash[S.charAt(i) - 'a']);
            if (i == right) {
                result.add(right - left + 1);
                left = i + 1;
            }
        }
```

我们从 i = 0 开始，遍历字符串。

i = 0：字符是 'a'
right = max(0, hash['a']) = max(0, 2) = 2

i = 1：字符是 'b'
right = max(2, hash['b']) = max(2, 5) = 5

i = 2：字符是 'a'
right = max(5, hash['a']) = 5 不变

i = 3：字符是 'c'
right = max(5, hash['c']) = max(5, 4) = 5 不变

i = 4：字符是 'c'
right = 5

i = 5：字符是 'b'
right = 5

此时 i == right，满足切分条件！
✅ 第一个区间是：从 left = 0 到 right = 5，长度 = 6
👉 result.add(6)

更新 left = 6

i = 6：字符是 'd'
right = max(6, hash['d']) = max(6, 11) = 11

i = 7：字符是 'e'
right = max(11, hash['e']) = 11

i = 8：字符是 'f'
right = max(11, hash['f']) = 11

i = 9：字符是 'f'，right 仍然是 11
i = 10：字符是 'e'
i = 11：字符是 'd'
i == right，满足切分条件！
✅ 第二个区间：从 left = 6 到 right = 11，长度 = 6
👉 result.add(6)

更新 left = 12

### leetcode56 merge intervals
数组是固定长度，没有add方法，只能转成list再转回
```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        int left = intervals[0][0];
        int right = intervals[0][1];

        List<int[]> res = new ArrayList<>();

        for (int i = 1; i < intervals.length; i++) {
            if (right >= intervals[i][0]) {
                right = Math.max(right, intervals[i][1]);
            } else {
                res.add(new int[]{left, right}); //添加前一个合并结果
                left = intervals[i][0];
                right = intervals[i][1];
            }
        }
        
        res.add(new int[]{left, right});
        return res.toArray(new int[res.size()][]);
    }
}
```

### [738. Monotone Increasing Digits](https://leetcode.com/problems/monotone-increasing-digits/)

该题中的要求：
- **逐位操作数字**（比如判断第 i 位和 i+1 位的关系，或者修改某一位），
- 输入类型是int，Java 是强类型语言，不允许直接把 `int` 当作多个字符处理。而一个整数 `1234` 这个数值本身，不等价于字符 `'1', '2', '3', '4'`，它只是一个数字，存储在内存中是二进制。

✅ 用字符数组的好处：
- `char[]` 可以轻松做到：`digits[i]` 拿到第 i 位数字（字符形式）；把每一位数字拿出来操作比如“3”“2”“1”这些字符，**必须先用 `String` 表达这个数**，再拆成字符数组`'0'`、`'1'`、`'2'` …… `'9'`。

`char`运算
在 Java 中，`char` 是一种整数类型，本质是一个 16 位的无符号整数，对应 **Unicode 编码（即 ASCII 扩展）**。
所以你可以像对整数那样对字符进行：
- 比较大小（`<`, `>`, `==`）
- 加减运算（`'4' - 1 = '3'`）
```java
System.out.println('2' < '4'); // true，因为 50 < 52
System.out.println((char)('4' - 1)); // 输出 '3'
```

```java
class Solution {
    public int monotoneIncreasingDigits(int n) {
        if (n == 0) return 0;
        char[] digits = Integer.toString(n).toCharArray();
        int mark = digits.length; 注意

        for (int i = digits.length - 1; i > 0; i--) {
            if (digits[i] < digits[i - 1]) {
                mark = i;
                digits[i - 1]--;
            }
        }

        for (int j = mark; j < digits.length; j++) {
            digits[j] = '9'; 注意
        }
  
        String s = new String(digits);  
        int result = Integer.parseInt(s);
        return result;
    }
}
```

✅方法二:用stringBuilder
```java
StringBuilder sb = new StringBuilder(Integer.toString(n));
int mark = sb.length();

for (int i = sb.length() - 1; i > 0; i--) {
    if (sb.charAt(i) < sb.charAt(i - 1)) {
        mark = i;
        sb.setCharAt(i - 1, (char)(sb.charAt(i - 1) - 1));
    }
}

for (int i = mark; i < sb.length(); i++) {
    sb.setCharAt(i, '9');
}

return Integer.parseInt(sb.toString());
```

虽然 `sb.charAt(...)` 是 `char`，但**一旦参与了减法 `- 1` 运算**，Java 自动把它提升为 `int`，所以最终结果是 `int` 类型，必须强制转回 `char` 才能赋值。在 Java 里，只要你对 `char`、`byte`、`short` 做数学运算（比如加减乘除），它们就会自动**“提升”为 `int` 类型**！

### 968 摄像头放置二叉树
#### 总体思路：
我们从下往上考虑摄像头的放置 —— 也就是用「后序遍历」的方式，每次判断一个节点的两个子节点的状态，从而决定是否在当前节点放摄像头。

选择后序遍历，左 右 中
#### 节点状态：
0 无覆盖，（最终状态不能有任何一个节点是状态0，但是递归过程中可以存在）
1 有摄像头，
2 有覆盖

Null（空节点）只能是状态2
#### 算法步骤
- 分析左右子节点的返回状态：
    - **如果左右孩子中有一个是0（未被覆盖）**，那么当前节点必须安装摄像头 → 返回 `1`
    - **如果左右孩子中有一个是1（有摄像头）**，那当前节点是被监控到的 → 返回 `2`
    - **如果左右孩子都是2（已被覆盖）**，说明当前节点没被覆盖 → 返回 `0`
- 最后，根节点如果是0，还要多装一个摄像头

```java
class Solution {
    int count;
    public int minCameraCover(TreeNode root) {
        count = 0;
        if (backtracking(root) == 0) {
            count++;
        }
        return count;
    }

    private int backtracking(TreeNode node) {
        if (node == null) return 2;//终止条件

        int left = backtracking(node.left);//左
        int right = backtracking(node.right);//右

        if (left == 0 || right == 0) {//中
            count++;
            return 1;
        }
        if (left == 1 || right == 1) return 2;
        return 0;
    }
}
```

## 3.3动态规划
### 动态规划五部曲
1. dp数组及下标的含义
2. 递推公式
3. dp数组如何初始化
4. 遍历顺序
5. 打印dp数组（debug）
### leetcode509 斐波那契数列
1.确定dp[i] -- 第i个数字
2.递推公式 -- dp[i] = dp[i - 1] + dp[i - 2]
3.dp数组初始化 -- dp[0] = 0, dp[1] = 1
4.遍历顺序 -- 从前向后
```java
class Solution {
    public int fib(int n) {
        if (n <= 1) return n;
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];   
    }
}
```

### leetcode70 爬楼梯
dp[i-2]爬两步到dp[i]，dp[i-1]爬一步到dp[i]，都分别对应一种方法
```java
class Solution {
    public int climbStairs(int n) {
        if (n == 1) return 1;
        int[] dp = new int[n + 1];
        dp[1] = 1;
        dp[2] = 2;
        for (int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
}
```

### leetcode746 min cost climbing stairs
dp[i] -- 到达`i`位置的花费
```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int[] dp = new int[cost.length + 1];
        dp[0] = 0; // 代表从地面起跳到第0阶
        dp[1] = 0; // 代表从地面起跳到第1阶
        for(int i = 2; i <= cost.length; i++) {
             dp[i] = Math.min(dp[i - 1] + cost[i - 1],  // 从i-1跳一步
                     dp[i - 2] + cost[i - 2]); // 从i-2跳两步
        }
        return dp[cost.length];
    }
}
```

### leetcode62 最短路径
`dp[i][j]`的含义是，到此处有多少种路径，而不是有多少步
初始化：最左面和最上面都是1，如果不填入的话默认是0
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        for (int a = 0; a < m; a++) {
            dp[a][0] = 1;
        }
        for (int b = 0; b < n; b++) {
            dp[0][b] = 1;
        }

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```

### leetcode62 最短路径2
```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[][] dp = new int[m][n];

        for (int a = 0; a < m; a++) {
            if (obstacleGrid[a][0] == 1) {
                dp[a][0] = 0;
                break;
            } 
            dp[a][0] = 1;
            

        }
        for (int b = 0; b < n; b++) {
            if (obstacleGrid[0][b] == 1) {
                dp[0][b] = 0;
                break;
            }
            dp[0][b] = 1;
        }

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 1) {
                    dp[i][j] = 0;
                } else
               { dp[i][j] = dp[i - 1][j] + dp[i][j - 1]; }
            }
        }
        return dp[m - 1][n - 1];
    }
}
```

### leetcode343整数拆分
dp[i] -- `i`拆分得到的最大乘积

**递推公式 --** 假设你当前在求 `dp[i]`，你打算拆一刀，拆成：
- 一段是 `j`（j从1到 i-1）
- 剩下的部分是 `i-j`

那么 `i` 的拆法有两种情况：
1️⃣ 直接拆成 `j` 和 `(i-j)`，两个数字不再拆：乘积是 `j * (i-j)`
2️⃣ 拆成 `j` 和 `(i-j)`，但是 `(i-j)` **还可以继续拆**，继续拆的最好乘积是 `dp[i-j]`：乘积是 `j * dp[i-j]`

为什么不拆`j` ? : 拆`i - j`也包含拆`j`的情况，如果非要拆成dp[j]`*`dp[i - j],那就默认必须拆成4种情况，不正确，而且也不符合我们对dp[i]的定义

所以dp[i]更新为：
```java
dp[i] = Math.max(dp[i], Math.max(j*(i-j), j*dp[i-j]));
```
这里比较的是：

1️⃣ **dp[i]**：当前 `dp[i]` 存下的历史最大值，防止被新一轮更新反而弄小了。

>**为什么需要两项都考虑？无法预知 (i-j) 是不是还值得继续拆**：

- 有些时候，**直接用 (i-j) 整体乘**效果更好（比如小的i-j直接用数字本身就大）。
- 有些时候，**dp[i-j]，即 (i-j) 继续拆的乘积反而更大**（比如i-j较大时，继续拆出多个3，整体乘积大于直接用i-j）。

👉 `Math.max` 方法只支持 **两个参数**

**递推逻辑 --** 

外层循环 `i` 的范围？
你要计算 `dp[2]` 到 `dp[n]`，所以：
```java
for (int i = 3; i <= n; i++) {
```

内层循环 `j` 的范围？
你枚举**第一刀怎么切**，切出一个 `j`，剩下是 `i-j`：
```java
for (int j = 1; j <= i - 1; j++) {
```
要保证 `(i-j)` 至少是 1，不能为 0（否则没意义，无法拆成两个正整数）。所以 `j` 最大只能取到 `i-1`，此时 `(i-j)` == 1，仍然合法。

```java
class Solution {
    public int integerBreak(int n) {
        int[] dp = new int[n + 1];
        dp[2] = 1;
        for (int i = 3; i <= n; i++) {
            for (int j = 1; j <= i - 1; j++) {
                dp[i] = Math.max(dp[i], Math.max(j * (i - j), j * dp[i - j]));
            }
        }
        return dp[n];
    }
}
```

### leetcode 96 unique binary search tree

![060702.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/060702.PNG)
![0f1802cbc7d286a2c688f85fe57e184.jpg](https://cdn.jsdelivr.net/gh/hoo01/image_auto/0f1802cbc7d286a2c688f85fe57e184.jpg)

```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1]; //代表累加,并非只有当前
        dp[1] = 1;
        dp[0] = 1;
        for (int i = 2; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                dp[i] += dp[j - 1] * dp[i - j]; //从小到大去遍历
            }
        }
        return dp[n];
    }
}
```
### leetcode 95 unique binary search treeⅡ
#### 返回数据类型 - `List<TreeNode>`
👉`List<TreeNode>` → 就是返回「很多棵树」。
每一个 `TreeNode` 是「一棵树的根节点」：
- 你可以通过 `TreeNode.left` / `TreeNode.right` 访问整棵树。
- LeetCode 的示例 `[1,null,2,null,3]` 是后台自动序列化你的 `TreeNode` 返回值而已，方便人类看结果。
👉 **整个 List 里有多少个 TreeNode，表示就生成了多少棵不同形状的 BST。**

#### 🌳 **核心思路 -- 递归建树**

- 你要生成 **所有不同形态的二叉搜索树**，节点值是 `1 ~ n`。
- BST 的性质是：
    - 一个根节点 `i`，左子树的节点值只能来自 `1 ~ (i-1)`，右子树只能来自 `(i+1) ~ n`。
- 所以 **可以枚举每个 i 作为根节点**，递归地生成左右子树的所有可能组合，再拼接成树。

#### 🏗️ **递归定义**

写一个函数 `generateTrees(start, end)`：
- 返回 `[所有从 start 到 end 组成的 BST 树的根节点列表]`。
- 整体初始是 `generateTrees(1, n)`
- 每次选根 i 后，左右子树范围收缩成 `[start, i-1]` 和 `[i+1, end]`
- 最终递归到 start > end 返回 null（空子树）

递归拆解思路：
    - 枚举 `i ∈ [start, end]` 作为根节点：
        - 左子树 = `generateTrees(start, i-1)`
        - 右子树 = `generateTrees(i+1, end)`
        - 每一种 `左子树 x 右子树` 的组合都可以作为以 `i` 为根的树。
Base Case：
    - 如果 `start > end`，说明子树为空，返回 `[null]`，表示「空树」也是一种情况（用于拼接左右子树时作为叶子节点的空孩子）。

```java
class Solution {
    public List<TreeNode> generateTrees(int n) {
        return generateTrees(1, n);
    }

    private List<TreeNode> generateTrees(int start, int end) {
        List<TreeNode> allTrees = new ArrayList<>();
        if (start > end) {
            allTrees.add(null); // 如果递归到 start > end，就相当于这个方向上是空节点 null。
            return allTrees;
        }

        // 枚举所有可能的根节点 i
        for (int i = start; i <= end; i++) {
            // 所有可能的左子树
            List<TreeNode> leftTrees = generateTrees(start, i - 1);
            // 所有可能的右子树
            List<TreeNode> rightTrees = generateTrees(i + 1, end);

            // 组合左右子树
            for (TreeNode left : leftTrees) {
                for (TreeNode right : rightTrees) {
                    TreeNode root = new TreeNode(i);
                    root.left = left;
                    root.right = right;
                    allTrees.add(root);
                }
            }
        }
        return allTrees;
    }
}
```

![546b27022baa9ad16fe4da24a4bfc6d.jpg](https://cdn.jsdelivr.net/gh/hoo01/image_auto/546b27022baa9ad16fe4da24a4bfc6d.jpg)

### 传统01背包问题

|     | 重量  | 价值  |
| --- | --- | --- |
| 物0  | 1   | 15  |
| 物1  | 3   | 20  |
| 物2  | 4   | 30  |
背包最大重量为4 
#### 问题描述
- 有若干个物品（每种物品只能选0或1次）
- 每个物品有一个**重量** w[i]和一个**价值** v[i]
- 有一个容量为 C 的背包
- **目标**：选一些物品放入背包，**总重量 ≤ C**，使得**总价值最大**

#### 二维数组
##### `dp[i][j]`
👉 **从前 i 个物品**（下标 0 到 i）中任意选，**放入容量为 j 的背包**，所能得到的**最大总价值**。
##### 状态转移（递推）思路
对于 **第 i 个物品**，有两个选择：
1. 不放入背包
- 那么 `dp[i][j]` 和之前一样，相当于只看前 i-1 个物品，背包容量还是 j。
- 价值：`dp[i-1][j]`

2. 放入背包
- 这个前提是`j >= w[i]`，背包得放得下。
- 那么此时：
    - 把 w[i]重量腾出来给物品 i 放
    - 剩余容量是`j − w[i]`，之前 i-1 个物品里对这个剩余容量能装多少价值？答案是 `dp[i-1][j-w[i]]`
    - 再加上当前物品 i 的价值`v[i]`
- 价值：`dp[i-1][j-w[i]] + v[i]`
`dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - w[i]] + v[i])`
##### 二维dp数组初始化
###### 第一种做法：
```java
int n = 物品个数;
int C = 背包容量;
int[][] dp = new int[n][C + 1];
```

**初始化为 0** 就可以了，因为：
- 初始情况下，**如果不选任何物品**，任何容量 j 都是价值 0 → 符合题意。
- Java 默认 `int[][] dp = new int[n][C + 1]` 是全 0，**不用额外手动填 0**
###### 第二种做法：
手动初始化第一行 `dp[0][j]`


|     | 0   | 1   | 2   | 3   | …   |
| --- | --- | --- | --- | --- | --- |
| 物0  | 15  | 15  | 15  | 15  |     |
| 物1  |     |     |     |     |     |
| 物2  |     |     |     |     |     |


- `dp[0][j]` 是 **只考虑物品 0** 时的结果：
```java
for (int j = 0; j <= C; j++) {
    if (j >= w[0]) {
        dp[0][j] = v[0]; // 可以放物品0进去
    } else {
        dp[0][j] = 0;    // 放不进去，价值为0
    }
}
```
👉 **这样做更保险、更直观**，尤其是：

- 有时候题目明确要求**必须选至少一个物品**，或者要考虑放第一个物品的情况；
- 手动初始化`dp[0][j]`，可以让 `dp[1][j]` 以后递推更容易理解；
- 也是**教科书标准写法**，很多人比赛/面试会这样写。

如果你直接用：
```java
for (int i = 0; i < n; i++) {
    for (int j = 0; j <= C; j++) {
       dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - w[i]] + v[i]);
    }
}
```
**问题就出在 i=0 的时候**：

- `dp[0][j]` 按公式会访问 `dp[-1][j]` → **越界**，不合法。
- 所以**你不能用递推公式计算 `dp[0][j]`，而应该手动初始化它 → **告诉程序第一行 `dp[0][j] `应该是什么**（要么是 0，要么是放了物品0）。

👉 这就是为什么**i=0这一行必须手动初始化**！

如果你写：
```java
for (int i = 1; i < n; i++) {
    for (int j = 0; j <= C; j++) {
       dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - w[i]] + v[i]);
    }
}
```
这样就**跳过了 i=0 的递推**，不会出现 `dp[-1][j]`的问题。
##### 遍历顺序
对于01背包二维数组，先遍历物品还是先遍历背包都是可以的

##### 更新顺序
```java
for (int i = 0; i < n; i++) {
    for (int j = 0; j <= C; j++) {
        if (j >= w[i]) {
            dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - w[i]] + v[i]);
        } else {
            dp[i][j] = dp[i - 1][j];
        }
    }
}
```

👉 更新 `dp[i][j]` **只依赖于上一行 `dp[i-1][...]`**，和 j 遍历顺序**无关**。
所以 j++ / j-- **都可以**，因为当前`dp[i][j]`的更新 **不会影响` dp[i-1][...]`**。
#### 一维数组
|     | 重量  | 价值  |
| --- | --- | --- |
| 物0  | 1   | 15  |
| 物1  | 3   | 20  |
| 物2  | 4   | 30  |
`dp[j]`：容量为`j`的背包，当前能取得的最大价值

递推公式：`dp[j] = max(dp[j] , (dp[j- w[i]]) + v[i])`
👉本质还是放不放入物品`[i]`
- 如果 **不选**，`dp[j]` 保持原值。
- 如果 **选**，你要知道 “剩下 `j - w[i]` 容量” 时能放出的最大价值 + 当前物品的价值 `v[i]`。

初始化：
`int[] dp = new int[C + 1];// 默认全是 0`

遍历顺序：
👉 先枚举物品 → 每个物品只更新一轮 `dp[j]` → 保证 **每个物品最多选 1 次**。
👉 内层倒序 j-- → 保证 `dp[j-w[i]]` 用的是 **上一轮 i-1 或更早的 `dp[j-w[i]]`，不被本轮污染。
```java
for (int i = 0; i < n; i++) {
    for (int j = C; j >= w[i]; j--) {  // 倒序！！
        dp[j] = Math.max(dp[j], dp[j - w[i]] + v[i]);
    }
}
```

### leetcode416 分割相等子数组
**Example 1:**

**Input:** nums = [1,5,11,5]
**Output:** true
**Explanation:** The array can be partitioned as [1, 5, 5] and [11].

dp[j] 代表背包容量为 j，放进的最大物品价值
本题，相当于背包里放入数值，那么物品 i 的重量是nums[i]，其价值也是nums[i]。so :
`dp[j] = max(dp[j] , (dp[j- nums[i]]) + nums[i])`

target = sum / 2 , if `dp[target] == target`，说明背包就装满了
- 如果 dp[sum/2] == sum/2，说明 **存在一种选法，恰好凑出容量 sum/2**，且不能再更大了（因为 sum/2 就是容量上限）。
- 如果 dp[sum/2] < sum/2，说明 **无论怎么选，凑不满 sum/2** → 不能拆成两个和相等的子集。
- 如果能找到这样的子集，剩下的那些没选的数自然组成另一个 target → 分成两部分成功 ✅。

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
        }
        if (sum % 2 != 0) return false;
        
        int[] dp = new int[sum/2 + 1]; 
        for (int i = 0; i < nums.length; i++) {
            for (int j = sum/2; j >= nums[i]; j--) {
                dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
            }
        }

        if (dp[sum/2] == sum/2) return true;
        return false; 
    }
}
```

#### boolean版dp[j]
dp[j] = 是否可以凑出和为 j
```java
boolean[] dp = new boolean[sum/2 + 1];
dp[j] = true → 可以凑出和为 j。
```

初始化
```java
dp[0] = true; // 什么都不选，和为 0 总是可以凑出来。
```

状态转移方程
```java
dp[j] = dp[j] || dp[j - nums[i]];
```
意思是：

- 不选 nums[i] → dp[j] 保持原状。
- 选 nums[i] → 如果之前能凑出 j - nums[i]，那现在可以凑出 j。

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int num : nums) sum += num;
        if (sum % 2 != 0) return false;
        
        int target = sum / 2;
        boolean[] dp = new boolean[target + 1];
        dp[0] = true; // 初始，和为 0 总是可以凑出来
        
        for (int num : nums) {
            for (int j = target; j >= num; j--) {
                dp[j] = dp[j] || dp[j - num];
            }
        }
        return dp[target];
    }
}
```

### leetcode [1049. Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)
#### 题目本质 = 分成两个子集，求差值最小
题目说你可以任意“砸石头” → 等价于 **把 stones 分成两堆**，让它们之间最后差的最小。

如果你把 stones 分成两堆：

- 一堆的总和是 sum1
- 另一堆的总和是 sum2

最后剩下的石头重量 = |sum1 - sum2|  
**目标 → 让这个差值尽可能小**！
#### dp[target] 的含义
👉 我们用 **背包 dp 求解**：
- 背包容量 = sum / 2
- `dp[j]` = 不超过 j，能凑出的最大总和

也就是说：`dp[target]` = ≤ sum/2 的最大子集和**

这其实等价于「把 stones 尽量选出一堆，和尽可能接近 sum/2」。

→ 那么另一堆的总和自然是 `sum - dp[target]`
```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for (int num : stones) {
            sum += num;
        }
        int target = sum / 2;
        int[] dp = new int[target + 1];
        for (int i = 0; i < stones.length; i++) {
            for (int j = target; j >= stones[i]; j--) {
                dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]);
            }
        }
        return (sum - 2 * dp[target]);
    }
}
```

### leetcode 494 target sum
posi + nega = sum
posi - nega = target

推出
posi = (target + sum) / 2

target是固定的，sum是固定的，posi就可以求出来。此时就是在集合nums中找出和为posi的组合。**即用nums装满容量为x的背包，有几种办法**

#### 二维dp解法（会超时）
##### 定义
`dp[i][j]` 👉 **用前 i 个数（即 nums[0] 到 nums[i - 1]），凑出和为 j 的方案数**。
可以理解成：**把前 i 个数字「选 or 不选」，去装满容量为 j 的背包，有多少种方法。**
##### 初始化
- `dp[0][0] = 1`  
    → 什么都不选，装满容量 0，1 种方案（选空集）。
- `dp[0][j] = 0` (j ≠ 0)  
    → 什么都不选，无法装满正容量 j，0 种方案。

|i \ j|0|1|2|3|4|
|---|---|---|---|---|---|
|**0**|1|0|0|0|0|
##### 状态转移思路：
对于每一个 `nums[i]`，我们考虑是否使用它来组成当前的容量 j：
（以下为什么是[i - 1]而不是[i],因为**dp 定义的维度和数组 index 不是一一对应**，导致容易混淆，所以尽量用一维数组区分）
两种选择：
1️⃣ **不选`nums[i]`**
- 当前的容量 j 只能靠前 i-1 个数字来凑：  
    `dp[i][j] = dp[i-1][j]`

2️⃣ **选 `nums[i]`**
- 当前的容量 j 来自于前 i-1 个数字凑出 j - nums[i]，再加上 nums[i]：
    `dp[i][j] += dp[i-1][j - nums[i]]`  
    （前提是 `j - nums[i - 1] >= 0`，防止负数越界）

|dp 定义方式|正确使用的 nums 索引|
|---|---|
|`dp[i][j]` 表示用了前 `i` 个数字|应该使用 `nums[i - 1]`|
|`i` 是数组下标（比如 for (int i...)）|直接使用 `nums[i]`|

#### 一维dp写法

```java
int[] dp = new int[p + 1];
dp[0] = 1;

for (int i = 0; i < nums.length; i++) {
    for (int j = p; j >= nums[i]; j--) {
        dp[j] += dp[j - nums[i]];
    }
}
```
**内层循环为何从大到小？**  
 - 避免重复使用同一元素（每个元素只能用一次）。如果 `j` 从小到大，更新 `dp[j]` 时可能使用当前 `nums[i]` 更新过的值（导致元素被多次使用），但从大到小更新可确保 `dp[j - nums[i]]` 基于上一状态（未包含当前 `nums[i]`）。
**转移方程：`dp[j] += dp[j - nums[i]]`**
- `dp[j]`（更新前）：**不使用 `nums[i]`** 时，和为 `j` 的方式数（旧值）。
- **`dp[j - nums[i]]`**：代表**使用当前数字** `nums[i]` 时，拼出剩余部分 `j - nums[i]` 的所有方法数。  （这些方法加上 `nums[i]` 正好达到 `j`）
- **相加**：因为"不使用"和"使用"是**完全独立、互斥的两种决策**！  总方法数 = (不使用的方法数) + (使用的方法数)
**与DFS的关联：这实际上等价于：**
```java
count = 0
def dfs(i, current_sum):
    if i==n: 
        if current_sum==target: count+=1
        return
    # 两种选择：加或减（在转化后问题中就是选或不选）
    dfs(i+1, current_sum)          # 不选当前数字
    dfs(i+1, current_sum+nums[i])  # 选当前数字
```
动态规划中的 `dp[j] + dp[j-nums[i]]` 正是对这两条递归路径的计数！
#### 完整代码
```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if ((sum + target) % 2 != 0 || Math.abs(target) > sum) return 0;
        int p = (sum + target) / 2;
        int[] dp = new int[p + 1];
        dp[0] = 1;
        
        for (int i = 0; i < nums.length; i++) {
            for (int j = p; j >= nums[i]; j--) {
                dp[j] += dp[j - nums[i]]; 
            }
        }
        
        return dp[p];
    }
}
```

### leetcode 474 ones and zeroes
#### `dp[i][j]`
👉 **在剩余 i 个 0、j 个 1 的限制下**，最多能选多少个字符串。
物品（字符串）只能放一次（放或者不放），但是背包的容量有两个维度，可以放 i 个0，j 个 1，在背包的容量限制下，可选最多的物品数量。
#### 遍历顺序
在遇到一个新字符串，视为一个物品，它有 `zeroCount` 个 0、`oneCount` 个 1：
```java
for (String str : strs) { // 先遍历物品
            // 统计当前字符串中的 0 和 1 的数量
            int zc = 0, oc = 0;
            for (char c : str.toCharArray()) {
                if (c == '0') zc++;
                else oc++;
            }
```
#### 转移方程
情况一：不选这个字符串 →  剩余还是`dp[i][j]` 。
情况二：选这个字符串 → 你之前的剩余是 `dp[i - zc][j - oc]`，在这个剩余基础上再选一个字符串 → +1。
选或者不选，选状态最优的那个
```java
dp[i][j] = Math.max(dp[i][j], dp[i - zc][j - oc] + 1);
```

举例：
假设当前有
```java
dp[5][3] = 3   // 用 5 个 0、3 个 1，最多选了 3 个字符串
```
有新字符串“10”，
你可以：
- 不选 `"10"`，保持 `dp[5][3] = 3`。
- 选 `"10"`，你要花掉 1 个 0 和 1 个 1，去看 `dp[5-1][3-1] = dp[4][2]` 这个状态。
假设
```java
dp[4][2] = 2
```
→ 选 `"10"` 后可以达到 `dp[4][2] + 1 = 3`。
最后 `dp[5][3]` 就是 `Math.max(3, 3)`，保持 3。
#### 初始化
**还没选任何字符串，背包是空的。**
- 用 0 个 0 和 0 个 1 → 显然此时能选的字符串数量 = 0。`dp[0][0] = 0;`
- 如果你没有选任何字符串，不管剩多少 0 和 1（背包容量多大），你默认状态是「啥都没选过」，所以**最多选 0 个字符串**，`dp[i][j] = 0`。
#### 代码
```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m + 1][n + 1];

        for (String str : strs) {
            int zc = 0, oc = 0;
            for (char c : str.toCharArray()) {
                if (c == '0') {
                    zc++;
                } else oc++;
            }

            for (int i = m; i >= zc; i--) {
                for (int j = n; j >= oc; j--) {
                    dp[i][j] = Math.max(dp[i][j], dp[i - zc][j - oc] + 1);
                }
            }
            
        }
        return dp[m][n];
    }
}
```

### 完全背包
一种物品可以反复放入背包

**完全背包必须是正序更新：**
- 必须正序，因为 **完全背包允许同一种物品放多次**，需要让 dp[j] 能依赖「当前轮之前更新出来的 dp`[j - weight[i]]`」。
- 你可以理解成：dp`[j - weight[i]] `里已经考虑了放 k 个物品 i，当前 dp`[j]` 就是考虑放 k+1 个物品 i。

**物品和背包的先后遍历顺序可以调**

先遍历物品，再遍历背包，为什么 `j = weight[i]` 开始，而不是 j = 0？如果从 `j = 0` 开始，有什么问题？
- 你要更新 `dp[j] = max(dp[j], dp[j - weight[i]] + value[i])`
- 但是当 `j < weight[i]`，dp`[j - weight[i]]` 根本就 **越界**，非法访问。
```java
for (int i = 0; i < n; i++) {
    for (int j = weight[i]; j <= capacity; j++) {
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

### 完全背包遍历顺序 -- 组合 or 排列
#### 1️⃣ 先遍历物品 → 求组合数
```java
for (int i = 0; i < n; i++) {        // 遍历物品
    for (int j = weight[i]; j <= capacity; j++) { // 遍历容量（正序）
        dp[j] += dp[j - weight[i]];
    }
}
```
- **组合数** → 不考虑顺序。
- 外层固定一个物品，先处理完「把它放 0 次、1 次、2 次……」的所有情况。
- 这样，dp[j] 中 dp[j - weight[i]] 是用「之前物品 + 当前物品」合法组合推出来的。
- 不会因为顺序不同而重复计算。
#### 2️⃣ 先遍历背包 → 求排列数
```java
for (int j = 0; j <= capacity; j++) { // 遍历容量
    for (int i = 0; i < n; i++) {     // 遍历物品
        if (j >= weight[i]) {
            dp[j] += dp[j - weight[i]];
        }
    }
}
```
- **排列数** → 顺序敏感。
- 先在外层遍历容量，内层每次都尝试放每种物品。
- `dp[j]` 里 `dp[j - weight[i]]` 会「累加所有顺序情况」，比如：
    - 1+2 和 2+1 被算作两种不同路径。
- 所以是排列数。
#### ✅ 0-1 背包的经典问题都是组合类问题：
- 每个物品只能选 0/1 次 → 本质是在「选 or 不选」，没有「选的顺序」这个因素。
- 选出来的集合就是一个组合，不关心顺序。

| 题目类型        | 推荐写法 |
| ----------- | ---- |
| 完全背包（最大价值）  | 先物品  |
| 零钱兑换（求组合数）  | 先物品  |
| 零钱兑换（求排列数）  | 先背包  |
| 爬楼梯（有多少种爬法） | 先背包  |
### dp[0] 初始化难题
- **最优值 -- 「最小值 / 最大值」型题 → dp[0] 通常是 0**  
    → 比如 322，要的最少硬币数 → dp[0] = 0。

- **计数型 --「方案个数 / 有多少种方法」型题 → dp[0] 通常是 1**  
    → 比如 518，要的方案数 → 空方案是合法的 1 种 → dp[0] = 1。
### leetcode 518 coin changeⅡ(完全背包组合问题)
```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        for (int i = 0; i < coins.length; i++) {
            for (int j = coins[i]; j <= amount; j++) {
                dp[j] += dp[j - coins[i]];
            }
        }
        return dp[amount];
    }
}
```
### 回溯对比
518 题 → 组合问题 → **i = startIndex** 开始遍历
```java
                     []
              /      |       \
            1        2         3
          / | \    / \        \
        1  2  3   2  3        3
      / | \
    1  2  3

【重点】每一层 "i" 不回退 → 只会向后选物品
- 第 1 层 i=0 → 1/2/3
- 第 2 层 i=0 or i=1/2 → 只选当前及后面的物品
- 不会出现 [1,2] 和 [2,1] 分别算两种情况 → 顺序不敏感
```

```java
void backtrack(int sum, int startIndex) {
    if (sum == target) count++;
    if (sum > target) return;
    
    for (int i = startIndex; i < coins.length; i++) {
        backtrack(sum + coins[i], i);
    }
}
```

377 题 → 排列问题 → **i = 0** 每层都从 0 开始
```java
                     []
              /      |      \
            1        2       3
          / | \    / | \   / | \
        1  2  3   1  2  3 1  2  3
      / | \
    1  2  3

【重点】每一层都从 0 开始 → 任意顺序都合法
- [1,2] 和 [2,1] 算不同路径 → 顺序敏感
- 每一层物品都能重复选，且顺序决定不同路径
```

```java
void backtrack(int sum) {
    if (sum == target) count++;
    if (sum > target) return;

    for (int i = 0; i < nums.length; i++) {
        backtrack(sum + nums[i]);
    }
}
```

### leetcode377 combination sum IV（完全背包排列问题）
```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for (int j = 0; j <= target; j++) {
            for (int i = 0; i < nums.length; i++) {
                if (j >= nums[i])
                {dp[j] += dp[j - nums[i]];}
            }

        }
        return dp[target];
    }
}
```

### leetcode322 coin change 
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if (amount == 0)
            return 0;
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1); // 初始化为不可能大的值
        dp[0] = 0;
        for (int i = 0; i < coins.length; i++) {
            for (int j = coins[i]; j <= amount; j++) {
                dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);
            }
        }
        if (dp[amount] == amount + 1) {
            return -1;
        } else {
            return dp[amount];
        }

    }
}
```

### leetcode279perfect squares
```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, n + 1);
        dp[0] = 0;
        for (int i = 1; i * i <= n; i++) {
            for (int j = i * i; j <= n; j++) {
                dp[j] = Math.min(dp[j], dp[j - i * i] + 1);
            }
        }
        return (dp[n] == n + 1) ? 0 : dp[n];
    }
}
```

### leetcode139 word break 单词拆分
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {        
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int j = 1; j <= s.length(); j++) { // 排列题，先遍历背包，再遍历物品
            for (String word : wordDict) {
                int len = word.length();
                if (j >= len && s.substring(j - len, j).equals(word)) {
                    if (dp[j - len]) {
                        dp[j] = true;
                        break;
                    }
                }
            }
        }
        return dp[s.length()];
    }
}
```

**`dp[j]` 表示 `s[0..j-1]` 这一段是否可以被字典单词拆分出来。**
**注意：j 是长度，`s[0..j-1]` 是内容。**

1️⃣ `j >= len`：
👉 背包容量至少要 >= word 长度，才能考虑放这个单词（不能越界）。

2️⃣ `s.substring(j - len, j).equals(word)`：
👉 **看 `s[0..j-1]` 的末尾 len 个字符，是不是正好等于这个 word。**

**`s[j - len]` 到 `s[j-1]` 这一段**（一共 len 个字符）。
然后 `.equals(word)` → 判断是不是等于当前 word。

举例：
```java
s = "leetcode"
wordDict = ["leet", "code"]

假设 j = 4，word = "leet"

if (j >= 4 && s.substring(0, 4).equals("leet"))
→ s[0..3] == "leet"
→ 是 true！

然后就看 dp[0] 是否是 true，如果是，说明 s[0..3] 可以被拆。
```

- 如果 dp[j - len] == true，说明 **前面一段已经合法拆分**。
- 只要当前这个 word 又能放在末尾 → 整个 s[0..j-1] 也就合法拆分 → dp[j] = true。

### leetcode 198 打家劫舍
dp[i] - 下标为 i 的数字，包含 i 及之前的数字，所能偷得的最大值
```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 0) return 0;
        if (nums.length == 1) return nums[0]; 
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        for (int i = 2; i < nums.length; i++) {
            dp[i] = Math.max((dp[i - 2] + nums[i]), dp[i - 1]);
        }
        return dp[nums.length - 1];
    }
}
```

### leetcode 213 打家劫舍2

- 要么选 **第 1 间 ~ 第 n-1 间**（不选最后一间）
- 要么选 **第 2 间 ~ 第 n 间**（不选第一间）
这两个区间分别跑一遍 **198 题的 rob**，然后取 max。
！！仅仅是考虑的范围不同，并不是必须选各自区间的尾元素，选不选还是交给递推公式
#### 解法一
```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        if (nums.length == 2) return Math.max(nums[0], nums[1]);
        int[] dp1 = new int[nums.length];
        int[] dp2 = new int[nums.length];
        // [0, n - 2]
        dp1[0] = nums[0];
        dp1[1] = Math.max(nums[0], nums[1]);
        for (int i = 2; i < nums.length - 1; i++) {
            dp1[i] = Math.max(dp1[i - 2] + nums[i], dp1[i - 1]);
        }
        // [1, n - 1]
        dp2[1] = nums[1];
        dp2[2] = Math.max(nums[1], nums[2]);
        for (int j = 3; j < nums.length; j++) {
            dp2[j] = Math.max((dp2[j - 2] + nums[j]), dp2[j - 1]);
        }

        int res = Math.max(dp2[nums.length - 1], dp1[nums.length - 2]);
        return res;
    }
}
```

#### 解法二
滚动数组写法「省略了 dp 数组」，你心理上会觉得：
`- dp[i-2] → prev2`
`- dp[i-1] → prev1`
`- dp[i] → cur
```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        if (nums.length == 2) return Math.max(nums[0], nums[1]);

        // rob range [0, n-2]  (不选最后一个)
        int rob1 = robRange(nums, 0, nums.length - 2);

        // rob range [1, n-1]  (不选第一个)
        int rob2 = robRange(nums, 1, nums.length - 1);

        return Math.max(rob1, rob2);
    }

    private int robRange(int[] nums, int start, int end) {
        int prev2 = 0; // 存dp[i - 2]
        int prev1 = 0; // 存dp[i - 1]

        for (int i = start; i <= end; i++) {
            int cur = Math.max(prev1, prev2 + nums[i]); // 相当于 dp[i]
            prev2 = prev1; // 向前推进一位
            prev1 = cur; // 向前推进一位
        }

        return prev1;
    }
}
```
**下一轮循环开始时：**

- dp[i-2] → 变成了 dp[i-1] → 所以 prev2 = prev1
- dp[i-1] → 变成了 dp[i] → 所以 prev1 = cur

**注意理解：这里不是说 "dp[i-2] 等于 dp[i-1]"，而是说**：

👉 我们滑动窗口，"现在的 dp[i-1] **在下一轮要扮演 dp[i-2] 的角色**"。  
👉 "现在的 dp[i] **在下一轮要扮演 dp[i-1] 的角色**"。

### leetcode 740 delete and earn
```java
class Solution {
    public int deleteAndEarn(int[] nums) {
        int maxVal = 0;
        for (int num : nums) {
            maxVal = Math.max(maxVal, num);
        }
        int[] sum = new int[maxVal + 1];
        for (int num : nums) {
            sum[num] += num;
        }
        int[] dp = new int[maxVal + 1];
        dp[0] = sum[0];
        dp[1] = Math.max(sum[0], sum[1]);
        for (int i = 2; i < sum.length; i++) {
            dp[i] = Math.max(dp[i - 1], dp[i - 2] + sum[i]);
        }
        return dp[sum.length - 1]; 
    }
}
```

### leetcode337 打家劫舍3(树形dp)

![微信截图_20250612150735.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250612150735.png)

#### 1️⃣ 这题的特殊之处
前面的 198 / 213 / 740 都是「线性结构」：
- 线性结构 → `dp[i-1]`, `dp[i-2]` 依赖关系很清楚。

**这题是树！**
- 树 → 一个节点的「左右子树」是互相独立的，不能简单用 `dp[i-1]` 那种思想。
- 你需要的是「**当前节点状态下，左右子树能提供什么信息**」。
#### 2️⃣ 核心思路：每个节点有两个状态
|状态|含义|
|---|---|
|res[0] 不偷当前节点|当前节点不偷时，子树能提供的最大收益|
|res[1] 偷当前节点|当前节点偷时，子树能提供的最大收益|
👉 **也就是说，这两个数不是普通数组，是「当前节点对应的两个状态的值」**。
👉 本质是：**你要为父节点提供两个状态的信息**，这样父节点才能根据自己的选择（偷 or 不偷）自由组合左右子树的信息。

**所以一维数组长度=2，其实是因为你需要传两个状态值。**

#### 树形dp的难点
- 一开始学 DP → `dp[i]` → 单个状态。
- 学到树形 DP → 你要**学会「传状态上去」，而不是传最终答案。**

👉 树形 DP 最大难点 = **如何设计「递归函数返回值」**。

#### 代码
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    class Info {
        int rob; // 偷的最大收益
        int notRob; //不偷的最大收益

        public Info(int rob, int notRob) { // 用有参构造 → 传入两个状态值
            this.rob = rob;
            this.notRob = notRob;
        }
    }

    public int rob(TreeNode root) {
        Info res = dfs(root);
        return Math.max(res.rob, res.notRob);
    }

    private Info dfs(TreeNode node) {//后序遍历
        if (node == null) return new Info(0, 0);
        Info left = dfs(node.left);
        Info right = dfs(node.right);

        int rob = node.val + left.notRob + right.notRob;
        int notRob = Math.max(left.notRob, left.rob) +  Math.max(right.notRob, right.rob);
        return new Info(rob, notRob);
    }
}
```

### leetcode [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)(树形dp)

![微信截图_20250613103558.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250613103558.png)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int diameter = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        dfs(root);
        return diameter;
    }

    private int dfs(TreeNode node) {
        if (node == null) return 0;

        int left = dfs(node.left); // 左子树最大深度
        int right = dfs(node.right); // 右子树最大深度

        diameter = Math.max(diameter, left + right);// 更新全局直径

        return Math.max(left, right) + 1; // 关键：return当前节点的最大深度
    }
}
```

### dfs(node)究竟代表什么

**核心原因：dfs(node) 的「定义」是你自己设计的！**
**dfs 本质是什么？**
👉 dfs(node) 本质上是「在递归过程中返回我希望子树告诉我的状态」。
    - 你在设计 dfs 的 return 值的时候，是根据题目需求定义的。
    - 不同题目对「子树状态」的需求不同，所以 return 值的含义也不同 → **递归框架一样，语义不同**！

| 题号  | dfs(node) 的定义     | left/right 含义    |
| --- | ----------------- | ---------------- |
| 543 | 以当前节点为根的最大深度（边数）  | 左 / 右子树最大深度      |
| 124 | 以当前节点为根的「最大单边贡献和」 | 左 / 右子树最大「单边贡献和」 |
```java
dfs(node):
    left = dfs(node.left)
    right = dfs(node.right)
    // 处理当前节点
    return 某个值
```
### leetcode [124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)（树形dp）
1. 在当前节点拐弯的路径和 = 左子树最大链和 + 右子树最大链和 + 当前节点值
2. 返回给父节点的是 max (左子树最大链和，右子树最大链和) + 当前节点值。如果这个值是负数，则返回0。
    👉 为什么？ → **父节点希望知道「单边最大贡献」**，因为 **不能连续穿过左右两边**。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int pathSum = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        dfs(root);
        return pathSum;
    }

    private int dfs(TreeNode node) {
        if (node == null) return 0;
        int left = dfs(node.left);
        int right = dfs(node.right);
        pathSum = Math.max(pathSum, left + right + node.val);// 先更新全局答案（左右都走）
        return Math.max(Math.max(left, right) + node.val, 0);// 再 return 单边贡献（左右只能走一边）
    }
}
```

### leetcode [2246. Longest Path With Different Adjacent Characters](https://leetcode.com/problems/longest-path-with-different-adjacent-characters/)(树形dp)

对于一般的树（非二叉树），怎么求出最长路径？
- 这题是 **任意节点可以作为「中心节点」**！
    - **不一定是根节点**，可能是树上任何一个节点作为中心，拼出来的路径最长。  需要dfs 整棵树，**每个节点都会尝试更新 ans。**
- 对于树上的每一个节点 x，计算：
```java
合法路径长度 = 左子树最大合法链 + 右子树最大合法链 + 1（当前节点）
```
![微信截图_20250613151038.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250613151038.png)
**Input:** parent = [-1,0,0,1,1,2], s = "abacbe"
#### 1. 建树
**题目没给 TreeNode**，只给 parent[] → 你需要自己建树 → **所谓「建树」本质上就是 → 把「父子关系」整理成「父节点 → 子节点」的映射。**
- 你要自己建「邻接表」g。
- List`<Integer>[]` 本质上就是一个「邻接表」 → 经典邻接表建树/建图的数据结构。

```java
List<Integer>[] g = new ArrayList[n];
for (int i = 0; i < n; i++) {
    g[i] = new ArrayList<>();
}
```
得到一个空的邻接表：
`g = [[], [], [], [], [], []]`

```java
for (int i = 1; i < n; i++) {
    g[parent[i]].add(i);
}
```
**把 parent[] 里「父子关系」映射到邻接表 `g[]` 里。`g[i] `里存的是「节点 i 的所有子节点」。**
得到`g = [[1, 2], [3, 4], [5], [], [], []]`

```java
g[0] → [1, 2] → 0 的孩子是 1 和 2
g[1] → [3, 4] → 1 的孩子是 3 和 4
g[2] → [5]    → 2 的孩子是 5
g[3] → []     → 3 是叶子
g[4] → []     → 4 是叶子
g[5] → []     → 5 是叶子
```
#### 2.递归
```java
对于当前节点 x：
    我遍历所有子节点 y：
        dfs(y) → 返回「以 y 为起点，单边最长合法链」

    我维护：
        max1 → 当前最大的合法链长度
        max2 → 当前次大的合法链长度

    更新全局答案：
        res = max(res, max1 + max2 + 1)

    return max1 → 返回单边贡献，供父节点用
```

```java
class Solution {
    int res;
    List<Integer>[] g;
    char[] chs;
    public int longestPath(int[] parent, String s) {
        int n = parent.length;
        g = new ArrayList[n];
        for (int i = 0; i < n; i++) {
            g[i] = new ArrayList<>();
        }
        for (int i = 1; i < n; i++) {
            g[parent[i]].add(i);
        }
        chs = s.toCharArray();
        dfs(0);
        return res;
    }

    private int dfs(int x) {
        int max1 = 0, max2 = 0;
        for (int y : g[x]) {
            int len = dfs(y) + 1;
            if (chs[y] != chs[x]) {
                if (len > max1) {
                    max2 = max1;
                    max1 = len;
                } else if (len > max2) {
                    max2 = len;
                }
            } 
        }
        res = Math.max(res, max2 + max1 + 1);
        return max1;
    }
}
```

#### 为什么是`List<Integer>[]`
```java
List<Integer>[] g = new ArrayList[n];
```
- 这个时候 **只是创建了一个「长度为 n 的数组」**，数组里的元素初始值都是 `null`。
- 因为 `List<Integer>` 是对象引用，数组里初始存的是 null。
- 所以你要手动给每个位置赋一个 ArrayList

为什么不能是：
```java
List<Integer>[] g = new List<Integer>[n];
```
- Java 的泛型是 **伪泛型**，在运行时「泛型信息会被擦除」，也就是说：`List<Integer>[]` → 在运行时其实只是 `List[]`，不会带 Integer 类型信息。
- java不允许new一个泛型数组。由于泛型在运行时擦除成原始类型，Java 不能保证你不会「插入错误类型」 → 所以直接禁止你 new 泛型数组。
- ArrayList 是 **具体类**，没有泛型擦除的问题。 new 的其实是「ArrayList 类型的数组」，只是数组里存的是 Object → ArrayList 的引用。 **不会有泛型安全问题**，所以 Java 允许你这么 new。

### leetcode687 [Longest Univalue Path](https://leetcode.com/problems/longest-univalue-path/)(树形dp)
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int maxVal = 0;
    public int longestUnivaluePath(TreeNode root) {
        dfs(root);
        return maxVal;
    }

    private int dfs(TreeNode node) {
        if (node == null) return 0;
        int left = dfs(node.left); // 左边能找到的最多相同值链
        int right = dfs(node.right); // 右边能找到的最多相同值链
        if (node.left != null && node.left.val != node.val) left = 0;
        if (node.right != null && node.right.val != node.val) right = 0;
        maxVal = Math.max(maxVal, left + right);
        return Math.max(left, right) + 1;
    }
}
```

### leetcode121 买卖股票1
`dp[i][1]` **在第 i 天结束后，手里**「持有股票」**的情况下，所能获得的最大利润（从开始到现在）。**
- 第`i` 天，保持昨天持有的股票： `dp[i][1] = dp[i - 1][1]`
    - 你并没有「卖出」，所以股票的市场价值变化对你没有实际影响（你还没变现）
    - 而你之前是否买入（以及什么时候买的）已经在 `dp[i-1][1]` 中记录了最优解
- 第`i` 天买入`dp[i][1] = - prices[i]`（重新开始，只能买一次）

所以`dp[i][1] = max(dp[i - 1][1], - prices[i])`

`dp[i][0]` 第`i`天不持有的最大利润(包括该天卖出或之前就没买)
`dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i]); // 卖出 or 什么都不做`

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][] dp = new int[n][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];

        for (int i = 1; i < n; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], -prices[i]);
        }

        return dp[n - 1][0]; // 最后一天未持股，收益最大
    }
}
```
- 我们只允许交易一次，所以最终持有股票（`dp[n-1][1]`）是「买了但没卖」，不是最大利润
- 必须是「已经卖出」，也就是 `dp[n-1][0]` 才能作为结果

### leetcode122 买卖股票2(无限次交易)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][] dp = new int[n][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];

        for (int i = 1; i < n; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }

        return dp[n - 1][0];
    }
}
```

#### 滚动数组优化解法
```java
int hold = -prices[0], noHold = 0;

for (int i = 1; i < prices.length; i++) {
    hold = Math.max(hold, noHold - prices[i]);     // 买入
    noHold = Math.max(noHold, hold + prices[i]);   // 卖出
}
```

### leetcode123 买卖股票3(至多2次交易)
#### 三维数组方法
- `i`：第几天 → 遍历天数 → `for (int i = 1; i < n; i++)`
- `k`：最多允许交易次数（0、1、2）→ 遍历交易阶段 → `for (int k = 1; k <= 2; k++)`
- `0/1`：是否持股 → 不需要写 `for`，我们在转移时**显式写两条语句**更新 `[k][0]` 和 `[k][1]`

```java
class Solution {
    public int maxProfit(int[] prices) {
        int[][][] dp = new int[prices.length][3][2];
        dp[0][0][0] = 0; // 0次交易，没持股
        dp[0][1][0] = 0; // 1次交易，没持股
        dp[0][2][0] = 0; // 2次交易，没持股
        dp[0][0][1] = -prices[0]; // 不合法（0次交易不能持股，但可以初始化为 -prices[0] 作为备份）
        dp[0][1][1] = -prices[0]; // 买入第一笔
        dp[0][2][1] = -prices[0]; // 买入第二笔（在真实过程中不会直接发生，但初始化为 -prices[0] 有利于统一）
        for (int i = 1; i < prices.length; i++) {
            for (int k = 1; k <= 2; k++) {
                dp[i][k][0] = Math.max(dp[i - 1][k][0], dp[i - 1][k][1] + prices[i]);
                dp[i][k][1] = Math.max(dp[i - 1][k][1], dp[i - 1][k - 1][0] - prices[i]);
            }
        }
        return dp[prices.length - 1][2][0];
    }
}
```
#### 滚动数组优化版本
| 变量名     | 含义                          |
| ------- | --------------------------- |
| `dp[0]` | 第一次买入后的最大利润（买入后手中持有股票）      |
| `dp[1]` | 第一次卖出后的最大利润（第一次交易完成，不持股）    |
| `dp[2]` | 第二次买入后的最大利润（第二次开始，重新持股）     |
| `dp[3]` | 第二次卖出后的最大利润（全部完成，不持股 ✅最终结果） |
```java
class Solution {
    public int maxProfit(int[] prices) {
        int[] dp = new int[4]; 
        // 存储两次交易的状态就行了
        // dp[0]代表第一次交易的买入
        dp[0] = -prices[0];
        // dp[1]代表第一次交易的卖出
        dp[1] = 0;
        // dp[2]代表第二次交易的买入
        dp[2] = -prices[0];
        // dp[3]代表第二次交易的卖出
        dp[3] = 0;
        for(int i = 1; i <= prices.length; i++){
            // 要么保持不变，要么没有就买，有了就卖
            dp[0] = Math.max(dp[0], -prices[i-1]);
            dp[1] = Math.max(dp[1], dp[0]+prices[i-1]);
            // 这已经是第二次交易了，所以得加上前一次交易卖出去的收获
            dp[2] = Math.max(dp[2], dp[1]-prices[i-1]);
            dp[3] = Math.max(dp[3], dp[2]+ prices[i-1]);
        }
        return dp[3];
    }
}
```

### leetcode188 买卖股票4(至多k次交易)
#### k >= n / 2的边界问题
`prices.length = n`
- 一共 `n` 天，可以最多交易的天数 = `n`
- 每次交易至少需要 **2 天**（买入1天，卖出1天）
- 所以你最多能做的完整交易数量 = `⌊n / 2⌋`（地板除）

举例 `n = 6` 天，最多交易次数 = `3` 次：

| 交易编号 | 买入天   | 卖出天   |
| ---- | ----- | ----- |
| 第一次  | 第 0 天 | 第 1 天 |
| 第二次  | 第 2 天 | 第 3 天 |
| 第三次  | 第 4 天 | 第 5 天 |
如果题目允许你最多交易 `k` 次，而你实际最多能做的交易次数也就是 `⌊n / 2⌋`，那么：

> 当 `k >= n / 2`，你根本**不可能用得完那么多次交易名额**，就等于没有交易次数限制！
> ✅ 所以当 `k >= n / 2`，就可以退化为 Leetcode 122（无限次买卖）
#### 滚动数组优化解法

```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        int n = prices.length;
        int sum = 0;
        if (k >= n / 2) {
            for (int i = 1; i < prices.length; i++) {
                if (prices[i] > prices[i - 1]) {
                    sum += prices[i] - prices[i - 1];
                }
            }
            return sum;
        }

        int[][] dp = new int[k + 1][2];
        for (int j = 0; j <= k; j++) {
            dp[j][0] = 0; // 第j次交易后，不持股
            dp[j][1] = -prices[0]; // 第j次交易后，持股（假设今天买入）
        }

        for (int i = 1; i < n; i++) {
            for (int j = 1; j <= k; j++) {
                //第 j 次交易后不持股的最大利润
                dp[j][0] = Math.max(dp[j][0], dp[j][1] + prices[i]);
                //第 j 次交易后持股的最大利润
                dp[j][1] = Math.max(dp[j][1], dp[j - 1][0] - prices[i]);
            }
        }
        return dp[k][0];
    }
}
```
> 买入操作是新的一笔交易的开始，必须以 `j - 1` 次交易完成后的「不持股状态」为基础，否则就相当于「超次数买入」了。

### leetcode [309. Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)
```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        if (n <= 1) return 0;
        int[][] dp = new int[n][2];

        // 初始化
        dp[0][0] = 0;
        dp[0][1] = -prices[0];

        dp[1][0] = Math.max(0, prices[1] - prices[0]);
        dp[1][1] = Math.max(-prices[0], -prices[1]);

        for (int i = 2; i < n; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);       // 卖出 or 不动
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 2][0] - prices[i]);       // 持股 or 冷却后买入
        }

        return dp[n - 1][0];  // 最后一天不持股，最大利润
    }
}
```

### leetcode[714. Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)
```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int n = prices.length;
        int[][] dp = new int[n][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];

        for (int i = 1; i < n; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i] - fee);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }

        return dp[n - 1][0];
    }
}
```

### leetcode300 [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)
以 `nums[i]` 结尾的**最长递增子序列的长度**。
对于每个位置 `i`，我们可以往前看所有 `j`（`j < i`）：
- 如果 `nums[j] < nums[i]`，说明可以把 `nums[i]` 接在 `nums[j]` 的后面形成递增序列；
- 此时可以更新 `dp[i] = max(dp[i], dp[j] + 1)`

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        Arrays.fill(dp, 1);
        for (int i = 1; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dp[i] = Math.max(dp[j] + 1, dp[i]);
                }
            }
        }
        int maxLen = 0;
        for (int len : dp) {
            maxLen = Math.max(maxLen, len);
        }
        return maxLen;
    }
}
```

### leetcode[674. Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/)
```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int[] dp = new int[nums.length];
        Arrays.fill(dp, 1);
        for (int i = 1; i < nums.length; i++) {
            if (nums[i - 1] < nums[i]) {
                dp[i] = Math.max(dp[i - 1] + 1, dp[i]);
            }
        }
        int maxLen = 0;
        for (int len : dp) {
            maxLen = Math.max(maxLen, len);
        }
        return maxLen;
        
    }
}
```

### leetcode718 [Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)
Given two integer arrays `nums1` and `nums2`, return _the maximum length of a subarray that appears in **both** arrays_.

**Example 1:**

**Input:** nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
**Output:** 3
**Explanation:** The repeated subarray with maximum length is [3,2,1].

**Example 2:**

**Input:** nums1 = [0,0,0,0,0], nums2 = [0,0,0,0,0]
**Output:** 5
**Explanation:** The repeated subarray with maximum length is [0,0,0,0,0].

`dp[i][j]` 定义为：
> "**以 `nums1[i-1]` 和 `nums2[j-1]` 结尾**的最长公共子数组的长度"


```java
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int res = 0;
        int[][] dp = new int[nums1.length + 1][nums2.length + 1];
        for (int i = 1; i < nums1.length + 1; i++) {
            for (int j = 1; j < nums2.length + 1; j++) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                    res = Math.max(res, dp[i][j]);
                } else {
                  dp[i][j] = 0; //可以不写
                }
            }
        }
        return res;
    }
}
```
举例
```java
nums1 = [0, 1, 1]
nums2 = [1, 0, 1]
```

![061701.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/061701.PNG)



### `dp[i][j]`**以 `nums1[i-1]` 和 `nums2[j-1]` 结尾**的最长公共子数组的长度

| 边界位置               | 典型错误                  | 正确做法                                                 |
| ------------------ | --------------------- | ---------------------------------------------------- |
| `dp[0][0]`         | 不小心访问 `dp[-1][-1]`    | 定义 `dp[i][j]` 为以 `nums1[i-1]` 和 `nums2[j-1]` 结尾，避免越界 |
| `i == 0 or j == 0` | 需要特殊处理第一行/列           | **多开一行一列**，自动用 0 初始化                                 |
| `for` 循环边界         | 写成 `i < nums1.length` | 写成 `i <= nums1.length`，从 `1` 开始，正好配合 `i-1` 的取值       |
### leetcode1143 [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)
经典的“字符串类动态规划”题中总结出的套路，比如：

- 最长公共子序列（LCS）
- 编辑距离（Edit Distance）
- 最长回文子序列
- 最长公共子串

这些题都有一个共同点：**两个字符串之间的匹配关系，可以用二维表格（状态空间）表示**，于是你自然会去思考：

> 如果 text1 的前 i 个字符 vs text2 的前 j 个字符，最优解怎么构建？这时你自然就会想到“**从更小的问题状态转移而来**”。

> `dp[i][j] = text1[0..i-1] 与 text2[0..j-1] 的最长公共子序列长度`

> 假设你已经知道 `dp[i][j]`，然后问自己：如果我不选当前字符（因为它们不等），我该看哪两个子问题？

答案显然是：

- 不选 `text1[i - 1]` → 看 `dp[i - 1][j]`
- 不选 `text2[j - 1]` → 看 `dp[i][j - 1]`

于是你就必须比较这两个解法，取更优的那个：
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int[][] dp = new int[text1.length() + 1][text2.length() + 1];
        for (int i = 1; i < text1.length() + 1; i++) {
            for (int j = 1; j < text2.length() + 1; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[text1.length()][text2.length()];
    }
}
```

### leetcode [1035. Uncrossed Lines](https://leetcode.com/problems/uncrossed-lines/)
转化成求最长公共子序列的问题
```java
class Solution {
    public int maxUncrossedLines(int[] nums1, int[] nums2) {
        int[][] dp = new int[nums1.length + 1][nums2.length + 1];
        for (int i = 1; i < nums1.length + 1; i++) {
            for (int j = 1; j < nums2.length + 1; j++) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        }
        return dp[nums1.length][nums2.length];
        
    }
}
```

### leetcode53 [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
从贪心算法看
```java
if (sum < 0) sum = 0;
sum += nums[i];
result = Math.max(result, sum);
```
逻辑非常直觉：“如果当前累计和已经是负数，那我就不要它了，从当前这个数重新开始。”

但是在dp做法里，比的不是“这个数是不是正数”，而是：当前这个数是自己单干更好，还是继续延续前面的更好？
举例：
```java
nums = [5, -2, 4]
```
结论：虽然 `-2` 是负数，但它 **不能扔**，因为它是“通往更大总和的中间一块踏板”。

`dp[i]` 代表以 `nums[i]` 结尾的最大子数组和，在最长公共子串 / 子序列，为了让 `dp[0][*]` 和 `dp[*][0]` 表示空字符串，防止越界，代码统一。在本题不涉及二维转移或越界问题，用本身下标更直观。
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        int res = dp[0];

        for (int i = 1; i < nums.length; i++) {
            dp[i] = Math.max(dp[i - 1] + nums[i], nums[i]);
            res = Math.max(res, dp[i]);
        }

        return res;
    }
}

```

### [392. Is Subsequence](https://leetcode.com/problems/is-subsequence/)
```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int[][] dp = new int[s.length() + 1][t.length() + 1];

        for (int i = 1; i < s.length() + 1; i++) {
            for (int j = 1; j < t.length() + 1; j++) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = dp[i][j - 1];
                }
            }
        }
        return dp[s.length()][t.length()] == s.length();
    }
}
```

`dp[i][j]`代表`nums[i -1]nums[j -1]`为结尾的最大公共子序列长度
else分支
- **我们在匹配 `s` 是否是 `t` 的子序列**，不是反过来；
- 所以必须是**“允许跳过 t 的字符，但不能跳过 s 的字符”**；
- 而这个逻辑在代码里就体现在：
    - ✅ `dp[i][j] = dp[i][j - 1];` → 跳过 t 的字符（合法）
    - ❌ `dp[i][j] = dp[i - 1][j - 1];` → 等于跳过 s 和 t 的字符（会错过匹配机会）

### [115. Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)
#### 题意转化：
从 `s` 中删除一些字符（不改变顺序），形成 `t`。问一共多少种不同的删除方式能使得 `s` 的子序列等于 `t`。
#### 定义：
设 `dp[i][j]` 表示： 从 `s[0...i-1]` 中形成 `t[0...j-1]` 的 **不同子序列的数量**。
#### 转移方程：
每次面对 `s[i-1]` 和 `t[j-1]`：
✅ **相等** → 两条路（选 or 不选）
1.**使用**`s[i-1]`字符来匹配 `t[j-1]`,那前面的匹配就必须是：
```java
s[0..i-2] 和 t[0..j-2] 的方案数
⇒ dp[i-1][j-1]
```
2.不用这个字符，那就把它跳过
```java
s[0..i-2] 和 t[0..j-1] 的方案数
⇒ dp[i-1][j]
```
总方案数：
```java
dp[i][j] = dp[i-1][j-1] + dp[i-1][j]
```
❌ **不等** → 只能跳过 → `dp[i-1][j]`
只能从：
```java
s[0..i-2] 构造 t[0..j-1]
⇒ dp[i-1][j]
```
#### 初始化
空串 s 可以匹配空串 t，但无法匹配任何非空串；而任何串都能通过“全跳过”来匹配空串
```java
dp[0][0] = 1;
dp[i][0] = 1;
dp[0][j] = 0;
```

```java
class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length();
        int n = t.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 0; i < m + 1; i++) {
            dp[i][0] = 1;
        }
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if(s.charAt(i - 1) == t.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[m][n];
    }
}
```

### [583. Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/)
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int same = 0;
        int m = word1.length();
        int n = word2.length();
        int[][] dp = new int[m + 1][n + 1];
        int res = 0;

        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        res = m + n - 2 * dp[m][n];
        return res;
    }
}
```

### [72. Edit Distance](https://leetcode.com/problems/edit-distance/)
```java
class Solution {
    public int minDistance(String text1, String text2) {
        int m = text1.length();
        int n = text2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 0; i < m + 1; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j < n + 1; j++) {
            dp[0][j] = j;
        }
        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.min(Math.min(dp[i - 1][j] + 1, dp[i][j - 1] + 1), dp[i - 1][j - 1] + 1);
                }
            }
        }

        return dp[m][n];
    }
}
```

### [647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)
Given a string `s`, return _the number of **palindromic substrings** in it_. A string is a **palindrome** when it reads the same backward as forward. A **substring** is a contiguous sequence of characters within the string.

**Example 1:**
**Input:** s = "abc"
**Output:** 3
**Explanation:** Three palindromic strings: "a", "b", "c".

**Example 2:**
**Input:** s = "aaa"
**Output:** 6
**Explanation:** Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".

✅ 什么是 **substring**（子字符串）

> A **substring** is a contiguous (连续的) sequence of characters **within** a string.

换句话说：
- 必须是 **连续的一段字符**
- 必须来自原字符串
- 顺序不能乱，不能跳

以`aaa`为例：

|子串内容|起止位置（从 0 开始）|
|---|---|
|`"a"`|0|
|`"a"`|1|
|`"a"`|2|
|`"aa"`|0-1|
|`"aa"`|1-2|
|`"aaa"`|0-2|
#### dp解法
```java
class Solution {
    public int countSubstrings(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        int count = 0;

        for (int i = n - 1; i >= 0; i--) { // 从后往前填表
            for (int j = i; j < n; j++) {
                if (s.charAt(i) == s.charAt(j)) {
                    if (j - i <= 2 || dp[i + 1][j - 1]) {
                        dp[i][j] = true;
                        count++;
                    }
                }
            }
        }

        return count;
    }
}
```

#### 双指针法
尽可能枚举所有的回文中心

1.  **一个字符串 `"abc"` 有哪些可能的回文中心？**
字符串 `"a b c"` 有 `3` 个字符，但能构成的**回文中心**有：
2. 单字符中心（字符上）→ 构造奇数回文串：
    - `"a"` （中心点是 index 0）
    - `"b"` （index 1）
    - `"c"` （index 2）
3. 双字符中心（两个字符之间）→ 构造偶数回文串：
    - `"ab"` 中间
    - `"bc"` 中间

👉 总共有 `2n - 1` 个中心点，可以将所有的中心编号为`center = 0 到 2n - 2`

2. **那 left/right 是怎么得到的？**
用 `center` 来生成所有可能的 `(left, right)` 起点，用于向两边扩展。**我们把字符之间的所有“缝”也当成中心来枚举**，然后用 `center / 2` 找到左边字符，`% 2` 判断是奇还是偶中心
推导：

✅ 偶数中心（比如 center = 2）
- center = 2
- `left = 2 / 2 = 1`
- `right = 1 + 2 % 2 = 1`
→ 中心是字符 `"b"`，即 `(1,1)`，用于扩展 `"aba"` 这样的奇数回文 ✅

✅ 奇数中心（比如 center = 3）
- center = 3
- `left = 3 / 2 = 1`
- `right = 1 + 1 = 2`
→ 中心是 `"b"` 与 `"c"` 之间 → 可以扩展出 `"bc"` 这样的偶数回文 ✅

```java
class Solution {
    public int countSubstrings(String s) {
        int count = 0;

        for (int center = 0; center < 2 * s.length() - 1; center++) {
            int left = center / 2;
            int right = left + center % 2;

            while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
                count++;
                left--;
                right++;
            }
        }

        return count;
    }
}
```

### [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)
Given a string `s`, return _the longest_ _palindromic_ _substring_ in `s`.

**Example 1:**
**Input:** s = "babad"
**Output:** "bab"
**Explanation:** "aba" is also a valid answer.

**Example 2:**
**Input:** s = "cbbd"
**Output:** "bb"

**Constraints:**
- `1 <= s.length <= 1000`
- `s` consist of only digits and English letters.

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        if (n <= 1) return s;

        boolean[][] dp = new boolean[n][n];
        int start = 0, maxLen = 1;

        for (int i = n - 1; i >= 0; i--) { // 从后往前填表
            for (int j = i; j < n; j++) {
                if (s.charAt(i) == s.charAt(j)) {
                    if (j - i <= 2 || dp[i + 1][j - 1]) {
                        dp[i][j] = true;
                        if (j - i + 1 > maxLen) {
                            maxLen = j - i + 1;
                            start = i;
                        }
                    }
                }
            }
        }
        return s.substring(start, start + maxLen);
    }
}
```

### [516. Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)
Given a string `s`, find _the longest palindromic **subsequence**'s length in_ `s`. A **subsequence** is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**
**Input:** s = "bbbab"
**Output:** 4
**Explanation:** One possible longest palindromic subsequence is "bbbb".

**Example 2:**
**Input:** s = "cbbd"
**Output:** 2
**Explanation:** One possible longest palindromic subsequence is "bb".

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        int[][] dp = new int[n][n];
        for (int i = 0; i < n; i++) {
            dp[i][i] = 1;
        }
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                if(s.charAt(i) == s.charAt(j)) {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                } else {
                    dp[i][j] = Math.max(dp[i][j - 1], dp[i + 1][j]);
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

## 3.3单调栈 monotonic stack

### 单调栈适合什么样的题目
1. 找左边第一个小于/大于某值的位置
2. 找右边第一个小于/大于某值的位置
3. 利用这个边界扩展区间做：面积、组合数、贡献统计
### 单调栈数据结构
`Deque<Integer> stack = new ArrayDeque<>();`
- **Deque** 是双端队列，支持 `push` / `pop` / `peek` 等栈操作。
- **ArrayDeque** 比 `Stack` 类更高效（因为 `Stack` 基于 `Vector`，线程安全但慢）。
### 快速模版
十六字箴言
***及时去掉无用数据，保证栈内数据有序***

```java
// 单调递增栈（找最近小于）
while (!stack.isEmpty() && arr[stack.peek()] >= arr[i]) {
    stack.pop();
}

// 单调递减栈（找最近大于）
while (!stack.isEmpty() && arr[stack.peek()] <= arr[i]) {
    stack.pop();
}
```

### 739 每日温度
![061802.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/061802.PNG)

- 单调栈里存的是下标
- 如果遍历的元素比栈顶元素大，更新此时遍历元素下标与栈顶元素下标的差值，然后栈弹出栈顶元素，如果遍历元素还比栈顶元素大，就一直弹出栈内元素；如果遍历元素比栈顶元素小或者等于就入栈，这样栈内的元素始终是单调递增的

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        Deque<Integer> st = new ArrayDeque<>();
        int n = temperatures.length;
        st.push(0);
        int[] res = new int[n];
        for (int i = 1; i < n; i++) {
            while (!st.isEmpty() && temperatures[i] > temperatures[st.peek()]) {
                    int prevIndex = st.pop();
                    res[prevIndex] = i - prevIndex;
            }
            st.push(i);
        }
        return res;
    }
}
```
不明白为什么用while，用[73,71,70,74]测试

### [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)
The **next greater element** of some element `x` in an array is the **first greater** element that is **to the right** of `x` in the same array.

You are given two **distinct 0-indexed** integer arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

For each `0 <= i < nums1.length`, find the index `j` such that `nums1[i] == nums2[j]` and determine the **next greater element** of `nums2[j]` in `nums2`. If there is no next greater element, then the answer for this query is `-1`.

Return _an array_ `ans` _of length_ `nums1.length` _such that_ `ans[i]` _is the **next greater element** as described above._

**Example 1:**

**Input:** nums1 = [4,1,2], nums2 = [1,3,4,2]
**Output:** [-1,3,-1]
**Explanation:** The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.

**Example 2:**

**Input:** nums1 = [2,4], nums2 = [1,2,3,4]
**Output:** [3,-1]
**Explanation:** The next greater element for each value of nums1 is as follows:
- 2 is underlined in nums2 = [1,2,3,4]. The next greater element is 3.
- 4 is underlined in nums2 = [1,2,3,4]. There is no next greater element, so the answer is -1.

**Constraints:**

- `1 <= nums1.length <= nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 10^4`
- All integers in `nums1` and `nums2` are **unique**.
- All the integers of `nums1` also appear in `nums2`.

#### 解题步骤：
1. 「下一个更大 / 更小元素」就应该立刻联想到 **单调栈**，这是算法刷题中很经典的一类题
2. nums1 是 nums2 的子集，查找效率要高 → HashMap
    - 我们已经能处理 `nums2` 的下一个更大元素了
     - 但 `nums1` 是要从 `nums2` 中「按值查找答案」
    此时我们需要一个「值 → 答案」的映射关系  → 最自然的工具就是 `HashMap`，因为它可以做到 O(1) 查询
```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int[] result = new int[m];
        Arrays.fill(result, -1);
        HashMap<Integer, Integer> map = new HashMap<>();
        Deque<Integer> stack = new ArrayDeque<>();
        for (int num2 : nums2) {
            while(!stack.isEmpty() && num2 > stack.peek()) {
                int smaller = stack.pop();
                map.put(smaller, num2);
            }
            stack.push(num2);
        }

        while(!stack.isEmpty()) {
            int remain = stack.pop();
            map.put(remain, -1);
        }

        for (int i = 0; i < nums1.length; i++) {
            result[i] = map.get(nums1[i]);
        }
        return result;
    }
}
```

### [503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)（环形元素）
环形题的解法 取模
```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        Arrays.fill(result, -1);
        Deque<Integer> stack = new ArrayDeque<>();
        for (int i = 0; i < 2 * n; i++) {
            while(!stack.isEmpty() && nums[i % n] > nums[stack.peek()]) {
                int prevIndex = stack.pop();
                result[prevIndex] = nums[i % n];
            }
            stack.push(i % n);
        }
        return result;
    }
}
```

### 42接雨水

#### 单调栈法
![061803.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/061803.PNG)

```java
class Solution {
    public int trap(int[] height) {
        Deque<Integer> stack = new ArrayDeque<>();
        int result = 0;
        int n = height.length;
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && height[i] >= height[stack.peek()]) {
                int bottomH = height[stack.pop()];
                if (stack.isEmpty())
                    break;
                int left = stack.peek();
                int w = i - left - 1;
                int h = Math.min(height[left], height[i]) - bottomH;
                result += w * h;
            }
            stack.push(i);
        }
        return result;
    }
}
```

#### 前后缀法
![061901.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/061901.PNG)

```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        if (n == 1) return 0;
        int[] preMax = new int[n];
        int[] sufMax = new int[n];
        preMax[0] = height[0];
        for (int i = 1; i < n; i++) {
            preMax[i] = Math.max(preMax[i - 1], height[i]);
        }
        sufMax[n - 1] = height[n - 1];
        for (int j = n - 2; j >= 0; j--) {
            sufMax[j] = Math.max(sufMax[j + 1], height[j]);
        }
        int area = 0;
        for (int a = 0; a < n; a++) {
            area += Math.min(preMax[a], sufMax[a]) - height[a];
        }
        return area;
    }
}
```
#### 前后缀优化法--相向双指针
> 谁短算谁的水，短边决定当前能装多少。
### [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
#### 双指针法
```java
class Solution {
    public int maxArea(int[] height) {
        int area = 0;
        int left = 0;
        int right = height.length - 1;
        while (left < right) {
            area = Math.max(area, (right - left) * Math.min(height[right], height[left]));
            if (height[left] > height[right]) {
                right--;
            } else {
                left++;
            }
        }
        return area;
    }
}
```

### [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

![1710804602-sheXGS-lc84.jpg](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1710804602-sheXGS-lc84.jpg)

首先，面积最大矩形的高度一定是 heights 中的元素。这可以用反证法证明：假如高度不在 heights 中，比如 4，那我们可以增加高度直到触及某根柱子的顶部，比如增加到 5，由于矩形底边长不变，高度增加，我们得到了面积更大的矩形，矛盾，所以面积最大矩形的高度一定是 heights 中的元素。

假设 h=heights[i] 是矩形的高度，那么矩形的宽度最大是多少？我们需要知道：

    在 i 左侧的小于 h 的最近元素的下标 left，如果不存在则为 −1。求出了 left，那么 left + 1 就是矩形最左边那根柱子。
    在 i 右侧的小于 h 的最近元素的下标 right，如果不存在则为 n。求出了 right，那么 right − 1 就是矩形最右边那根柱子。

比如示例 1（上图），选择 i=2 这个柱子作为矩形高，那么左边小于 heights[2]=5 的最近元素的下标为 left=1，右边小于 heights[2]=5 的最近元素的下标为 right=4。矩形的宽度就是 right−left−1=4−1−1=2，矩形面积为 h⋅(right−left−1)=5⋅2=10。

枚举 i，计算对应的矩形面积，更新答案的最大值。

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Stack<Integer> st = new Stack<>();

        // 1. 求左边第一个小于当前柱子的下标
        for (int i = 0; i < n; i++) {
            while (!st.isEmpty() && heights[st.peek()] >= heights[i]) {
                st.pop();
            }
            // 如果栈空，说明左边没有比它小的
            left[i] = st.isEmpty() ? -1 : st.peek();
            st.push(i);
        }

        // 清空栈，重用
        st.clear();

        // 2. 求右边第一个小于当前柱子的下标
        for (int i = n - 1; i >= 0; i--) {
            while (!st.isEmpty() && heights[st.peek()] >= heights[i]) {
                st.pop();
            }
            // 如果栈空，说明右边没有比它小的
            right[i] = st.isEmpty() ? n : st.peek();
            st.push(i);
        }

        // 3. 枚举每个位置，计算面积
        int maxArea = 0;
        for (int i = 0; i < n; i++) {
            int width = right[i] - left[i] - 1;
            int area = heights[i] * width;
            maxArea = Math.max(maxArea, area);
        }

        return maxArea;
    }
}

```

### [1793. Maximum Score of a Good Subarray](https://leetcode.com/problems/maximum-score-of-a-good-subarray/)

#### 双指针法

```java
class Solution {
    public int maximumScore(int[] nums, int k) {
        int n = nums.length;
        int left = k, right = k; // 从中间起步
        int minVal = nums[k];
        int maxScore = nums[k]; // 初始 score 就是自己一个柱子
        while (left > 0 || right < n - 1) {
            // 尝试扩展的下一个值：nums[left - 1] 和 nums[right + 1]
            // 哪边大，往哪边扩展，让最小值减少得更慢。
            if (left == 0) {
                right++;
            } else if (right == n - 1) {
                left--;
            } else {
                if (nums[left - 1] > nums[right + 1]) {
                    left--;
                } else {
                    right++;
                }
            }
            // 扩展后，更新当前区间的最小值
            minVal = Math.min(minVal, Math.min(nums[left], nums[right]));
            // 更新score
            int score = minVal * (right - left + 1);
            maxScore = Math.max(maxScore, score);
        }
        return maxScore;
    }
}
```

| 项        | 复杂度    | 原因               |
| -------- | ------ | ---------------- |
| ⏱ 时间复杂度  | `O(n)` | 最多扩展整个数组，每次只扩一边  |
| 🧠 空间复杂度 | `O(1)` | 只用了常数级别变量，没有额外数组 |
#### 单调栈法
1. 枚举每一个元素 `nums[i]`，把它当作区间的“最小值”，向两边扩展，找最大能延伸的宽度。
    - 按照84题直方图的做法，如果我们确定了高度 `h = nums[p]`，我们想知道以 `h` 为最矮的柱子，**它最多能延伸多宽**。那这个宽度边界，就来自于：
    - **扩展到不能再包含比 `h` 更小的元素为止，否则矩形最小值就不是 `h` 了。**

以 `nums[i]` 为最小值的最大能延伸的区间是：`[left[i] + 1, right[i] - 1]`

2. 对每个 `i`，如果它的区间包含 `k`，就可以作为候选矩形：
```java
if (left[i] + 1 <= k && k <= right[i] - 1) {
    int width = right[i] - left[i] - 1;
    int area = nums[i] * width;
    maxArea = Math.max(maxArea, area);
}
```

```java
class Solution {
    public int maximumScore(int[] nums, int k) {
        int n = nums.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Stack<Integer> stack = new Stack<>();

        // 找每个元素左边第一个小于它的元素位置
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && nums[stack.peek()] >= nums[i]) {
                stack.pop();
            }
            left[i] = stack.isEmpty() ? -1 : stack.peek();
            stack.push(i);
        }

        // 清空栈
        stack.clear();

        // 找每个元素右边第一个小于它的元素位置
        for (int i = n - 1; i >= 0; i--) {
            while (!stack.isEmpty() && nums[stack.peek()] >= nums[i]) {
                stack.pop();
            }
            right[i] = stack.isEmpty() ? n : stack.peek();
            stack.push(i);
        }

        int maxArea = 0;

        for (int i = 0; i < n; i++) {
            if (left[i] + 1 <= k && k <= right[i] - 1) {
                int width = right[i] - left[i] - 1;
                int area = nums[i] * width;
                maxArea = Math.max(maxArea, area);
            }
        }

        return maxArea;
    }
}
```
### 📈 Maximum Profitable Groups

A team of analysts at Amazon needs to analyze the stock prices of Amazon over a period of several months.

A group of consecutively chosen months is said to be **maximum profitable** if the price in its **first or last month** is the **maximum** for the group. More formally, a group of consecutive months `[l, r]` (1 ≤ l ≤ r ≤ n) is said to be **maximum profitable** if either:

- `stockPrice[l] == max(stockPrice[l], stockPrice[l+1], ..., stockPrice[r])`,  
- or`stockPrice[r] == max(stockPrice[l], stockPrice[l+1], ..., stockPrice[r])`

Given prices over `n` consecutive months, find the number of **maximum profitable groups** which can be formed. Note: the months chosen must be consecutive, i.e., you must choose a subarray of the given array.

 🧪 Example
Consider there are `n = 3` months of data:  
`stockPrice = [2, 3, 2]`
All possible groups:

|Group|First|Last|Max in Group|Is Max Profitable?|
|---|---|---|---|---|
|[2]|2|2|2|Yes|
|[2, 3]|2|3|3|Yes|
|[2, 3, 2]|2|2|3|No|
|[3]|3|3|3|Yes|
|[3, 2]|3|2|3|Yes|
|[2]|2|2|2|Yes|

All five groups other than prices[2,3,2] are maximum profitable, in [2,3,2], the maximum value 3 is neither the first or the last element. Return 5.

#### 解题思路
转换思路：以每个元素 `prices[i]` 为最大值，统计它可以作为最大值的子数组个数，要求：`prices[i]` 是子数组的最大值，并且 **出现在子数组的开头或结尾**

对于每个元素 `prices[i]`，我们想知道它作为最大值时：
- 它左边最近的 **比它大的位置** `left`（开区间左边界）
- 它右边最近的 **比它大的位置** `right`（开区间右边界）

那以 `prices[i]` 为最大值的所有子数组就是：
- i 为左边界 的组合：有 `right - i` 种（右边可以选 `i` 到 `right-1`）
- i 为右边界的组合：有 `i - left` 种（左边可以选 `left+1` 到 `i`）

```java
合法数量 = (right - i) + (i - left) - 1  // 减去重复的 [i, i]
```

#### 完整代码
```java
import java.util.ArrayDeque;  
import java.util.Deque;  
  
public class countMaxProfitableGroups {  
    public static void main(String[] args) {  
        int[] stockPrices = {2, 3, 2};  
        int result = countMaxProfitableGroups(stockPrices);  
        System.out.println(result); // 应该输出 5    }  
    public static int countMaxProfitableGroups(int[] stockPrices) {  
        Deque<Integer> stack = new ArrayDeque<>();  
        int n = stockPrices.length;  
  
        int[] left = new int[n];  
        int[] right = new int[n];  
  
        for (int i = 0; i < n; i++) {  
            while(!stack.isEmpty() && stockPrices[stack.peek()] <= stockPrices[i]) {  
                stack.pop();  
            }  
            left[i] = stack.isEmpty() ? -1 : stack.peek();  
            stack.push(i);  
        }  
  
        stack.clear();  
  
        for (int i = n - 1; i >= 0; i--) {  
            while(!stack.isEmpty() && stockPrices[stack.peek()] <= stockPrices[i]) {  
                stack.pop();  
            }  
            right[i] = stack.isEmpty() ? n : stack.peek();  
            stack.push(i);  
        }  
        int count = 0;  
        for (int i = 0; i < n; i++) {  
            int leftLen = i - left[i];  
            int rightLen = right[i] - i;  
            count += leftLen + rightLen - 1; // 减1避免重复[i, i]  
        }  
        return count;  
    }  
}
```

### [1475. Final Prices With a Special Discount in a Shop](https://leetcode.com/problems/final-prices-with-a-special-discount-in-a-shop/)

```java
class Solution {
    public int[] finalPrices(int[] prices) {
        Deque<Integer> stack = new ArrayDeque<>();
        int n = prices.length;
        int[] newPrices = new int[n];
        stack.push(0);
        for (int i = 1; i < n; i++) {
            while (!stack.isEmpty() && prices[i] <= prices[stack.peek()]) {
                int idx = stack.pop();
                newPrices[idx] = prices[idx] - prices[i];
            }
            stack.push(i);
        }
        while (!stack.isEmpty()) {
            int idx = stack.pop();
            newPrices[idx] = prices[idx];
        }
        return newPrices;
    }
}
```
### [901. Online Stock Span](https://leetcode.com/problems/online-stock-span/)
记住十六字真言：
​及时去掉无用数据，保证栈中元素有序。​

- 当前 `price` ≥ 栈顶价格 → 栈顶已经“用不到了” → `pop` 掉
- 然后把它的 `span` 加到当前的 `span` 上 → 累计连续天数
- 最终再把当前 `[price, span]` 入栈，成为新的候选者
```java
class StockSpanner {
    Deque<int[]> stack; // 1. 成员变量：用来“记住”状态 栈中结构：[price, span]

    public StockSpanner() { // 2. 构造方法：初始化成员变量
        stack = new ArrayDeque<>();
    }

    public int next(int price) { // 3. 实例方法：每调用一次，处理今天的价格，返回跨度
        int span = 1;
        while (!stack.isEmpty() && price >= stack.peek()[0]) {
            span += stack.pop()[1];
        }
        stack.push(new int[]{price, span});
        return span;
    }
}
```

### [1019. Next Greater Node In Linked List](https://leetcode.com/problems/next-greater-node-in-linked-list/)
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public int[] nextLargerNodes(ListNode head) {
        int size = 0;
        ListNode node = head;
        while (node != null) {
            size++;
            node = node.next;
        }
        int[] result = new int[size];
        Deque<Integer> stack = new ArrayDeque<>();
        int i = 0;
        for (ListNode curr = head; curr != null; curr = curr.next) {
            while (!stack.isEmpty() && curr.val > result[stack.peek()]) {
                result[stack.pop()] = curr.val;
            }
            stack.push(i);
            result[i] = curr.val;
            i++;
        }
        for (int idx : stack) {
            result[idx] = 0;
        }
        return result;
    }
}
```

| 部分            | 复杂度  | 分析                                                                                         |
| ------------- | ---- | ------------------------------------------------------------------------------------------ |
| 遍历链表统计长度      | O(n) | `O(n)`，其中 `n` 是链表的长度。                                                                      |
| 转成数组 + 单调栈处理  | O(n) | **单调栈**，栈中每个元素（即数组下标）最多：<br>进栈一次，出栈一次<br>所以总共最多执行 `2n` 次操作（`n` 次 `push()` + `n` 次 `pop()`） |
| 最终处理残留栈中元素设为0 | O(n) | 最多 `n` 次（栈最多含有 `n` 个元素） → `O(n)`                                                           |
总时间复杂度：`O(n)`

### [1944. Number of Visible People in a Queue](https://leetcode.com/problems/number-of-visible-people-in-a-queue/)
```java
class Solution {
    public int[] canSeePersonsCount(int[] heights) {
        int n = heights.length;
        int[] result = new int[n];
        Deque<Integer> stack = new ArrayDeque<>();

        for (int i = n - 1; i >= 0; i--) {
            int count = 0;
            while (!stack.isEmpty() && heights[i] > heights[stack.peek()]) { // 弹出所有比当前矮的人
                stack.pop();
                count++;
            }
            if (!stack.isEmpty()) { // 你只能看到一个比你高的人（第一个！），你不能连续看到多个比你高的人！
                count++;
            }
            result[i] = count;
            stack.push(i);
    
        }
        
        return result;
    }
}
```

### [85. Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)
将每一行当作**直方图底部**，将**从上到这一行的1的高度**累加起来，形成一个高度数组 `heights[]`，然后在每一行上应用 **84题的解法** —— 即在直方图中找最大矩形面积。
```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        int ans = 0;
        int[] height = new int[n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                height[j] = (matrix[i][j] == '1')? height[j] + 1 : 0;
            }
            ans = Math.max(ans, maxArea(height));
        }
        return ans;
    }

    public int maxArea(int[] height) {
        int n = height.length;
        Deque<Integer> stack = new ArrayDeque<>();
        int[] left = new int[n];
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && height[i] <= height[stack.peek()]) {
                stack.pop();
            }
            left[i] = stack.isEmpty() ? -1 : stack.peek(); 
            stack.push(i);
        }
        stack.clear();
        int[] right = new int[n];
        for (int i = n - 1; i >= 0; i--) {
            while (!stack.isEmpty() && height[i] <= height[stack.peek()]) {
                stack.pop();
            }
            right[i] = stack.isEmpty() ? n : stack.peek(); 
            stack.push(i);
        }
        int maxArea = 0;
        for (int i = 0; i < n; i++) {
            maxArea = Math.max(maxArea, (right[i] - left[i] - 1) * height[i]);
        }
        return maxArea;
    }
}
```

## 3.4图论
### 基础概念
| 中文名     | 英文名                          | 简要说明          |
| ------- | ---------------------------- | ------------- |
| 图       | Graph                        | 顶点（节点） + 边的集合 |
| 无向图     | Undirected Graph             | 边没有方向，例如 A-B  |
| 有向图     | Directed Graph               | 边有方向，例如 A→B   |
| 有向无环图   | Directed Acyclic Graph (DAG) | 不存在回路的有向图     |
| 节点 / 顶点 | Node / Vertex                | 图中的点          |
| 边       | Edge                         | 节点之间的连接       |
| 权重      | Weight                       | 边的代价（如距离、费用）  |
| 邻接表     | Adjacency List               | 存图的一种方法       |
| 邻接矩阵    | Adjacency Matrix             | 存图的另一种方法      |
### [797. All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)
```java
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> path = new LinkedList<>();
        path.add(0);
        dfs(graph, 0, path, result);
        return result;
    }

    private void dfs(int[][] graph, int currNode, List<Integer> path, List<List<Integer>> result) {
        if (currNode == graph.length - 1) {
            result.add(new ArrayList<>(path));
            return;
        }

        for (int nextNode : graph[currNode]) {
            path.add(nextNode);
            dfs(graph, nextNode, path, result);
            path.removeLast();
        }
    }
}
```

|项目|复杂度|说明|
|---|---|---|
|时间复杂度|O(2ⁿ × n)|最坏每个节点分叉出多个路径|
|空间复杂度|O(2ⁿ × n)|存储所有路径 + 递归栈|
Big O of two to the power of n times n.
Since the graph is a DAG and each node can have multiple outgoing edges, the number of paths from node 0 to node n - 1 can grow exponentially. In the worst case—say, a fully connected DAG where each node connects to all later nodes—we could have up to **2ⁿ** different paths. For each path, we store it as a list, which takes up to **O(n)** time to copy. So, the total time is **O(2ⁿ × n)**.