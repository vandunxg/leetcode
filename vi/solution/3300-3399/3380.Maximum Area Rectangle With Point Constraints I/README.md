---
comments: true
difficulty: Medium
rating: 1743
source: Weekly Contest 427 Q2
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Geometry
    - Array
    - Math
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [3380. Maximum Area Rectangle With Point Constraints I](https://leetcode.com/problems/maximum-area-rectangle-with-point-constraints-i)

[中文文档](/solution/3300-3399/3380.Maximum%20Area%20Rectangle%20With%20Point%20Constraints%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>points</code>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu diễn tọa độ của một điểm trên mặt phẳng vô hạn.</p>

<p>Nhiệm vụ của bạn là tìm <strong>diện tích lớn nhất</strong> của một hình chữ nhật thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Có thể được tạo thành bằng cách dùng <strong>bốn</strong> điểm trong mảng làm các đỉnh.</li>
	<li><strong>Không</strong> chứa bất kỳ điểm nào khác ở bên trong hoặc trên biên.</li>
	<li>Có các cạnh <strong>song song</strong> với các trục tọa độ.</li>
</ul>

<p>Trả về <strong>diện tích lớn nhất</strong> có thể tạo thành, hoặc -1 nếu không thể tạo được hình chữ nhật nào như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[1,1],[1,3],[3,1],[3,3]]</span></p>

<p><strong>Đầu ra: </strong>4</p>

<p><strong>Giải thích:</strong></p>

<p><strong class="example"><img alt="Example 1 diagram" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3380.Maximum%20Area%20Rectangle%20With%20Point%20Constraints%20I/images/3380_ex0.png" style="width: 250px; height: 250px;" /></strong></p>

<p>Ta có thể tạo một hình chữ nhật với 4 điểm này làm các đỉnh và không có điểm nào khác nằm bên trong hoặc trên biên<!-- notionvc: f270d0a3-a596-4ed6-9997-2c7416b2b4ee -->. Vì vậy, diện tích lớn nhất có thể là 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[1,1],[1,3],[3,1],[3,3],[2,2]]</span></p>

<p><strong>Đầu ra:</strong><b> </b>-1</p>

<p><strong>Giải thích:</strong></p>

<p><strong class="example"><img alt="Example 2 diagram" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3380.Maximum%20Area%20Rectangle%20With%20Point%20Constraints%20I/images/3380_ex1.png" style="width: 230px; height: 230px;" /></strong></p>

<p>Chỉ có một hình chữ nhật có thể tạo thành từ các điểm <code>[1,1], [1,3], [3,1]</code> và <code>[3,3]</code>, nhưng <code>[2,2]</code> sẽ luôn nằm bên trong hình chữ nhật đó. Vì vậy, kết quả là -1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[1,1],[1,3],[3,1],[3,3],[1,2],[3,2]]</span></p>

<p><strong>Đầu ra: </strong>2</p>

<p><strong>Giải thích:</strong></p>

<p><strong class="example"><img alt="Example 3 diagram" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3380.Maximum%20Area%20Rectangle%20With%20Point%20Constraints%20I/images/3380_ex2.png" style="width: 230px; height: 230px;" /></strong></p>

<p>Hình chữ nhật có diện tích lớn nhất được tạo bởi các điểm <code>[1,3], [1,2], [3,2], [3,3]</code>, với diện tích bằng 2. Ngoài ra, các điểm <code>[1,1], [1,2], [3,1], [3,2]</code> cũng tạo thành một hình chữ nhật hợp lệ có cùng diện tích.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 10</code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 100</code></li>
	<li>Tất cả các điểm đã cho là <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một hình chữ nhật có các cạnh song song với trục được xác định bởi hai đỉnh đối diện, và không có điểm nào khác được nằm bên trong hoặc trên biên ngoài bốn đỉnh. Với $n \le 10$, ta thử mọi cặp điểm rồi kiểm tra các điểm còn lại.
>
> Từ một cặp điểm, ta tạo được một hình chữ nhật bằng cách lấy $\min/\max$. Một điểm nằm trên phần biên không phải đỉnh hoặc nằm bên trong sẽ khiến hình chữ nhật không hợp lệ; ta đếm số điểm trùng với các đỉnh.
>
> Nếu có đúng bốn đỉnh thì cập nhật diện tích. Nếu không có trường hợp nào hợp lệ, trả về $-1$.

<!-- thinking:end -->

Ta có thể liệt kê đỉnh dưới bên trái $(x_3, y_3)$ và đỉnh trên bên phải $(x_4, y_4)$ của hình chữ nhật. Sau đó, ta liệt kê tất cả các điểm $(x, y)$ và kiểm tra xem điểm đó có nằm bên trong hoặc trên biên của hình chữ nhật hay không. Nếu có, hình chữ nhật không thỏa mãn điều kiện. Nếu không, ta loại các điểm nằm ngoài hình chữ nhật và kiểm tra xem còn lại 4 điểm hay không. Nếu có, 4 điểm này có thể tạo thành một hình chữ nhật. Ta tính diện tích hình chữ nhật và lấy giá trị lớn nhất.

Độ phức tạp thời gian là $O(n^3)$, trong đó $n$ là độ dài của mảng $\textit{points}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxRectangleArea(self, points: List[List[int]]) -> int:
        def check(x1: int, y1: int, x2: int, y2: int) -> bool:
            cnt = 0
            for x, y in points:
                if x < x1 or x > x2 or y < y1 or y > y2:
                    continue
                if (x == x1 or x == x2) and (y == y1 or y == y2):
                    cnt += 1
                    continue
                return False
            return cnt == 4

        ans = -1
        for i, (x1, y1) in enumerate(points):
            for x2, y2 in points[:i]:
                x3, y3 = min(x1, x2), min(y1, y2)
                x4, y4 = max(x1, x2), max(y1, y2)
                if check(x3, y3, x4, y4):
                    ans = max(ans, (x4 - x3) * (y4 - y3))
        return ans
```

#### Java

```java
class Solution {
    public int maxRectangleArea(int[][] points) {
        int ans = -1;
        for (int i = 0; i < points.length; ++i) {
            int x1 = points[i][0], y1 = points[i][1];
            for (int j = 0; j < i; ++j) {
                int x2 = points[j][0], y2 = points[j][1];
                int x3 = Math.min(x1, x2), y3 = Math.min(y1, y2);
                int x4 = Math.max(x1, x2), y4 = Math.max(y1, y2);
                if (check(points, x3, y3, x4, y4)) {
                    ans = Math.max(ans, (x4 - x3) * (y4 - y3));
                }
            }
        }
        return ans;
    }

    private boolean check(int[][] points, int x1, int y1, int x2, int y2) {
        int cnt = 0;
        for (var p : points) {
            int x = p[0];
            int y = p[1];
            if (x < x1 || x > x2 || y < y1 || y > y2) {
                continue;
            }
            if ((x == x1 || x == x2) && (y == y1 || y == y2)) {
                cnt++;
                continue;
            }
            return false;
        }
        return cnt == 4;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxRectangleArea(vector<vector<int>>& points) {
        auto check = [&](int x1, int y1, int x2, int y2) -> bool {
            int cnt = 0;
            for (const auto& point : points) {
                int x = point[0];
                int y = point[1];
                if (x < x1 || x > x2 || y < y1 || y > y2) {
                    continue;
                }
                if ((x == x1 || x == x2) && (y == y1 || y == y2)) {
                    cnt++;
                    continue;
                }
                return false;
            }
            return cnt == 4;
        };

        int ans = -1;
        for (int i = 0; i < points.size(); i++) {
            int x1 = points[i][0], y1 = points[i][1];
            for (int j = 0; j < i; j++) {
                int x2 = points[j][0], y2 = points[j][1];
                int x3 = min(x1, x2), y3 = min(y1, y2);
                int x4 = max(x1, x2), y4 = max(y1, y2);
                if (check(x3, y3, x4, y4)) {
                    ans = max(ans, (x4 - x3) * (y4 - y3));
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxRectangleArea(points [][]int) int {
	check := func(x1, y1, x2, y2 int) bool {
		cnt := 0
		for _, point := range points {
			x, y := point[0], point[1]
			if x < x1 || x > x2 || y < y1 || y > y2 {
				continue
			}
			if (x == x1 || x == x2) && (y == y1 || y == y2) {
				cnt++
				continue
			}
			return false
		}
		return cnt == 4
	}

	ans := -1
	for i := 0; i < len(points); i++ {
		x1, y1 := points[i][0], points[i][1]
		for j := 0; j < i; j++ {
			x2, y2 := points[j][0], points[j][1]
			x3, y3 := min(x1, x2), min(y1, y2)
			x4, y4 := max(x1, x2), max(y1, y2)
			if check(x3, y3, x4, y4) {
				ans = max(ans, (x4-x3)*(y4-y3))
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxRectangleArea(points: number[][]): number {
    const check = (x1: number, y1: number, x2: number, y2: number): boolean => {
        let cnt = 0;
        for (const point of points) {
            const [x, y] = point;
            if (x < x1 || x > x2 || y < y1 || y > y2) {
                continue;
            }
            if ((x === x1 || x === x2) && (y === y1 || y === y2)) {
                cnt++;
                continue;
            }
            return false;
        }
        return cnt === 4;
    };

    let ans = -1;
    for (let i = 0; i < points.length; i++) {
        const [x1, y1] = points[i];
        for (let j = 0; j < i; j++) {
            const [x2, y2] = points[j];
            const [x3, y3] = [Math.min(x1, x2), Math.min(y1, y2)];
            const [x4, y4] = [Math.max(x1, x2), Math.max(y1, y2)];
            if (check(x3, y3, x4, y4)) {
                ans = Math.max(ans, (x4 - x3) * (y4 - y3));
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
