---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
---

<!-- problem:start -->

# [339. Nested List Weight Sum 🔒](https://leetcode.com/problems/nested-list-weight-sum)

[中文文档](/solution/0300-0399/0339.Nested%20List%20Weight%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách lồng nhau gồm các số nguyên <code>nestedList</code>. Mỗi phần tử là một số nguyên hoặc một danh sách có thể chứa số nguyên hay các danh sách khác.</p>

<p><strong>Độ sâu</strong> của một số nguyên là số lớp danh sách chứa nó. Ví dụ, trong danh sách lồng nhau <code>[1,[2,2],[[3],2],1]</code>, độ sâu của mỗi số nguyên được xác định theo số lớp danh sách chứa số đó.</p>

<p>Hãy trả về <em>tổng của từng số nguyên trong </em><code>nestedList</code><em> nhân với <strong>độ sâu</strong> của nó</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0339.Nested%20List%20Weight%20Sum/images/nestedlistweightsumex1.png" style="width: 405px; height: 99px;" />
<pre>
<strong>Đầu vào:</strong> nestedList = [[1,1],2,[1,1]]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Có bốn số 1 ở độ sâu 2 và một số 2 ở độ sâu 1. 1*2 + 1*2 + 2*1 + 1*2 + 1*2 = 10.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0339.Nested%20List%20Weight%20Sum/images/nestedlistweightsumex2.png" style="width: 315px; height: 106px;" />
<pre>
<strong>Đầu vào:</strong> nestedList = [1,[4,[6]]]
<strong>Đầu ra:</strong> 27
<strong>Giải thích:</strong> Có một số 1 ở độ sâu 1, một số 4 ở độ sâu 2 và một số 6 ở độ sâu 3. 1*1 + 4*2 + 6*3 = 27.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nestedList = [0]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nestedList.length &lt;= 50</code></li>
	<li>Giá trị các số nguyên trong danh sách lồng nhau nằm trong khoảng <code>[-100, 100]</code>.</li>
	<li><strong>Độ sâu</strong> tối đa của mỗi số nguyên không vượt quá <code>50</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần nhân mỗi số nguyên với độ sâu của nó rồi tính tổng. Cấu trúc lồng nhau là một cây $N$-ary, trong đó độ sâu bắt đầu từ $1$. Chỉ cần duyệt một lượt.
>
> DFS: nếu gặp số nguyên thì cộng $value\times depth$; nếu gặp danh sách thì đệ quy xuống các phần tử con với $depth+1$. Độ sâu ở cấp ngoài cùng là $1$.

<!-- thinking:end -->

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
    def depthSum(self, nestedList: List[NestedInteger]) -> int:
        def dfs(nestedList, depth):
            depth_sum = 0
            for item in nestedList:
                if item.isInteger():
                    depth_sum += item.getInteger() * depth
                else:
                    depth_sum += dfs(item.getList(), depth + 1)
            return depth_sum

        return dfs(nestedList, 1)
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
    public int depthSum(List<NestedInteger> nestedList) {
        return dfs(nestedList, 1);
    }

    private int dfs(List<NestedInteger> nestedList, int depth) {
        int depthSum = 0;
        for (NestedInteger item : nestedList) {
            if (item.isInteger()) {
                depthSum += item.getInteger() * depth;
            } else {
                depthSum += dfs(item.getList(), depth + 1);
            }
        }
        return depthSum;
    }
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
var depthSum = function (nestedList) {
    const dfs = (nestedList, depth) => {
        let depthSum = 0;
        for (const item of nestedList) {
            if (item.isInteger()) {
                depthSum += item.getInteger() * depth;
            } else {
                depthSum += dfs(item.getList(), depth + 1);
            }
        }
        return depthSum;
    };
    return dfs(nestedList, 1);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
