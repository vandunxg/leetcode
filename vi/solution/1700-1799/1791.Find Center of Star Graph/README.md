---
comments: true
difficulty: Easy
rating: 1286
source: Weekly Contest 232 Q2
tags:
    - Graph
---

<!-- problem:start -->

# [1791. Find Center of Star Graph](https://leetcode.com/problems/find-center-of-star-graph)

[中文文档](/solution/1700-1799/1791.Find%20Center%20of%20Star%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Có một đồ thị vô hướng dạng <strong>ngôi sao</strong> gồm <code>n</code> đỉnh được đánh số từ <code>1</code> đến <code>n</code>. Đồ thị ngôi sao có một đỉnh <strong>tâm</strong> và <strong>đúng</strong> <code>n - 1</code> cạnh nối đỉnh tâm với mọi đỉnh khác.</p>

<p>Cho mảng số nguyên 2 chiều <code>edges</code>, trong đó mỗi <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết có một cạnh giữa hai đỉnh <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>. Hãy trả về tâm của đồ thị ngôi sao đã cho.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1791.Find%20Center%20of%20Star%20Graph/images/star_graph.png" style="width: 331px; height: 321px;" />
<pre>
<strong>Input:</strong> edges = [[1,2],[2,3],[4,2]]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Như hình trên, đỉnh 2 nối với mọi đỉnh khác nên 2 là tâm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> edges = [[1,2],[5,1],[1,3],[1,4]]
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>1 &lt;= u<sub>i,</sub> v<sub>i</sub> &lt;= n</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
<li><code>edges</code> đã cho biểu diễn một đồ thị ngôi sao hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: So sánh trực tiếp các đỉnh của hai cạnh đầu tiên

<!-- thinking:start -->

> **Tư duy**
>
> Tâm của ngôi sao kề với mọi cạnh, vì vậy bất kỳ hai cạnh nào cũng có chung đỉnh này. Không cần dựng đồ thị.
>
> Đầu mút nào của cạnh đầu tiên cũng xuất hiện trong cạnh thứ hai chính là tâm.

<!-- thinking:end -->

Đặc điểm của đỉnh tâm là nó nối với mọi đỉnh khác. Vì vậy, chỉ cần so sánh hai cạnh đầu tiên: đỉnh xuất hiện trong cả hai chính là đỉnh tâm.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findCenter(self, edges: List[List[int]]) -> int:
        return edges[0][0] if edges[0][0] in edges[1] else edges[0][1]
```

#### Java

```java
class Solution {
    public int findCenter(int[][] edges) {
        int a = edges[0][0], b = edges[0][1];
        int c = edges[1][0], d = edges[1][1];
        return a == c || a == d ? a : b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findCenter(vector<vector<int>>& edges) {
        int a = edges[0][0], b = edges[0][1];
        int c = edges[1][0], d = edges[1][1];
        return a == c || a == d ? a : b;
    }
};
```

#### Go

```go
func findCenter(edges [][]int) int {
	a, b := edges[0][0], edges[0][1]
	c, d := edges[1][0], edges[1][1]
	if a == c || a == d {
		return a
	}
	return b
}
```

#### TypeScript

```ts
function findCenter(edges: number[][]): number {
    for (let num of edges[0]) {
        if (edges[1].includes(num)) {
            return num;
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn find_center(edges: Vec<Vec<i32>>) -> i32 {
        if edges[0][0] == edges[1][0] || edges[0][0] == edges[1][1] {
            return edges[0][0];
        }
        edges[0][1]
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} edges
 * @return {number}
 */
var findCenter = function (edges) {
    const [a, b] = edges[0];
    const [c, d] = edges[1];
    return a == c || a == d ? a : b;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
