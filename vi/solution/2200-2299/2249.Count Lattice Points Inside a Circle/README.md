---
comments: true
difficulty: Medium
rating: 1602
source: Weekly Contest 290 Q2
tags:
    - Geometry
    - Array
    - Hash Table
    - Math
    - Enumeration
---

<!-- problem:start -->

# [2249. Count Lattice Points Inside a Circle](https://leetcode.com/problems/count-lattice-points-inside-a-circle)

[中文文档](/solution/2200-2299/2249.Count%20Lattice%20Points%20Inside%20a%20Circle/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>circles</code>, trong đó <code>circles[i] = [x<sub>i</sub>, y<sub>i</sub>, r<sub>i</sub>]</code> biểu diễn tâm <code>(x<sub>i</sub>, y<sub>i</sub>)</code> và bán kính <code>r<sub>i</sub></code> của đường tròn thứ <code>i<sup>th</sup></code> được vẽ trên một lưới, hãy trả về <em><strong>số điểm nguyên</strong> </em><em>nằm bên trong <strong>ít nhất một</strong> đường tròn</em>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li><strong>Điểm nguyên</strong> là điểm có tọa độ nguyên.</li>
	<li>Các điểm nằm <strong>trên chu vi đường tròn</strong> cũng được xem là nằm bên trong đường tròn đó.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2249.Count%20Lattice%20Points%20Inside%20a%20Circle/images/exa-11.png" style="width: 300px; height: 300px;" />
<pre>
<strong>Đầu vào:</strong> circles = [[2,2,1]]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Hình trên biểu diễn đường tròn đã cho.
Các điểm nguyên nằm bên trong đường tròn là (1, 2), (2, 1), (2, 2), (2, 3) và (3, 2), được hiển thị bằng màu xanh lá.
Các điểm khác như (1, 1) và (1, 3), được hiển thị bằng màu đỏ, không được xem là nằm bên trong đường tròn.
Vì vậy, số điểm nguyên nằm bên trong ít nhất một đường tròn là 5.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2249.Count%20Lattice%20Points%20Inside%20a%20Circle/images/exa-22.png" style="width: 300px; height: 300px;" />
<pre>
<strong>Đầu vào:</strong> circles = [[2,2,2],[3,4,1]]
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong>
Hình trên biểu diễn các đường tròn đã cho.
Có chính xác 16 điểm nguyên nằm bên trong ít nhất một đường tròn.
Một số điểm trong đó là (0, 2), (2, 0), (2, 4), (3, 2) và (4, 4).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= circles.length &lt;= 200</code></li>
	<li><code>circles[i].length == 3</code></li>
	<li><code>1 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 100</code></li>
	<li><code>1 &lt;= r<sub>i</sub> &lt;= min(x<sub>i</sub>, y<sub>i</sub>)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các điểm nguyên được phủ bởi ít nhất một đường tròn. Có nhiều nhất $200$ đường tròn và tọa độ không vượt quá $100$, nên hình chữ nhật bao quanh là nhỏ. Việc liệt kê các điểm của từng hình tròn rồi khử trùng lặp cần một set; kiểm tra từng điểm nguyên với các đường tròn sẽ đơn giản hơn.
>
> Hình chữ nhật có giới hạn đến $\max(x+r)$ và $\max(y+r)$. Với mỗi $(i,j)$, kiểm tra xem có đường tròn nào thỏa mãn điều kiện khoảng cách bình phương hay không, đếm điểm đó rồi dừng. Độ phức tạp $O(XYn)$ phù hợp với giới hạn đề bài.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countLatticePoints(self, circles: List[List[int]]) -> int:
        ans = 0
        mx = max(x + r for x, _, r in circles)
        my = max(y + r for _, y, r in circles)
        for i in range(mx + 1):
            for j in range(my + 1):
                for x, y, r in circles:
                    dx, dy = i - x, j - y
                    if dx * dx + dy * dy <= r * r:
                        ans += 1
                        break
        return ans
```

#### Java

```java
class Solution {
    public int countLatticePoints(int[][] circles) {
        int mx = 0, my = 0;
        for (var c : circles) {
            mx = Math.max(mx, c[0] + c[2]);
            my = Math.max(my, c[1] + c[2]);
        }
        int ans = 0;
        for (int i = 0; i <= mx; ++i) {
            for (int j = 0; j <= my; ++j) {
                for (var c : circles) {
                    int dx = i - c[0], dy = j - c[1];
                    if (dx * dx + dy * dy <= c[2] * c[2]) {
                        ++ans;
                        break;
                    }
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
    int countLatticePoints(vector<vector<int>>& circles) {
        int mx = 0, my = 0;
        for (auto& c : circles) {
            mx = max(mx, c[0] + c[2]);
            my = max(my, c[1] + c[2]);
        }
        int ans = 0;
        for (int i = 0; i <= mx; ++i) {
            for (int j = 0; j <= my; ++j) {
                for (auto& c : circles) {
                    int dx = i - c[0], dy = j - c[1];
                    if (dx * dx + dy * dy <= c[2] * c[2]) {
                        ++ans;
                        break;
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countLatticePoints(circles [][]int) (ans int) {
	mx, my := 0, 0
	for _, c := range circles {
		mx = max(mx, c[0]+c[2])
		my = max(my, c[1]+c[2])
	}
	for i := 0; i <= mx; i++ {
		for j := 0; j <= my; j++ {
			for _, c := range circles {
				dx, dy := i-c[0], j-c[1]
				if dx*dx+dy*dy <= c[2]*c[2] {
					ans++
					break
				}
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countLatticePoints(circles: number[][]): number {
    let mx = 0;
    let my = 0;
    for (const [x, y, r] of circles) {
        mx = Math.max(mx, x + r);
        my = Math.max(my, y + r);
    }
    let ans = 0;
    for (let i = 0; i <= mx; ++i) {
        for (let j = 0; j <= my; ++j) {
            for (const [x, y, r] of circles) {
                const dx = i - x;
                const dy = j - y;
                if (dx * dx + dy * dy <= r * r) {
                    ++ans;
                    break;
                }
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
