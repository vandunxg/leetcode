---
comments: true
difficulty: Medium
rating: 1818
source: Biweekly Contest 159 Q2
tags:
    - Greedy
    - Geometry
    - Array
    - Hash Table
    - Math
    - Enumeration
---

<!-- problem:start -->

# [3588. Find Maximum Area of a Triangle](https://leetcode.com/problems/find-maximum-area-of-a-triangle)

[中文文档](/solution/3500-3599/3588.Find%20Maximum%20Area%20of%20a%20Triangle/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng hai chiều <code>coords</code> có kích thước <code>n x 2</code>, biểu diễn tọa độ của <code>n</code> điểm trên một mặt phẳng Cartesian vô hạn.</p>

<p>Hãy tìm <strong>hai lần</strong> <strong>diện tích lớn nhất</strong> của một tam giác có các đỉnh là ba phần tử <em>bất kỳ</em> trong <code>coords</code>, sao cho ít nhất một cạnh của tam giác <strong>song song</strong> với trục x hoặc trục y. Cụ thể, nếu diện tích lớn nhất của tam giác như vậy là <code>A</code>, hãy trả về <code>2 * A</code>.</p>

<p>Nếu không tồn tại tam giác thỏa mãn, trả về -1.</p>

<p><strong>Lưu ý</strong> rằng tam giác <em>không thể</em> có diện tích bằng không.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">coords = [[1,1],[1,2],[3,2],[3,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3588.Find%20Maximum%20Area%20of%20a%20Triangle/images/image-20250420010047-1.png" style="width: 300px; height: 289px;" /></p>

<p>Tam giác trong hình có đáy bằng 1 và chiều cao bằng 2. Do đó, diện tích của nó là <code>1/2 * base * height = 1</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">coords = [[1,1],[2,2],[3,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tam giác duy nhất có thể tạo thành có các đỉnh là <code>(1, 1)</code>, <code>(2, 2)</code> và <code>(3, 3)</code>. Không có cạnh nào của nó song song với trục x hoặc trục y.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == coords.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= coords[i][0], coords[i][1] &lt;= 10<sup>6</sup></code></li>
	<li>Tất cả <code>coords[i]</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Hash Map

<!-- thinking:start -->

> **Tư duy**
>
> Một cạnh phải song song với một trục tọa độ; hai lần diện tích bằng đáy nhân chiều cao. Với đáy thẳng đứng, khoảng cách theo $y$ tại một $x$ cố định là độ dài đáy, còn chiều cao là khoảng cách theo phương ngang tới $x$ nhỏ nhất hoặc lớn nhất trên toàn bộ các điểm.
>
> Dùng hash map để lưu các cực trị $y$ theo từng $x$. Đổi chỗ tọa độ rồi lặp lại cho đáy nằm ngang. Trả về $-1$ nếu diện tích vẫn bằng $0$.

<!-- thinking:end -->

Bài toán yêu cầu hai lần diện tích của tam giác, vì vậy ta có thể trực tiếp tính tích của đáy và chiều cao của tam giác.

Vì tam giác phải có ít nhất một cạnh song song với trục $x$ hoặc trục $y$, ta có thể liệt kê các cạnh song song với trục $x$ và tính diện tích nhân đôi cho mọi tam giác có thể, sau đó đổi chỗ các tọa độ trong $\textit{coords}$ và lặp lại quá trình để tính diện tích nhân đôi cho các tam giác có cạnh song song với trục $y$.

Do đó, ta thiết kế một hàm $\textit{calc}$ để tính diện tích nhân đôi cho mọi tam giác có cạnh song song với trục $y$.

Ta dùng hai hash map $\textit{f}$ và $\textit{g}$ để lưu tọa độ $y$ nhỏ nhất và lớn nhất tương ứng với mỗi tọa độ $x$. Sau đó, ta duyệt qua $\textit{coords}$, cập nhật $\textit{f}$ và $\textit{g}$, đồng thời lưu tọa độ $x$ nhỏ nhất và lớn nhất. Cuối cùng, ta duyệt qua $\textit{f}$, tính diện tích nhân đôi cho mỗi tọa độ $x$ và cập nhật đáp án.

Trong hàm chính, trước tiên ta gọi hàm $\textit{calc}$ để tính diện tích nhân đôi cho các tam giác có cạnh song song với trục $y$, sau đó đổi chỗ các tọa độ trong $\textit{coords}$ và lặp lại quá trình để tính diện tích nhân đôi cho các tam giác có cạnh song song với trục $x$. Cuối cùng, ta trả về đáp án; nếu đáp án bằng 0, ta trả về -1.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $\textit{coords}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxArea(self, coords: List[List[int]]) -> int:
        def calc() -> int:
            mn, mx = inf, 0
            f = {}
            g = {}
            for x, y in coords:
                mn = min(mn, x)
                mx = max(mx, x)
                if x in f:
                    f[x] = min(f[x], y)
                    g[x] = max(g[x], y)
                else:
                    f[x] = g[x] = y
            ans = 0
            for x, y in f.items():
                d = g[x] - y
                ans = max(ans, d * max(mx - x, x - mn))
            return ans

        ans = calc()
        for c in coords:
            c[0], c[1] = c[1], c[0]
        ans = max(ans, calc())
        return ans if ans else -1
```

#### Java

```java
class Solution {
    public long maxArea(int[][] coords) {
        long ans = calc(coords);
        for (int[] c : coords) {
            int tmp = c[0];
            c[0] = c[1];
            c[1] = tmp;
        }
        ans = Math.max(ans, calc(coords));
        return ans > 0 ? ans : -1;
    }

    private long calc(int[][] coords) {
        int mn = Integer.MAX_VALUE, mx = 0;
        Map<Integer, Integer> f = new HashMap<>();
        Map<Integer, Integer> g = new HashMap<>();

        for (int[] c : coords) {
            int x = c[0], y = c[1];
            mn = Math.min(mn, x);
            mx = Math.max(mx, x);
            if (f.containsKey(x)) {
                f.put(x, Math.min(f.get(x), y));
                g.put(x, Math.max(g.get(x), y));
            } else {
                f.put(x, y);
                g.put(x, y);
            }
        }

        long ans = 0;
        for (var e : f.entrySet()) {
            int x = e.getKey();
            int y = e.getValue();
            int d = g.get(x) - y;
            ans = Math.max(ans, (long) d * Math.max(mx - x, x - mn));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxArea(vector<vector<int>>& coords) {
        auto calc = [&]() -> long long {
            int mn = INT_MAX, mx = 0;
            unordered_map<int, int> f, g;
            for (auto& c : coords) {
                int x = c[0], y = c[1];
                mn = min(mn, x);
                mx = max(mx, x);
                if (f.count(x)) {
                    f[x] = min(f[x], y);
                    g[x] = max(g[x], y);
                } else {
                    f[x] = y;
                    g[x] = y;
                }
            }
            long long ans = 0;
            for (auto& [x, y] : f) {
                int d = g[x] - y;
                ans = max(ans, 1LL * d * max(mx - x, x - mn));
            }
            return ans;
        };

        long long ans = calc();
        for (auto& c : coords) {
            swap(c[0], c[1]);
        }
        ans = max(ans, calc());
        return ans > 0 ? ans : -1;
    }
};
```

#### Go

```go
func maxArea(coords [][]int) int64 {
	calc := func() int64 {
		mn, mx := int(1e9), 0
		f := make(map[int]int)
		g := make(map[int]int)
		for _, c := range coords {
			x, y := c[0], c[1]
			mn = min(mn, x)
			mx = max(mx, x)
			if _, ok := f[x]; ok {
				f[x] = min(f[x], y)
				g[x] = max(g[x], y)
			} else {
				f[x] = y
				g[x] = y
			}
		}
		var ans int64
		for x, y := range f {
			d := g[x] - y
			ans = max(ans, int64(d)*int64(max(mx-x, x-mn)))
		}
		return ans
	}

	ans := calc()
	for _, c := range coords {
		c[0], c[1] = c[1], c[0]
	}
	ans = max(ans, calc())
	if ans > 0 {
		return ans
	}
	return -1
}
```

#### TypeScript

```ts
function maxArea(coords: number[][]): number {
    function calc(): number {
        let [mn, mx] = [Infinity, 0];
        const f = new Map<number, number>();
        const g = new Map<number, number>();

        for (const [x, y] of coords) {
            mn = Math.min(mn, x);
            mx = Math.max(mx, x);
            if (f.has(x)) {
                f.set(x, Math.min(f.get(x)!, y));
                g.set(x, Math.max(g.get(x)!, y));
            } else {
                f.set(x, y);
                g.set(x, y);
            }
        }

        let ans = 0;
        for (const [x, y] of f) {
            const d = g.get(x)! - y;
            ans = Math.max(ans, d * Math.max(mx - x, x - mn));
        }
        return ans;
    }

    let ans = calc();
    for (const c of coords) {
        [c[0], c[1]] = [c[1], c[0]];
    }
    ans = Math.max(ans, calc());
    return ans > 0 ? ans : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
