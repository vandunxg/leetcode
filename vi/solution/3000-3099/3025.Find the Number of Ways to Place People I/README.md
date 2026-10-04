---
comments: true
difficulty: Medium
rating: 1707
source: Biweekly Contest 123 Q2
tags:
    - Geometry
    - Array
    - Math
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [3025. Find the Number of Ways to Place People I](https://leetcode.com/problems/find-the-number-of-ways-to-place-people-i)

[中文文档](/solution/3000-3099/3025.Find%20the%20Number%20of%20Ways%20to%20Place%20People%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng 2D <code>points</code> có kích thước <code>n x 2</code>, biểu diễn tọa độ nguyên của một số điểm trên mặt phẳng 2D, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.</p>

<p>Hãy đếm số cặp điểm <code>(A, B)</code> sao cho</p>

<ul>
	<li><code>A</code> nằm ở phía <strong>trên bên trái</strong> của <code>B</code>, và</li>
	<li>không có điểm nào khác nằm trong hình chữ nhật (hoặc đoạn thẳng) do chúng tạo thành, <strong>kể cả biên</strong>, ngoại trừ hai điểm <code>A</code> và <code>B</code>.</li>
</ul>

<p>Trả về số lượng cặp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[1,1],[2,2],[3,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3025.Find%20the%20Number%20of%20Ways%20to%20Place%20People%20I/images/example1alicebob.png" style="width: 427px; height: 350px;" /></p>

<p>Không có cách nào chọn <code>A</code> và <code>B</code> sao cho <code>A</code> nằm ở phía trên bên trái của <code>B</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[6,2],[4,4],[2,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img height="365" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3025.Find%20the%20Number%20of%20Ways%20to%20Place%20People%20I/images/t2.jpg" width="1321" /></p>

<ul>
	<li>Hình bên trái là cặp <code>(points[1], points[0])</code>, trong đó <code>points[1]</code> nằm ở phía trên bên trái của <code>points[0]</code> và hình chữ nhật không chứa điểm nào khác.</li>
	<li>Hình ở giữa là cặp <code>(points[2], points[1])</code>, tương tự hình bên trái, đây là một cặp hợp lệ.</li>
	<li>Hình bên phải là cặp <code>(points[2], points[0])</code>, trong đó <code>points[2]</code> nằm ở phía trên bên trái của <code>points[0]</code>, nhưng <code>points[1]</code> nằm bên trong hình chữ nhật nên đây không phải là một cặp hợp lệ.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[3,1],[1,3],[1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3025.Find%20the%20Number%20of%20Ways%20to%20Place%20People%20I/images/t3.jpg" style="width: 1269px; height: 350px;" /></p>

<ul>
	<li>Hình bên trái là cặp <code>(points[2], points[0])</code>, trong đó <code>points[2]</code> nằm ở phía trên bên trái của <code>points[0]</code> và không có điểm nào khác trên đoạn thẳng mà chúng tạo thành. Lưu ý rằng đây vẫn là một trạng thái hợp lệ khi hai điểm tạo thành một đường thẳng.</li>
	<li>Hình ở giữa là cặp <code>(points[1], points[2])</code>, tương tự hình bên trái, đây là một cặp hợp lệ.</li>
	<li>Hình bên phải là cặp <code>(points[1], points[0])</code>, đây không phải là một cặp hợp lệ vì <code>points[2]</code> nằm trên biên của hình chữ nhật.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 50</code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>0 &lt;= points[i][0], points[i][1] &lt;= 50</code></li>
	<li>Tất cả <code>points[i]</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp và phân loại

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 50$. Việc liệt kê một góc trên bên trái và một góc dưới bên phải rồi quét hình chữ nhật là $O(n^3)$ và vẫn chạy được, nhưng sẽ lặp lại rất nhiều công việc.
>
> Sau khi sắp xếp theo $x$ tăng dần, và nếu bằng nhau thì theo $y$ giảm dần, giá trị $y$ của góc dưới bên phải hợp lệ phải tăng nghiêm ngặt khi $x$ tăng; nếu không, điểm mới sẽ nằm bên trong một hình chữ nhật trước đó.
>
> Với mỗi điểm phía trên bên trái, chúng ta lưu $y_2$ lớn nhất đã chọn và chỉ đếm một cặp khi $\textit{maxY} < y_2 \le y_1$.

<!-- thinking:end -->

Đầu tiên, chúng ta sắp xếp mảng. Sau đó, chúng ta có thể phân loại kết quả dựa trên các tính chất của một tam giác.

- Nếu tổng của hai số nhỏ hơn hoặc bằng số lớn nhất, chúng không thể tạo thành một tam giác. Trả về "Invalid".
- Nếu ba số bằng nhau, đó là tam giác đều. Trả về "Equilateral".
- Nếu có hai số bằng nhau, đó là tam giác cân. Trả về "Isosceles".
- Nếu không thuộc các trường hợp trên, đó là tam giác thường. Trả về "Scalene".

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfPairs(self, points: List[List[int]]) -> int:
        points.sort(key=lambda x: (x[0], -x[1]))
        ans = 0
        for i, (_, y1) in enumerate(points):
            max_y = -inf
            for _, y2 in points[i + 1 :]:
                if max_y < y2 <= y1:
                    max_y = y2
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int numberOfPairs(int[][] points) {
        Arrays.sort(points, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        int ans = 0;
        int n = points.length;
        final int inf = 1 << 30;
        for (int i = 0; i < n; ++i) {
            int y1 = points[i][1];
            int maxY = -inf;
            for (int j = i + 1; j < n; ++j) {
                int y2 = points[j][1];
                if (maxY < y2 && y2 <= y1) {
                    maxY = y2;
                    ++ans;
                }
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
    int numberOfPairs(vector<vector<int>>& points) {
        sort(points.begin(), points.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[0] < b[0] || (a[0] == b[0] && b[1] < a[1]);
        });
        int n = points.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int y1 = points[i][1];
            int maxY = INT_MIN;
            for (int j = i + 1; j < n; ++j) {
                int y2 = points[j][1];
                if (maxY < y2 && y2 <= y1) {
                    maxY = y2;
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfPairs(points [][]int) (ans int) {
	sort.Slice(points, func(i, j int) bool {
		return points[i][0] < points[j][0] || points[i][0] == points[j][0] && points[j][1] < points[i][1]
	})
	for i, p1 := range points {
		y1 := p1[1]
		maxY := math.MinInt32
		for _, p2 := range points[i+1:] {
			y2 := p2[1]
			if maxY < y2 && y2 <= y1 {
				maxY = y2
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function numberOfPairs(points: number[][]): number {
    points.sort((a, b) => (a[0] === b[0] ? b[1] - a[1] : a[0] - b[0]));
    const n = points.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        const [_, y1] = points[i];
        let maxY = -Infinity;
        for (let j = i + 1; j < n; ++j) {
            const [_, y2] = points[j];
            if (maxY < y2 && y2 <= y1) {
                maxY = y2;
                ++ans;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_pairs(mut points: Vec<Vec<i32>>) -> i32 {
        points.sort_by(|a, b| {
            if a[0] == b[0] {
                b[1].cmp(&a[1])
            } else {
                a[0].cmp(&b[0])
            }
        });

        let n = points.len();
        let mut ans = 0;
        for i in 0..n {
            let y1 = points[i][1];
            let mut max_y = i32::MIN;
            for j in (i + 1)..n {
                let y2 = points[j][1];
                if max_y < y2 && y2 <= y1 {
                    max_y = y2;
                    ans += 1;
                }
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int NumberOfPairs(int[][] points) {
        Array.Sort(points, (a, b) => a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        int ans = 0;
        int n = points.Length;
        int inf = 1 << 30;
        for (int i = 0; i < n; ++i) {
            int y1 = points[i][1];
            int maxY = -inf;
            for (int j = i + 1; j < n; ++j) {
                int y2 = points[j][1];
                if (maxY < y2 && y2 <= y1) {
                    maxY = y2;
                    ++ans;
                }
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
