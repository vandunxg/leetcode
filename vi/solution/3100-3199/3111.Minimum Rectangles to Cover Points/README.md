---
comments: true
difficulty: Medium
rating: 1401
source: Biweekly Contest 128 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3111. Minimum Rectangles to Cover Points](https://leetcode.com/problems/minimum-rectangles-to-cover-points)

[中文文档](/solution/3100-3199/3111.Minimum%20Rectangles%20to%20Cover%20Points/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2D <code>points</code>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>. Bạn cũng được cho một số nguyên <code>w</code>. Nhiệm vụ của bạn là <strong>bao phủ</strong> <strong>tất cả</strong> các điểm đã cho bằng các hình chữ nhật.</p>

<p>Mỗi hình chữ nhật có cạnh dưới tại một điểm <code>(x<sub>1</sub>, 0)</code> và cạnh trên tại một điểm <code>(x<sub>2</sub>, y<sub>2</sub>)</code>, trong đó <code>x<sub>1</sub> &lt;= x<sub>2</sub></code>, <code>y<sub>2</sub> &gt;= 0</code>, và điều kiện <code>x<sub>2</sub> - x<sub>1</sub> &lt;= w</code> <strong>bắt buộc</strong> phải được thỏa mãn với mỗi hình chữ nhật.</p>

<p>Một điểm được xem là nằm trong một hình chữ nhật nếu nó nằm bên trong hoặc trên biên của hình chữ nhật đó.</p>

<p>Trả về một số nguyên biểu thị <strong>số lượng tối thiểu</strong> hình chữ nhật cần dùng để mỗi điểm đều được bao phủ bởi <strong>ít nhất một</strong> hình chữ nhật<em>.</em></p>

<p><strong>Lưu ý:</strong> Một điểm có thể được bao phủ bởi nhiều hình chữ nhật.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3111.Minimum%20Rectangles%20to%20Cover%20Points/images/screenshot-from-2024-03-04-20-33-05.png" style="width: 205px; height: 300px;" /></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">points = [[2,1],[1,0],[1,4],[1,8],[3,5],[4,6]], w = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">2</span></p>

<p><strong>Giải thích: </strong></p>

<p>Hình trên minh họa một cách đặt các hình chữ nhật để bao phủ các điểm:</p>

<ul>
	<li>Một hình chữ nhật có cạnh dưới tại <code>(1, 0)</code> và cạnh trên tại <code>(2, 8)</code></li>
	<li>Một hình chữ nhật có cạnh dưới tại <code>(3, 0)</code> và cạnh trên tại <code>(4, 8)</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3111.Minimum%20Rectangles%20to%20Cover%20Points/images/screenshot-from-2024-03-04-18-59-12.png" style="width: 260px; height: 250px;" /></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">points = [[0,0],[1,1],[2,2],[3,3],[4,4],[5,5],[6,6]], w = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">3</span></p>

<p><strong>Giải thích: </strong></p>

<p>Hình trên minh họa một cách đặt các hình chữ nhật để bao phủ các điểm:</p>

<ul>
	<li>Một hình chữ nhật có cạnh dưới tại <code>(0, 0)</code> và cạnh trên tại <code>(2, 2)</code></li>
	<li>Một hình chữ nhật có cạnh dưới tại <code>(3, 0)</code> và cạnh trên tại <code>(5, 5)</code></li>
	<li>Một hình chữ nhật có cạnh dưới tại <code>(6, 0)</code> và cạnh trên tại <code>(6, 6)</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3111.Minimum%20Rectangles%20to%20Cover%20Points/images/screenshot-from-2024-03-04-20-24-03.png" style="height: 150px; width: 127px;" /></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">points = [[2,3],[1,2]], w = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">2</span></p>

<p><strong>Giải thích: </strong></p>

<p>Hình trên minh họa một cách đặt các hình chữ nhật để bao phủ các điểm:</p>

<ul>
	<li>Một hình chữ nhật có cạnh dưới tại <code>(1, 0)</code> và cạnh trên tại <code>(1, 2)</code></li>
	<li>Một hình chữ nhật có cạnh dưới tại <code>(2, 0)</code> và cạnh trên tại <code>(2, 3)</code></li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 10<sup>5</sup></code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>0 &lt;= x<sub>i</sub> == points[i][0] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= y<sub>i</sub> == points[i][1] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= w &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả các cặp <code>(x<sub>i</sub>, y<sub>i</sub>)</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Chiều cao của hình chữ nhật không bị giới hạn, nên việc bao phủ chỉ phụ thuộc vào các tọa độ $x$ và độ rộng $w$. Việc phân các điểm vào các hình chữ nhật bằng cách tìm kiếm sẽ có số trường hợp tăng theo cấp số nhân.
>
> Sau khi sắp xếp theo $x$, mỗi hình chữ nhật nên vươn sang phải xa nhất có thể trong phạm vi $w$. Một điểm nằm ngoài vùng đang được bao phủ sẽ buộc phải tạo một hình chữ nhật mới, với cạnh trái tại chính điểm đó.
>
> Sắp xếp các điểm, duy trì biên phải hiện tại $x_1$, và khi $x>x_1$ thì bắt đầu một hình chữ nhật mới tại $x$ với $x_1=x+w$. Số lần bắt đầu chính là số hình chữ nhật tối thiểu.

<!-- thinking:end -->

Theo mô tả đề bài, chúng ta không cần quan tâm đến chiều cao của các hình chữ nhật, mà chỉ cần xét độ rộng.

Chúng ta có thể sắp xếp tất cả các điểm theo tọa độ x và dùng biến $x_1$ để ghi nhận tọa độ x ngoài cùng bên phải mà hình chữ nhật hiện tại có thể bao phủ. Ban đầu, $x_1 = -1$.

Tiếp theo, chúng ta duyệt qua tất cả các điểm. Nếu tọa độ x của điểm hiện tại $x$ lớn hơn $x_1$, điều đó có nghĩa là hình chữ nhật hiện tại không thể bao phủ điểm này. Chúng ta cần thêm một hình chữ nhật mới, tăng đáp án lên một và cập nhật $x_1 = x + w$.

Sau khi hoàn tất việc duyệt, chúng ta thu được số lượng hình chữ nhật tối thiểu cần dùng.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là số lượng điểm.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minRectanglesToCoverPoints(self, points: List[List[int]], w: int) -> int:
        points.sort()
        ans, x1 = 0, -1
        for x, _ in points:
            if x > x1:
                ans += 1
                x1 = x + w
        return ans
```

#### Java

```java
class Solution {
    public int minRectanglesToCoverPoints(int[][] points, int w) {
        Arrays.sort(points, (a, b) -> a[0] - b[0]);
        int ans = 0, x1 = -1;
        for (int[] p : points) {
            int x = p[0];
            if (x > x1) {
                ++ans;
                x1 = x + w;
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
    int minRectanglesToCoverPoints(vector<vector<int>>& points, int w) {
        sort(points.begin(), points.end());
        int ans = 0, x1 = -1;
        for (const auto& p : points) {
            int x = p[0];
            if (x > x1) {
                ++ans;
                x1 = x + w;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minRectanglesToCoverPoints(points [][]int, w int) (ans int) {
	sort.Slice(points, func(i, j int) bool { return points[i][0] < points[j][0] })
	x1 := -1
	for _, p := range points {
		if x := p[0]; x > x1 {
			ans++
			x1 = x + w
		}
	}
	return
}
```

#### TypeScript

```ts
function minRectanglesToCoverPoints(points: number[][], w: number): number {
    points.sort((a, b) => a[0] - b[0]);
    let [ans, x1] = [0, -1];
    for (const [x, _] of points) {
        if (x > x1) {
            ++ans;
            x1 = x + w;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_rectangles_to_cover_points(mut points: Vec<Vec<i32>>, w: i32) -> i32 {
        points.sort_by(|a, b| a[0].cmp(&b[0]));
        let mut ans = 0;
        let mut x1 = -1;
        for p in points {
            let x = p[0];
            if x > x1 {
                ans += 1;
                x1 = x + w;
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int MinRectanglesToCoverPoints(int[][] points, int w) {
        Array.Sort(points, (a, b) => a[0] - b[0]);
        int ans = 0, x1 = -1;
        foreach (int[] p in points) {
            int x = p[0];
            if (x > x1) {
                ans++;
                x1 = x + w;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
