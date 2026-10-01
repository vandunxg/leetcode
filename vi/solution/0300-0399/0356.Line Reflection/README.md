---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Math
---

<!-- problem:start -->

# [356. Line Reflection 🔒](https://leetcode.com/problems/line-reflection)

[中文文档](/solution/0300-0399/0356.Line%20Reflection/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>n</code> điểm trên mặt phẳng 2D, hãy xác định xem có đường thẳng nào song song với trục y sao cho các điểm đã cho đối xứng qua đường thẳng đó hay không.</p>

<p>Nói cách khác, hãy xác định xem có tồn tại đường thẳng mà khi phản chiếu tất cả các điểm qua đó, tập điểm ban đầu có trùng với tập điểm sau phản chiếu hay không.</p>

<p><strong>Lưu ý</strong> rằng có thể có các điểm bị lặp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0356.Line%20Reflection/images/356_example_1.png" style="width: 389px; height: 340px;" />
<pre>
<strong>Đầu vào:</strong> points = [[1,1],[-1,1]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta có thể chọn đường thẳng x = 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0356.Line%20Reflection/images/356_example_2.png" style="width: 300px; height: 294px;" />
<pre>
<strong>Đầu vào:</strong> points = [[1,1],[-1,-1]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể chọn đường thẳng nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == points.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>8</sup> &lt;= points[i][j] &lt;= 10<sup>8</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải tốt hơn <code>O(n<sup>2</sup>)</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra xem các điểm có đối xứng qua một đường thẳng đứng hay không. Không cần thử mọi trục đối xứng có thể có: nếu tồn tại trục như vậy, nó đi qua trung điểm của hai tọa độ $x$ cực trị.
>
> Đặt $s=\min x+\max x$. Với mỗi điểm $(x,y)$, điểm $(s-x,y)$ cũng phải có trong tập. Lưu các điểm rồi kiểm tra một lượt.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isReflected(self, points: List[List[int]]) -> bool:
        min_x, max_x = inf, -inf
        point_set = set()
        for x, y in points:
            min_x = min(min_x, x)
            max_x = max(max_x, x)
            point_set.add((x, y))
        s = min_x + max_x
        return all((s - x, y) in point_set for x, y in points)
```

#### Java

```java
class Solution {
    public boolean isReflected(int[][] points) {
        final int inf = 1 << 30;
        int minX = inf, maxX = -inf;
        Set<List<Integer>> pointSet = new HashSet<>();
        for (int[] p : points) {
            minX = Math.min(minX, p[0]);
            maxX = Math.max(maxX, p[0]);
            pointSet.add(List.of(p[0], p[1]));
        }
        int s = minX + maxX;
        for (int[] p : points) {
            if (!pointSet.contains(List.of(s - p[0], p[1]))) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isReflected(vector<vector<int>>& points) {
        const int inf = 1 << 30;
        int minX = inf, maxX = -inf;
        set<pair<int, int>> pointSet;
        for (auto& p : points) {
            minX = min(minX, p[0]);
            maxX = max(maxX, p[0]);
            pointSet.insert({p[0], p[1]});
        }
        int s = minX + maxX;
        for (auto& p : points) {
            if (!pointSet.count({s - p[0], p[1]})) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func isReflected(points [][]int) bool {
	const inf = 1 << 30
	minX, maxX := inf, -inf
	pointSet := map[[2]int]bool{}
	for _, p := range points {
		minX = min(minX, p[0])
		maxX = max(maxX, p[0])
		pointSet[[2]int{p[0], p[1]}] = true
	}
	s := minX + maxX
	for _, p := range points {
		if !pointSet[[2]int{s - p[0], p[1]}] {
			return false
		}
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
