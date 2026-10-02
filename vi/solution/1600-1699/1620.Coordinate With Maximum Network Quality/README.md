---
comments: true
difficulty: Medium
rating: 1665
source: Biweekly Contest 37 Q2
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [1620. Coordinate With Maximum Network Quality](https://leetcode.com/problems/coordinate-with-maximum-network-quality)

[中文文档](/solution/1600-1699/1620.Coordinate%20With%20Maximum%20Network%20Quality/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng các trạm phát sóng <code>towers</code>, trong đó <code>towers[i] = [x<sub>i</sub>, y<sub>i</sub>, q<sub>i</sub>]</code> biểu diễn trạm phát sóng thứ <code>i<sup>th</sup></code> tại vị trí <code>(x<sub>i</sub>, y<sub>i</sub>)</code> với hệ số chất lượng <code>q<sub>i</sub></code>. Tọa độ đều là <strong>tọa độ nguyên</strong> trên mặt phẳng X-Y, và khoảng cách giữa hai tọa độ là <strong>khoảng cách Euclid</strong>.</p>

<p>Cho số nguyên <code>radius</code>, một trạm được xem là <strong>có thể tiếp cận</strong> nếu khoảng cách <strong>nhỏ hơn hoặc bằng</strong> <code>radius</code>. Ngoài khoảng cách đó, tín hiệu bị nhiễu và trạm <strong>không thể tiếp cận</strong>.</p>

<p>Chất lượng tín hiệu của trạm thứ <code>i<sup>th</sup></code> tại tọa độ <code>(x, y)</code> được tính theo công thức <code>&lfloor;q<sub>i</sub> / (1 + d)&rfloor;</code>, trong đó <code>d</code> là khoảng cách giữa trạm và tọa độ. <strong>Chất lượng mạng</strong> tại một tọa độ là tổng chất lượng tín hiệu từ tất cả các trạm <strong>có thể tiếp cận</strong>.</p>

<p>Trả về <em>mảng </em><code>[c<sub>x</sub>, c<sub>y</sub>]</code><em> biểu diễn tọa độ <strong>nguyên</strong> </em><code>(c<sub>x</sub>, c<sub>y</sub>)</code><em> có <strong>chất lượng mạng</strong> lớn nhất. Nếu có nhiều tọa độ có cùng <strong>chất lượng mạng</strong>, trả về tọa độ <strong>không âm</strong> nhỏ nhất theo thứ tự từ điển.</em></p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Tọa độ <code>(x1, y1)</code> nhỏ hơn <code>(x2, y2)</code> theo thứ tự từ điển nếu:

    <ul>
     	<li><code>x1 &lt; x2</code>, hoặc</li>
     	<li><code>x1 == x2</code> và <code>y1 &lt; y2</code>.</li>
    </ul>
    </li>
    <li><code>&lfloor;val&rfloor;</code> là số nguyên lớn nhất nhỏ hơn hoặc bằng <code>val</code> (hàm floor).</li>

</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1620.Coordinate%20With%20Maximum%20Network%20Quality/images/untitled-diagram.png" style="width: 176px; height: 176px;" />
<pre>
<strong>Input:</strong> towers = [[1,2,5],[2,1,7],[3,1,9]], radius = 2
<strong>Output:</strong> [2,1]
<strong>Explanation:</strong> Tại tọa độ (2, 1), tổng chất lượng là 13.
- Quality of 7 from (2, 1) results in &lfloor;7 / (1 + sqrt(0)&rfloor; = &lfloor;7&rfloor; = 7
- Quality of 5 from (1, 2) results in &lfloor;5 / (1 + sqrt(2)&rfloor; = &lfloor;2.07&rfloor; = 2
- Quality of 9 from (3, 1) results in &lfloor;9 / (1 + sqrt(1)&rfloor; = &lfloor;4.5&rfloor; = 4
No other coordinate has a higher network quality.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> towers = [[23,11,21]], radius = 9
<strong>Output:</strong> [23,11]
<strong>Explanation:</strong> Vì chỉ có một trạm, chất lượng mạng cao nhất ngay tại vị trí của trạm.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> towers = [[1,2,13],[2,1,7],[0,1,9]], radius = 2
<strong>Output:</strong> [1,2]
<strong>Explanation:</strong> Tọa độ (1, 2) có chất lượng mạng cao nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= towers.length &lt;= 50</code></li>
	<li><code>towers[i].length == 3</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub>, q<sub>i</sub> &lt;= 50</code></li>
	<li><code>1 &lt;= radius &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều nhất $50$ trạm và tọa độ thuộc $[0,50]$, nên chỉ có $51 \times 51$ điểm ứng viên; duyệt ba vòng lặp để tính chất lượng là đủ.
>
> Một trạm đóng góp $0$ khi vượt quá $\textit{radius}$, còn nếu không thì đóng góp $\lfloor q/(1+d) \rfloor$.
>
> Tính tổng đóng góp của mọi trạm tại từng điểm nguyên và giữ lại chất lượng lớn nhất, dùng thứ tự từ điển để phá hòa. Bắt đầu từ $(0,0)$ sẽ cho đúng đáp án khi mọi chất lượng đều bằng không.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def bestCoordinate(self, towers: List[List[int]], radius: int) -> List[int]:
        mx = 0
        ans = [0, 0]
        for i in range(51):
            for j in range(51):
                t = 0
                for x, y, q in towers:
                    d = ((x - i) ** 2 + (y - j) ** 2) ** 0.5
                    if d <= radius:
                        t += floor(q / (1 + d))
                if t > mx:
                    mx = t
                    ans = [i, j]
        return ans
```

#### Java

```java
class Solution {
    public int[] bestCoordinate(int[][] towers, int radius) {
        int mx = 0;
        int[] ans = new int[] {0, 0};
        for (int i = 0; i < 51; ++i) {
            for (int j = 0; j < 51; ++j) {
                int t = 0;
                for (var e : towers) {
                    double d = Math.sqrt((i - e[0]) * (i - e[0]) + (j - e[1]) * (j - e[1]));
                    if (d <= radius) {
                        t += Math.floor(e[2] / (1 + d));
                    }
                }
                if (mx < t) {
                    mx = t;
                    ans = new int[] {i, j};
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
    vector<int> bestCoordinate(vector<vector<int>>& towers, int radius) {
        int mx = 0;
        vector<int> ans = {0, 0};
        for (int i = 0; i < 51; ++i) {
            for (int j = 0; j < 51; ++j) {
                int t = 0;
                for (auto& e : towers) {
                    double d = sqrt((i - e[0]) * (i - e[0]) + (j - e[1]) * (j - e[1]));
                    if (d <= radius) {
                        t += floor(e[2] / (1 + d));
                    }
                }
                if (mx < t) {
                    mx = t;
                    ans = {i, j};
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func bestCoordinate(towers [][]int, radius int) []int {
	ans := []int{0, 0}
	mx := 0
	for i := 0; i < 51; i++ {
		for j := 0; j < 51; j++ {
			t := 0
			for _, e := range towers {
				d := math.Sqrt(float64((i-e[0])*(i-e[0]) + (j-e[1])*(j-e[1])))
				if d <= float64(radius) {
					t += int(float64(e[2]) / (1 + d))
				}
			}
			if mx < t {
				mx = t
				ans = []int{i, j}
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
