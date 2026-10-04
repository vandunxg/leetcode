---
comments: true
difficulty: Hard
rating: 2020
source: Biweekly Contest 123 Q4
tags:
    - Geometry
    - Array
    - Math
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [3027. Find the Number of Ways to Place People II](https://leetcode.com/problems/find-the-number-of-ways-to-place-people-ii)

[中文文档](/solution/3000-3099/3027.Find%20the%20Number%20of%20Ways%20to%20Place%20People%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng 2D <code>points</code> có kích thước <code>n x 2</code>, biểu diễn tọa độ nguyên của một số điểm trên mặt phẳng 2D, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.</p>

<p>Ta định nghĩa hướng <strong>phải</strong> là trục x dương (<strong>tọa độ x tăng</strong>) và hướng <strong>trái</strong> là trục x âm (<strong>tọa độ x giảm</strong>). Tương tự, hướng <strong>lên</strong> là trục y dương (<strong>tọa độ y tăng</strong>) và hướng <strong>xuống</strong> là trục y âm (<strong>tọa độ y giảm</strong>).</p>

<p>Bạn phải đặt <code>n</code> người, bao gồm Alice và Bob, tại các điểm này sao cho mỗi điểm có <strong>chính xác một</strong> người. Alice muốn ở riêng với Bob, nên Alice sẽ dựng một hàng rào hình chữ nhật với vị trí của Alice là <strong>góc trên bên trái</strong> và vị trí của Bob là <strong>góc dưới bên phải</strong> của hàng rào (<strong>Lưu ý</strong> rằng hàng rào <strong>có thể không</strong> bao quanh diện tích nào, tức là có thể là một đường thẳng). Nếu có bất kỳ người nào ngoài Alice và Bob ở <strong>bên trong</strong> hoặc <strong>trên</strong> hàng rào, Alice sẽ buồn.</p>

<p>Hãy trả về <em>số lượng <strong>cặp điểm</strong> mà bạn có thể đặt Alice và Bob sao cho Alice <strong>không</strong> buồn khi dựng hàng rào</em>.</p>

<p><strong>Lưu ý</strong> rằng Alice chỉ có thể dựng hàng rào với vị trí của Alice là góc trên bên trái và vị trí của Bob là góc dưới bên phải. Ví dụ, Alice không thể dựng một trong hai hàng rào trong hình dưới đây với bốn góc <code>(1, 1)</code>, <code>(1, 3)</code>, <code>(3, 1)</code> và <code>(3, 3)</code>, vì:</p>

<ul>
	<li>Nếu Alice ở <code>(3, 3)</code> và Bob ở <code>(1, 1)</code>, vị trí của Alice không phải là góc trên bên trái và vị trí của Bob không phải là góc dưới bên phải của hàng rào.</li>
	<li>Nếu Alice ở <code>(1, 3)</code> và Bob ở <code>(1, 1)</code>&nbsp;(với hình chữ nhật được minh họa trong hình thay vì một đường thẳng),&nbsp;vị trí của Bob không phải là góc dưới bên phải của hàng rào.</li>
</ul>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3027.Find%20the%20Number%20of%20Ways%20to%20Place%20People%20II/images/example0alicebob-1.png" style="width: 750px; height: 308px;padding: 10px; background: #fff; border-radius: .5rem;" />
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3027.Find%20the%20Number%20of%20Ways%20to%20Place%20People%20II/images/example1alicebob.png" style="width: 376px; height: 308px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem;" />
<pre>
<strong>Đầu vào:</strong> points = [[1,1],[2,2],[3,3]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có cách nào đặt Alice và Bob sao cho Alice có thể dựng hàng rào với vị trí của Alice là góc trên bên trái và vị trí của Bob là góc dưới bên phải. Vì vậy, ta trả về 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3027.Find%20the%20Number%20of%20Ways%20to%20Place%20People%20II/images/example2alicebob.png" style="width: 1321px; height: 363px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem;" />
<pre>
<strong>Đầu vào:</strong> points = [[6,2],[4,4],[2,6]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có hai cách đặt Alice và Bob sao cho Alice không buồn:
- Đặt Alice ở (4, 4) và Bob ở (6, 2).
- Đặt Alice ở (2, 6) và Bob ở (4, 4).
Không thể đặt Alice ở (2, 6) và Bob ở (6, 2) vì người ở (4, 4) sẽ nằm bên trong hàng rào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3027.Find%20the%20Number%20of%20Ways%20to%20Place%20People%20II/images/example4alicebob.png" style="width: 1123px; height: 308px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem;" />
<pre>
<strong>Đầu vào:</strong> points = [[3,1],[1,3],[1,1]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có hai cách đặt Alice và Bob sao cho Alice không buồn:
- Đặt Alice ở (1, 1) và Bob ở (3, 1).
- Đặt Alice ở (1, 3) và Bob ở (1, 1).
Không thể đặt Alice ở (1, 3) và Bob ở (3, 1) vì người ở (1, 1) sẽ nằm trên hàng rào.
Lưu ý rằng hàng rào có bao quanh diện tích hay không không quan trọng; hàng rào thứ nhất và thứ hai trong hình đều hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>-10<sup>9</sup> &lt;= points[i][0], points[i][1] &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả <code>points[i]</code> đều phân biệt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp và phân loại

<!-- thinking:start -->

> **Tư duy**
>
> Đề bài giống phần I, nhưng $n \le 1000$, nên việc kiểm tra các điểm bên trong với độ phức tạp $O(n^3)$ sẽ bị quá thời gian.
>
> Nhận xét rằng một phép duyệt sau khi sắp xếp cần $y$ tăng nghiêm ngặt không phụ thuộc vào $n$. Việc liệt kê các cặp có độ phức tạp $O(n^2)$ và phù hợp với giới hạn mới.
>
> Ta giữ nguyên cách sắp xếp và phép duyệt với $\textit{maxY}$ và không cần kiểm tra các điểm khác bên trong hình chữ nhật.

<!-- thinking:end -->

Đầu tiên, ta sắp xếp mảng. Sau đó, ta có thể phân loại kết quả dựa trên các tính chất của một tam giác.

- Nếu tổng của hai số nhỏ hơn nhỏ hơn hoặc bằng số lớn nhất, chúng không thể tạo thành một tam giác. Trả về "Invalid".
- Nếu ba số bằng nhau, đó là một tam giác đều. Trả về "Equilateral".
- Nếu có hai số bằng nhau, đó là một tam giác cân. Trả về "Isosceles".
- Nếu không thuộc các trường hợp trên, đó là một tam giác thường. Trả về "Scalene".

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
