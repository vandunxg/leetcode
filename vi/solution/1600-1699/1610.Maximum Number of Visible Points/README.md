---
comments: true
difficulty: Hard
rating: 2147
source: Weekly Contest 209 Q3
tags:
    - Geometry
    - Array
    - Math
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [1610. Maximum Number of Visible Points](https://leetcode.com/problems/maximum-number-of-visible-points)

[中文文档](/solution/1600-1699/1610.Maximum%20Number%20of%20Visible%20Points/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>points</code>, số nguyên <code>angle</code> và vị trí <code>location</code>, trong đó <code>location = [pos<sub>x</sub>, pos<sub>y</sub>]</code> và <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> đều biểu diễn <strong>tọa độ nguyên</strong> trên mặt phẳng X-Y.</p>

<p>Ban đầu, bạn hướng thẳng về phía đông từ vị trí của mình. Bạn <strong>không thể di chuyển</strong>, nhưng có thể <strong>xoay</strong>. Nói cách khác, <code>pos<sub>x</sub></code> và <code>pos<sub>y</sub></code> không thay đổi. Góc nhìn theo <strong>độ</strong> được biểu diễn bởi <code>angle</code>, xác định phạm vi quan sát theo mỗi hướng. Gọi <code>d</code> là số độ xoay ngược chiều kim đồng hồ. Khi đó, góc nhìn là khoảng <strong>bao gồm hai đầu</strong> <code>[d - angle/2, d + angle/2]</code>.</p>

<p>
<video autoplay="" controls="" height="360" muted="" style="max-width:100%;height:auto;" width="480"><source src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1610.Maximum%20Number%20of%20Visible%20Points/images/angle.mp4" type="video/mp4" />Your browser does not support the video tag or this video format.</video>
</p>

<p>Bạn có thể <strong>nhìn thấy</strong> một tập điểm nếu, với mỗi điểm, <strong>góc</strong> tạo bởi điểm đó, vị trí của bạn và hướng đông ngay tại vị trí của bạn <strong>nằm trong góc nhìn</strong>.</p>

<p>Có thể có nhiều điểm cùng tọa độ. Có thể có điểm nằm ngay tại vị trí của bạn, và bạn luôn nhìn thấy chúng bất kể hướng xoay. Các điểm không che khuất tầm nhìn đến những điểm khác.</p>

<p>Trả về <em>số điểm lớn nhất mà bạn có thể nhìn thấy</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1610.Maximum%20Number%20of%20Visible%20Points/images/89a07e9b-00ab-4967-976a-c723b2aa8656.png" style="width: 400px; height: 300px;" />
<pre>
<strong>Input:</strong> points = [[2,1],[2,2],[3,3]], angle = 90, location = [1,1]
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Vùng tô đậm biểu diễn góc nhìn của bạn. Tất cả điểm đều có thể được nhìn thấy, bao gồm [3,3] dù [2,2] nằm phía trước trên cùng một đường ngắm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> points = [[2,1],[2,2],[3,4],[1,1]], angle = 90, location = [1,1]
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Tất cả điểm đều có thể được nhìn thấy, bao gồm điểm nằm tại vị trí của bạn.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1610.Maximum%20Number%20of%20Visible%20Points/images/5010bfd3-86e6-465f-ac64-e9df941d2e49.png" style="width: 690px; height: 348px;" />
<pre>
<strong>Input:</strong> points = [[1,0],[2,1]], angle = 13, location = [1,1]
<strong>Output:</strong> 1
<strong>Giải thích:</strong> Bạn chỉ có thể nhìn thấy một trong hai điểm như hình trên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 10<sup>5</sup></code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>location.length == 2</code></li>
	<li><code>0 &lt;= angle &lt; 360</code></li>
	<li><code>0 &lt;= pos<sub>x</sub>, pos<sub>y</sub>, x<sub>i</sub>, y<sub>i</sub> &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Vùng nhìn là một cung có góc $\textit{angle}$. Với $10^5$ điểm, thử mọi cặp đầu mút theo góc cực sẽ quá chậm. Các điểm trùng vị trí luôn nhìn thấy được theo mọi hướng nên cần đếm riêng.
>
> Chuyển các điểm còn lại thành góc cực rồi sắp xếp, xem tập điểm nhìn thấy như một cửa sổ tròn có độ rộng $\textit{angle}$. Nhân đôi dãy bằng cách dịch thêm $2\pi$ biến vòng tròn thành mảng tuyến tính.
>
> Với mỗi đầu trái $v[i]$, tìm nhị phân góc phải nhất $\le v[i]+\theta$, lấy kích thước cửa sổ lớn nhất rồi cộng số điểm trùng vị trí.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def visiblePoints(
        self, points: List[List[int]], angle: int, location: List[int]
    ) -> int:
        v = []
        x, y = location
        same = 0
        for xi, yi in points:
            if xi == x and yi == y:
                same += 1
            else:
                v.append(atan2(yi - y, xi - x))
        v.sort()
        n = len(v)
        v += [deg + 2 * pi for deg in v]
        t = angle * pi / 180
        mx = max((bisect_right(v, v[i] + t) - i for i in range(n)), default=0)
        return mx + same
```

#### Java

```java
class Solution {
    public int visiblePoints(List<List<Integer>> points, int angle, List<Integer> location) {
        List<Double> v = new ArrayList<>();
        int x = location.get(0), y = location.get(1);
        int same = 0;
        for (List<Integer> p : points) {
            int xi = p.get(0), yi = p.get(1);
            if (xi == x && yi == y) {
                ++same;
                continue;
            }
            v.add(Math.atan2(yi - y, xi - x));
        }
        Collections.sort(v);
        int n = v.size();
        for (int i = 0; i < n; ++i) {
            v.add(v.get(i) + 2 * Math.PI);
        }
        int mx = 0;
        Double t = angle * Math.PI / 180;
        for (int i = 0, j = 0; j < 2 * n; ++j) {
            while (i < j && v.get(j) - v.get(i) > t) {
                ++i;
            }
            mx = Math.max(mx, j - i + 1);
        }
        return mx + same;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int visiblePoints(vector<vector<int>>& points, int angle, vector<int>& location) {
        vector<double> v;
        int x = location[0], y = location[1];
        int same = 0;
        for (auto& p : points) {
            int xi = p[0], yi = p[1];
            if (xi == x && yi == y)
                ++same;
            else
                v.emplace_back(atan2(yi - y, xi - x));
        }
        sort(v.begin(), v.end());
        int n = v.size();
        for (int i = 0; i < n; ++i) v.emplace_back(v[i] + 2 * M_PI);

        int mx = 0;
        double t = angle * M_PI / 180;
        for (int i = 0, j = 0; j < 2 * n; ++j) {
            while (i < j && v[j] - v[i] > t) ++i;
            mx = max(mx, j - i + 1);
        }
        return mx + same;
    }
};
```

#### Go

```go
func visiblePoints(points [][]int, angle int, location []int) int {
	same := 0
	v := []float64{}
	for _, p := range points {
		if p[0] == location[0] && p[1] == location[1] {
			same++
		} else {
			v = append(v, math.Atan2(float64(p[1]-location[1]), float64(p[0]-location[0])))
		}
	}
	sort.Float64s(v)
	for _, deg := range v {
		v = append(v, deg+2*math.Pi)
	}

	mx := 0
	t := float64(angle) * math.Pi / 180
	for i, j := 0, 0; j < len(v); j++ {
		for i < j && v[j]-v[i] > t {
			i++
		}
		mx = max(mx, j-i+1)
	}
	return same + mx
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
