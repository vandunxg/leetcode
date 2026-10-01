---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [256. Paint House 🔒](https://leetcode.com/problems/paint-house)

[中文文档](/solution/0200-0299/0256.Paint%20House/README.md)

## Mô tả

<!-- description:start -->

<p>Có một dãy gồm <code>n</code> ngôi nhà, mỗi nhà có thể được sơn một trong ba màu: đỏ, xanh dương hoặc xanh lá. Chi phí sơn mỗi nhà tùy theo màu sẽ khác nhau. Bạn cần sơn tất cả các ngôi nhà sao cho không có hai nhà liền kề nào cùng màu.</p>

<p>Chi phí sơn mỗi ngôi nhà theo từng màu được biểu diễn bằng ma trận chi phí <code>n x 3</code> <code>costs</code>.</p>

<ul>
	<li>Ví dụ, <code>costs[0][0]</code> là chi phí sơn nhà <code>0</code> màu đỏ; <code>costs[1][2]</code> là chi phí sơn nhà 1 màu xanh lá, v.v.</li>
</ul>

<p>Hãy trả về <em>chi phí nhỏ nhất để sơn tất cả các ngôi nhà</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> costs = [[17,2,17],[16,16,5],[14,3,19]]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Sơn nhà 0 màu xanh dương, nhà 1 màu xanh lá, nhà 2 màu xanh dương.
Chi phí nhỏ nhất: 2 + 5 + 3 = 10.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> costs = [[7,6,2]]
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>costs.length == n</code></li>
	<li><code>costs[i].length == 3</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= costs[i][j] &lt;= 20</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai ngôi nhà liền kề không thể cùng màu, nên số cách sơn cần xét sẽ rất lớn nếu liệt kê tất cả. Chi phí nhỏ nhất khi sơn nhà $i$ màu $c$ chỉ phụ thuộc vào chi phí của hai màu còn lại ở nhà $i-1$.
>
> Ba biến cập nhật luân phiên lưu tổng chi phí nhỏ nhất ứng với từng màu; đáp án là giá trị nhỏ nhất trong ba tổng đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, costs: List[List[int]]) -> int:
        a = b = c = 0
        for ca, cb, cc in costs:
            a, b, c = min(b, c) + ca, min(a, c) + cb, min(a, b) + cc
        return min(a, b, c)
```

#### Java

```java
class Solution {
    public int minCost(int[][] costs) {
        int r = 0, g = 0, b = 0;
        for (int[] cost : costs) {
            int _r = r, _g = g, _b = b;
            r = Math.min(_g, _b) + cost[0];
            g = Math.min(_r, _b) + cost[1];
            b = Math.min(_r, _g) + cost[2];
        }
        return Math.min(r, Math.min(g, b));
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCost(vector<vector<int>>& costs) {
        int r = 0, g = 0, b = 0;
        for (auto& cost : costs) {
            int _r = r, _g = g, _b = b;
            r = min(_g, _b) + cost[0];
            g = min(_r, _b) + cost[1];
            b = min(_r, _g) + cost[2];
        }
        return min(r, min(g, b));
    }
};
```

#### Go

```go
func minCost(costs [][]int) int {
	r, g, b := 0, 0, 0
	for _, cost := range costs {
		_r, _g, _b := r, g, b
		r = min(_g, _b) + cost[0]
		g = min(_r, _b) + cost[1]
		b = min(_r, _g) + cost[2]
	}
	return min(r, min(g, b))
}
```

#### JavaScript

```js
/**
 * @param {number[][]} costs
 * @return {number}
 */
var minCost = function (costs) {
    let [a, b, c] = [0, 0, 0];
    for (let [ca, cb, cc] of costs) {
        [a, b, c] = [Math.min(b, c) + ca, Math.min(a, c) + cb, Math.min(a, b) + cc];
    }
    return Math.min(a, b, c);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
