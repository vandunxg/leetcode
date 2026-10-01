---
comments: true
difficulty: Medium
tags:
    - Stack
    - Depth-First Search
    - Breadth-First Search
---

<!-- problem:start -->

# [364. Nested List Weight Sum II 🔒](https://leetcode.com/problems/nested-list-weight-sum-ii)

[中文文档](/solution/0300-0399/0364.Nested%20List%20Weight%20Sum%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách lồng nhau <code>nestedList</code> gồm các số nguyên. Mỗi phần tử là số nguyên hoặc một danh sách có thể chứa số nguyên hay các danh sách khác.</p>

<p><strong>Độ sâu</strong> của một số nguyên là số danh sách chứa nó. Ví dụ, trong danh sách lồng nhau <code>[1,[2,2],[[3],2],1]</code>, mỗi số nguyên có <strong>độ sâu</strong> tương ứng với số danh sách bao quanh nó. Gọi <code>maxDepth</code> là <strong>độ sâu lớn nhất</strong> của các số nguyên.</p>

<p><strong>Trọng số</strong> của một số nguyên được tính bằng <code>maxDepth - (the depth of the integer) + 1</code>.</p>

<p>Trả về <em>tổng các số nguyên trong </em><code>nestedList</code><em> sau khi nhân mỗi số với <strong>trọng số</strong> tương ứng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0364.Nested%20List%20Weight%20Sum%20II/images/nestedlistweightsumiiex1.png" style="width: 426px; height: 181px;" />
<pre>
<strong>Đầu vào:</strong> nestedList = [[1,1],2,[1,1]]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Bốn số 1 có trọng số 1, một số 2 có trọng số 2.
1*1 + 1*1 + 2*2 + 1*1 + 1*1 = 8
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0364.Nested%20List%20Weight%20Sum%20II/images/nestedlistweightsumiiex2.png" style="width: 349px; height: 192px;" />
<pre>
<strong>Đầu vào:</strong> nestedList = [1,[4,[6]]]
<strong>Đầu ra:</strong> 17
<strong>Giải thích:</strong> Một số 1 ở độ sâu 3, một số 4 ở độ sâu 2 và một số 6 ở độ sâu 1.
1*3 + 4*2 + 6*1 = 17
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nestedList.length &lt;= 50</code></li>
	<li>Giá trị các số nguyên trong danh sách lồng nhau nằm trong khoảng <code>[-100, 100]</code>.</li>
	<li><strong>Độ sâu</strong> lớn nhất của số nguyên bất kỳ không vượt quá <code>50</code>.</li>
	<li>Không có danh sách rỗng.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Khác với bài 339, trọng số được tính bằng $maxDepth-depth+1$. Có thể duyệt hai lượt (tìm độ sâu rồi tính tổng), nhưng một lượt DFS cũng có thể đồng thời tính tổng thông thường $s$ và tổng có trọng số theo độ sâu $ws$.
>
> Kết quả cần tìm là $\sum a_i(maxDepth-d_i+1)=(maxDepth+1)s-ws$. DFS cập nhật $maxDepth$; khi gặp số nguyên, cộng giá trị đó vào cả $s$ và $ws$.

<!-- thinking:end -->

Giả sử các số nguyên là $a_1, a_2, \cdots, a_n$, độ sâu tương ứng là $d_1, d_2, \cdots, d_n$, và độ sâu lớn nhất là $\textit{maxDepth}$. Khi đó, đáp án là:

$$
a_1 \times \textit{maxDepth} - a_1 \times d_1 + a_1 + a_2 \times \textit{maxDepth} - a_2 \times d_2 + a_2 + \cdots + a_n \times \textit{maxDepth} - a_n \times d_n + a_n
$$

có thể viết thành:

$$
(\textit{maxDepth} + 1) \times (a_1 + a_2 + \cdots + a_n) - (a_1 \times d_1 + a_2 \times d_2 + \cdots + a_n \times d_n)
$$

Gọi tổng tất cả số nguyên là $s$, tổng các tích giữa mỗi số nguyên với độ sâu của nó là $ws$. Khi đó, đáp án là:

$$
(\textit{maxDepth} + 1) \times s - ws
$$

Vì vậy, ta thiết kế hàm $dfs(x, d)$ để bắt đầu duyệt từ $x$ với độ sâu $d$. Hàm $dfs(x, d)$ hoạt động như sau:

- Trước tiên, cập nhật $\textit{maxDepth} = \max(\textit{maxDepth}, d)$;
- Nếu $x$ là số nguyên, cập nhật $s = s + x$, $ws = ws + x \times d$;
- Nếu không, ta duyệt đệ quy từng phần tử $y$ của $x$ và gọi $dfs(y, d + 1)$.

Ta duyệt toàn bộ danh sách, gọi $dfs(x, 1)$ cho mỗi phần tử $x$, rồi trả về $(\textit{maxDepth} + 1) \times s - ws$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng số nguyên.

<!-- tabs:start -->

#### Python3

```python
# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
# class NestedInteger:
#    def __init__(self, value=None):
#        """
#        If value is not specified, initializes an empty list.
#        Otherwise initializes a single integer equal to value.
#        """
#
#    def isInteger(self):
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        :rtype bool
#        """
#
#    def add(self, elem):
#        """
#        Set this NestedInteger to hold a nested list and adds a nested integer elem to it.
#        :rtype void
#        """
#
#    def setInteger(self, value):
#        """
#        Set this NestedInteger to hold a single integer equal to value.
#        :rtype void
#        """
#
#    def getInteger(self):
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        :rtype int
#        """
#
#    def getList(self):
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        :rtype List[NestedInteger]
#        """
class Solution:
    def depthSumInverse(self, nestedList: List[NestedInteger]) -> int:
        def dfs(x, d):
            nonlocal maxDepth, s, ws
            maxDepth = max(maxDepth, d)
            if x.isInteger():
                s += x.getInteger()
                ws += x.getInteger() * d
            else:
                for y in x.getList():
                    dfs(y, d + 1)

        maxDepth = s = ws = 0
        for x in nestedList:
            dfs(x, 1)
        return (maxDepth + 1) * s - ws
```

#### Java

```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *     // Constructor initializes an empty nested list.
 *     public NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     public NestedInteger(int value);
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // Set this NestedInteger to hold a single integer.
 *     public void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     public void add(NestedInteger ni);
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return empty list if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
class Solution {
    private int maxDepth;
    private int ws;
    private int s;

    public int depthSumInverse(List<NestedInteger> nestedList) {
        for (NestedInteger x : nestedList) {
            dfs(x, 1);
        }
        return (maxDepth + 1) * s - ws;
    }

    private void dfs(NestedInteger x, int d) {
        maxDepth = Math.max(maxDepth, d);
        if (x.isInteger()) {
            ws += x.getInteger() * d;
            s += x.getInteger();
        } else {
            for (NestedInteger y : x.getList()) {
                dfs(y, d + 1);
            }
        }
    }
}
```

#### C++

```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Constructor initializes an empty nested list.
 *     NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     NestedInteger(int value);
 *
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Set this NestedInteger to hold a single integer.
 *     void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     void add(const NestedInteger &ni);
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class Solution {
public:
    int depthSumInverse(vector<NestedInteger>& nestedList) {
        int maxDepth = 0, ws = 0, s = 0;
        function<void(NestedInteger&, int)> dfs = [&](NestedInteger& x, int d) {
            maxDepth = max(maxDepth, d);
            if (x.isInteger()) {
                ws += x.getInteger() * d;
                s += x.getInteger();
            } else {
                for (auto& y : x.getList()) {
                    dfs(y, d + 1);
                }
            }
        };
        for (auto& x : nestedList) {
            dfs(x, 1);
        }
        return (maxDepth + 1) * s - ws;
    }
};
```

#### Go

```go
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * type NestedInteger struct {
 * }
 *
 * // Return true if this NestedInteger holds a single integer, rather than a nested list.
 * func (n NestedInteger) IsInteger() bool {}
 *
 * // Return the single integer that this NestedInteger holds, if it holds a single integer
 * // The result is undefined if this NestedInteger holds a nested list
 * // So before calling this method, you should have a check
 * func (n NestedInteger) GetInteger() int {}
 *
 * // Set this NestedInteger to hold a single integer.
 * func (n *NestedInteger) SetInteger(value int) {}
 *
 * // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 * func (n *NestedInteger) Add(elem NestedInteger) {}
 *
 * // Return the nested list that this NestedInteger holds, if it holds a nested list
 * // The list length is zero if this NestedInteger holds a single integer
 * // You can access NestedInteger's List element directly if you want to modify it
 * func (n NestedInteger) GetList() []*NestedInteger {}
 */
func depthSumInverse(nestedList []*NestedInteger) int {
	var maxDepth, ws, s int
	var dfs func(*NestedInteger, int)
	dfs = func(x *NestedInteger, d int) {
		maxDepth = max(maxDepth, d)
		if x.IsInteger() {
			ws += x.GetInteger() * d
			s += x.GetInteger()
		} else {
			for _, y := range x.GetList() {
				dfs(y, d+1)
			}
		}
	}
	for _, x := range nestedList {
		dfs(x, 1)
	}
	return (maxDepth+1)*s - ws
}
```

#### TypeScript

```ts
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *     If value is provided, then it holds a single integer
 *     Otherwise it holds an empty nested list
 *     constructor(value?: number) {
 *         ...
 *     };
 *
 *     Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     isInteger(): boolean {
 *         ...
 *     };
 *
 *     Return the single integer that this NestedInteger holds, if it holds a single integer
 *     Return null if this NestedInteger holds a nested list
 *     getInteger(): number | null {
 *         ...
 *     };
 *
 *     Set this NestedInteger to hold a single integer equal to value.
 *     setInteger(value: number) {
 *         ...
 *     };
 *
 *     Set this NestedInteger to hold a nested list and adds a nested integer elem to it.
 *     add(elem: NestedInteger) {
 *         ...
 *     };
 *
 *     Return the nested list that this NestedInteger holds,
 *     or an empty list if this NestedInteger holds a single integer
 *     getList(): NestedInteger[] {
 *         ...
 *     };
 * };
 */

function depthSumInverse(nestedList: NestedInteger[]): number {
    let [maxDepth, ws, s] = [0, 0, 0];
    const dfs = (x: NestedInteger, d: number) => {
        maxDepth = Math.max(maxDepth, d);
        if (x.isInteger()) {
            ws += x.getInteger() * d;
            s += x.getInteger();
        } else {
            for (const y of x.getList()) {
                dfs(y, d + 1);
            }
        }
    };
    for (const x of nestedList) {
        dfs(x, 1);
    }
    return (maxDepth + 1) * s - ws;
}
```

#### JavaScript

```js
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * function NestedInteger() {
 *
 *     Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     @return {boolean}
 *     this.isInteger = function() {
 *         ...
 *     };
 *
 *     Return the single integer that this NestedInteger holds, if it holds a single integer
 *     Return null if this NestedInteger holds a nested list
 *     @return {integer}
 *     this.getInteger = function() {
 *         ...
 *     };
 *
 *     Set this NestedInteger to hold a single integer equal to value.
 *     @return {void}
 *     this.setInteger = function(value) {
 *         ...
 *     };
 *
 *     Set this NestedInteger to hold a nested list and adds a nested integer elem to it.
 *     @return {void}
 *     this.add = function(elem) {
 *         ...
 *     };
 *
 *     Return the nested list that this NestedInteger holds, if it holds a nested list
 *     Return null if this NestedInteger holds a single integer
 *     @return {NestedInteger[]}
 *     this.getList = function() {
 *         ...
 *     };
 * };
 */
/**
 * @param {NestedInteger[]} nestedList
 * @return {number}
 */
var depthSumInverse = function (nestedList) {
    let [maxDepth, ws, s] = [0, 0, 0];
    const dfs = (x, d) => {
        maxDepth = Math.max(maxDepth, d);
        if (x.isInteger()) {
            ws += x.getInteger() * d;
            s += x.getInteger();
        } else {
            for (const y of x.getList()) {
                dfs(y, d + 1);
            }
        }
    };
    for (const x of nestedList) {
        dfs(x, 1);
    }
    return (maxDepth + 1) * s - ws;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
