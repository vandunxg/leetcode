---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons)

[中文文档](/solution/0400-0499/0452.Minimum%20Number%20of%20Arrows%20to%20Burst%20Balloons/README.md)

## Mô tả

<!-- description:start -->

<p>Có một số quả bóng hình cầu được dán lên bức tường phẳng biểu diễn mặt phẳng XY. Các quả bóng được biểu diễn bằng mảng số nguyên 2D <code>points</code>, trong đó <code>points[i] = [x<sub>start</sub>, x<sub>end</sub>]</code> mô tả quả bóng có <strong>đường kính ngang</strong> trải từ <code>x<sub>start</sub></code> đến <code>x<sub>end</sub></code>. Bạn không biết tọa độ y chính xác của các quả bóng.</p>

<p>Mũi tên có thể được bắn <strong>thẳng đứng lên trên</strong> (theo chiều dương của trục y) từ các điểm khác nhau trên trục x. Một quả bóng có <code>x<sub>start</sub></code> và <code>x<sub>end</sub></code> sẽ bị <strong>làm vỡ</strong> khi bắn mũi tên tại <code>x</code> nếu <code>x<sub>start</sub> &lt;= x &lt;= x<sub>end</sub></code>. Số lượng mũi tên có thể bắn là <strong>không giới hạn</strong>. Mũi tên sau khi bắn sẽ tiếp tục bay lên vô hạn và làm vỡ mọi quả bóng trên đường đi.</p>

<p>Cho mảng <code>points</code>, hãy trả về <em>số mũi tên <strong>ít nhất</strong> cần bắn để làm vỡ tất cả bóng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> points = [[10,16],[2,8],[1,6],[7,12]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có thể làm vỡ các quả bóng bằng 2 mũi tên:
- Bắn một mũi tên tại x = 6 để làm vỡ các quả bóng [2,8] và [1,6].
- Bắn một mũi tên tại x = 11 để làm vỡ các quả bóng [10,16] và [7,12].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> points = [[1,2],[3,4],[5,6],[7,8]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Mỗi quả bóng cần một mũi tên, tổng cộng là 4 mũi tên.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> points = [[1,2],[2,3],[3,4],[4,5]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có thể làm vỡ các quả bóng bằng 2 mũi tên:
- Bắn một mũi tên tại x = 2 để làm vỡ các quả bóng [1,2] và [2,3].
- Bắn một mũi tên tại x = 4 để làm vỡ các quả bóng [3,4] và [4,5].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 10<sup>5</sup></code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>-2<sup>31</sup> &lt;= x<sub>start</sub> &lt; x<sub>end</sub> &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một mũi tên làm vỡ mọi quả bóng phủ lên tọa độ $x$ đó; ta cần dùng ít mũi tên nhất. Bài toán tương đương với chọn ít điểm nhất để giao với tất cả các interval.
>
> Sắp xếp theo đầu mút phải. Đặt mũi tên hiện tại tại đầu mút phải của quả bóng vừa xét; nếu đầu mút trái của quả bóng tiếp theo nằm bên phải điểm đó, bắn thêm một mũi tên và đặt nó tại đầu mút phải mới.
>
> Đặt mũi tên tại điểm ngoài cùng bên phải của một interval giúp nó làm vỡ được nhiều quả bóng phía sau nhất có thể; sắp xếp theo đầu mút phải khiến lựa chọn greedy này tối ưu.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        ans, last = 0, -inf
        for a, b in sorted(points, key=lambda x: x[1]):
            if a > last:
                ans += 1
                last = b
        return ans
```

#### Java

```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        // 直接 a[1] - b[1] 可能会溢出
        Arrays.sort(points, Comparator.comparingInt(a -> a[1]));
        int ans = 0;
        long last = -(1L << 60);
        for (var p : points) {
            int a = p[0], b = p[1];
            if (a > last) {
                ++ans;
                last = b;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(), points.end(), [](vector<int>& a, vector<int>& b) {
            return a[1] < b[1];
        });
        int ans = 0;
        long long last = -(1LL << 60);
        for (auto& p : points) {
            int a = p[0], b = p[1];
            if (a > last) {
                ++ans;
                last = b;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findMinArrowShots(points [][]int) (ans int) {
	sort.Slice(points, func(i, j int) bool { return points[i][1] < points[j][1] })
	last := -(1 << 60)
	for _, p := range points {
		a, b := p[0], p[1]
		if a > last {
			ans++
			last = b
		}
	}
	return
}
```

#### TypeScript

```ts
function findMinArrowShots(points: number[][]): number {
    points.sort((a, b) => a[1] - b[1]);
    let ans = 0;
    let last = -Infinity;
    for (const [a, b] of points) {
        if (last < a) {
            ans++;
            last = b;
        }
    }
    return ans;
}
```

#### C#

```cs
public class Solution {
    public int FindMinArrowShots(int[][] points) {
        Array.Sort(points, (a, b) => a[1] < b[1] ? -1 : a[1] > b[1] ? 1 : 0);
        int ans = 0;
        long last = long.MinValue;
        foreach (var point in points) {
            if (point[0] > last) {
                ++ans;
                last = point[1];
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
