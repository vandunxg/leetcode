---
comments: true
difficulty: Easy
rating: 1247
source: Weekly Contest 159 Q1
tags:
    - Geometry
    - Array
    - Math
---

<!-- problem:start -->

# [1232. Check If It Is a Straight Line](https://leetcode.com/problems/check-if-it-is-a-straight-line)

[中文文档](/solution/1200-1299/1232.Check%20If%20It%20Is%20a%20Straight%20Line/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>coordinates</code>, trong đó <code>coordinates[i] = [x, y]</code> và <code>[x, y]</code> biểu thị tọa độ của một điểm. Hãy kiểm tra xem các điểm này có nằm trên cùng một đường thẳng trong mặt phẳng XY hay không.</p>

<p>&nbsp;</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1232.Check%20If%20It%20Is%20a%20Straight%20Line/images/untitled-diagram-2.jpg" style="width: 336px; height: 336px;" /></p>

<pre>
<strong>Đầu vào:</strong> coordinates = [[1,2],[2,3],[3,4],[4,5],[5,6],[6,7]]
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1232.Check%20If%20It%20Is%20a%20Straight%20Line/images/untitled-diagram-1.jpg" style="width: 348px; height: 336px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> coordinates = [[1,1],[2,2],[3,4],[4,5],[5,6],[7,7]]
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;=&nbsp;coordinates.length &lt;= 1000</code></li>
	<li><code>coordinates[i].length == 2</code></li>
	<li><code>-10^4 &lt;=&nbsp;coordinates[i][0],&nbsp;coordinates[i][1] &lt;= 10^4</code></li>
	<li><code>coordinates</code> không chứa điểm nào bị lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Các điểm thẳng hàng khi và chỉ khi mọi vector từ điểm đầu tiên đều song song với cạnh đầu tiên. So sánh hệ số góc sẽ gặp vấn đề với đường thẳng đứng và sai số số thực. Tích có hướng $(x-x_1)(y_2-y_1)=(y-y_1)(x_2-x_1)$ giúp tránh phép chia.
>
> Ta cố định hai điểm đầu tiên rồi kiểm tra đẳng thức trên các điểm còn lại. Vì $n \le 1000$, chỉ cần một lượt duyệt tuyến tính. Các điểm đều khác nhau nên không có vector nào bằng vector không.

<!-- thinking:end -->

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng `coordinates`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkStraightLine(self, coordinates: List[List[int]]) -> bool:
        x1, y1 = coordinates[0]
        x2, y2 = coordinates[1]
        for x, y in coordinates[2:]:
            if (x - x1) * (y2 - y1) != (y - y1) * (x2 - x1):
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean checkStraightLine(int[][] coordinates) {
        int x1 = coordinates[0][0], y1 = coordinates[0][1];
        int x2 = coordinates[1][0], y2 = coordinates[1][1];
        for (int i = 2; i < coordinates.length; ++i) {
            int x = coordinates[i][0], y = coordinates[i][1];
            if ((x - x1) * (y2 - y1) != (y - y1) * (x2 - x1)) {
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
    bool checkStraightLine(vector<vector<int>>& coordinates) {
        int x1 = coordinates[0][0], y1 = coordinates[0][1];
        int x2 = coordinates[1][0], y2 = coordinates[1][1];
        for (int i = 2; i < coordinates.size(); ++i) {
            int x = coordinates[i][0], y = coordinates[i][1];
            if ((x - x1) * (y2 - y1) != (y - y1) * (x2 - x1)) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func checkStraightLine(coordinates [][]int) bool {
	x1, y1 := coordinates[0][0], coordinates[0][1]
	x2, y2 := coordinates[1][0], coordinates[1][1]
	for i := 2; i < len(coordinates); i++ {
		x, y := coordinates[i][0], coordinates[i][1]
		if (x-x1)*(y2-y1) != (y-y1)*(x2-x1) {
			return false
		}
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
