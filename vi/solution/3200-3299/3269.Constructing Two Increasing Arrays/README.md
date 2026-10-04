---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3269. Constructing Two Increasing Arrays 🔒](https://leetcode.com/problems/constructing-two-increasing-arrays)

[中文文档](/solution/3200-3299/3269.Constructing%20Two%20Increasing%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho 2 mảng số nguyên <code>nums1</code> và <code>nums2</code> chỉ gồm 0 và 1, nhiệm vụ của bạn là tính <strong>giá trị lớn nhất</strong> <strong>nhỏ nhất có thể</strong> trong hai mảng <code>nums1</code> và <code>nums2</code> sau khi thực hiện các thao tác sau.</p>

<p>Thay mỗi 0 bằng một <em>số nguyên dương chẵn</em> và mỗi 1 bằng một <em>số nguyên dương lẻ</em>. Sau khi thay thế, cả hai mảng phải <strong>tăng dần</strong> và mỗi số nguyên chỉ được sử dụng <strong>nhiều nhất</strong> một lần.</p>

<p>Trả về <em>giá trị lớn nhất nhỏ nhất có thể</em> sau khi thực hiện các thay đổi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [], nums2 = [1,0,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi thay thế, <code>nums1 = []</code>, còn <code>nums2 = [1, 2, 3, 5]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [0,1,0,1], nums2 = [1,0,0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách thay thế để phần tử lớn nhất là 9: <code>nums1 = [2, 3, 8, 9]</code>, và <code>nums2 = [1, 4, 6, 7]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [0,1,0,0,1], nums2 = [0,0,0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách thay thế để phần tử lớn nhất là 13: <code>nums1 = [2, 3, 4, 6, 7]</code>, và <code>nums2 = [8, 10, 12, 13]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= nums1.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums2.length &lt;= 1000</code></li>
	<li><code>nums1</code> và <code>nums2</code> chỉ gồm 0 và 1.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Hai mảng phải trở thành các mảng tăng dần với tính chẵn lẻ đã cho, đồng thời tối thiểu hóa giá trị lớn nhất cuối cùng. Vì $m,n\le 1000$, ta có thể nghĩ đến việc thử từng giá trị lớn nhất, nhưng bài toán xây dựng có cấu trúc con tối ưu.
>
> $f[i][j]$ là giá trị lớn nhất nhỏ nhất có thể đạt được sau khi xử lý $i$ giá trị của $nums1$ và $j$ giá trị của $nums2$. Mỗi lần thêm phần tử vào một mảng, ta chọn số nguyên nhỏ nhất lớn hơn giá trị lớn nhất hiện tại và có tính chẵn lẻ phù hợp. Ở biên, ta chỉ có thể tiếp tục một mảng; ở bên trong, ta chọn phương án tốt hơn trong hai cách thêm phần tử.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là giá trị nhỏ nhất của phần tử lớn nhất trong $i$ phần tử đầu tiên của mảng $\textit{nums1}$ và $j$ phần tử đầu tiên của mảng $\textit{nums2}$. Ban đầu, $f[i][j] = 0$, và đáp án là $f[m][n]$, trong đó $m$ và $n$ lần lượt là độ dài của hai mảng $\textit{nums1}$ và $\textit{nums2}$.

Nếu $j = 0$, giá trị của $f[i][0]$ chỉ có thể được suy ra từ $f[i - 1][0]$, với công thức chuyển $f[i][0] = \textit{nxt}(f[i - 1][0], \textit{nums1}[i - 1])$, trong đó $\textit{nxt}(x, y)$ biểu diễn số nguyên nhỏ nhất lớn hơn $x$ và có cùng tính chẵn lẻ với $y$.

Nếu $i = 0$, giá trị của $f[0][j]$ chỉ có thể được suy ra từ $f[0][j - 1]$, với công thức chuyển $f[0][j] = \textit{nxt}(f[0][j - 1], \textit{nums2}[j - 1])$.

Nếu $i > 0$ và $j > 0$, giá trị của $f[i][j]$ có thể được suy ra từ cả $f[i - 1][j]$ và $f[i][j - 1]$, với công thức chuyển $f[i][j] = \min(\textit{nxt}(f[i - 1][j], \textit{nums1}[i - 1]), \textit{nxt}(f[i][j - 1], \textit{nums2}[j - 1]))$.

Cuối cùng, trả về $f[m][n]$.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là độ dài của hai mảng $\textit{nums1}$ và $\textit{nums2}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minLargest(self, nums1: List[int], nums2: List[int]) -> int:
        def nxt(x: int, y: int) -> int:
            return x + 1 if (x & 1 ^ y) == 1 else x + 2

        m, n = len(nums1), len(nums2)
        f = [[0] * (n + 1) for _ in range(m + 1)]
        for i, x in enumerate(nums1, 1):
            f[i][0] = nxt(f[i - 1][0], x)
        for j, y in enumerate(nums2, 1):
            f[0][j] = nxt(f[0][j - 1], y)
        for i, x in enumerate(nums1, 1):
            for j, y in enumerate(nums2, 1):
                f[i][j] = min(nxt(f[i - 1][j], x), nxt(f[i][j - 1], y))
        return f[m][n]
```

#### Java

```java
class Solution {
    public int minLargest(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int[][] f = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            f[i][0] = nxt(f[i - 1][0], nums1[i - 1]);
        }
        for (int j = 1; j <= n; ++j) {
            f[0][j] = nxt(f[0][j - 1], nums2[j - 1]);
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                int x = nxt(f[i - 1][j], nums1[i - 1]);
                int y = nxt(f[i][j - 1], nums2[j - 1]);
                f[i][j] = Math.min(x, y);
            }
        }
        return f[m][n];
    }

    private int nxt(int x, int y) {
        return (x & 1 ^ y) == 1 ? x + 1 : x + 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minLargest(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        int f[m + 1][n + 1];
        memset(f, 0, sizeof(f));
        auto nxt = [](int x, int y) -> int {
            return (x & 1 ^ y) == 1 ? x + 1 : x + 2;
        };
        for (int i = 1; i <= m; ++i) {
            f[i][0] = nxt(f[i - 1][0], nums1[i - 1]);
        }
        for (int j = 1; j <= n; ++j) {
            f[0][j] = nxt(f[0][j - 1], nums2[j - 1]);
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                int x = nxt(f[i - 1][j], nums1[i - 1]);
                int y = nxt(f[i][j - 1], nums2[j - 1]);
                f[i][j] = min(x, y);
            }
        }
        return f[m][n];
    }
};
```

#### Go

```go
func minLargest(nums1 []int, nums2 []int) int {
	m, n := len(nums1), len(nums2)
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	nxt := func(x, y int) int {
		if (x&1 ^ y) == 1 {
			return x + 1
		}
		return x + 2
	}
	for i := 1; i <= m; i++ {
		f[i][0] = nxt(f[i-1][0], nums1[i-1])
	}
	for j := 1; j <= n; j++ {
		f[0][j] = nxt(f[0][j-1], nums2[j-1])
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			x := nxt(f[i-1][j], nums1[i-1])
			y := nxt(f[i][j-1], nums2[j-1])
			f[i][j] = min(x, y)
		}
	}
	return f[m][n]
}
```

#### TypeScript

```ts
function minLargest(nums1: number[], nums2: number[]): number {
    const m = nums1.length;
    const n = nums2.length;
    const f: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    const nxt = (x: number, y: number): number => {
        return (x & 1) ^ y ? x + 1 : x + 2;
    };
    for (let i = 1; i <= m; ++i) {
        f[i][0] = nxt(f[i - 1][0], nums1[i - 1]);
    }
    for (let j = 1; j <= n; ++j) {
        f[0][j] = nxt(f[0][j - 1], nums2[j - 1]);
    }
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            f[i][j] = Math.min(nxt(f[i - 1][j], nums1[i - 1]), nxt(f[i][j - 1], nums2[j - 1]));
        }
    }
    return f[m][n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
