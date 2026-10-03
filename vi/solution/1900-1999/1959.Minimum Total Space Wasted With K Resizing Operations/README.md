---
comments: true
difficulty: Medium
rating: 2310
source: Biweekly Contest 58 Q3
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [1959. Minimum Total Space Wasted With K Resizing Operations](https://leetcode.com/problems/minimum-total-space-wasted-with-k-resizing-operations)

[中文文档](/solution/1900-1999/1959.Minimum%20Total%20Space%20Wasted%20With%20K%20Resizing%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang thiết kế một mảng động. Cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>nums</code>, trong đó <code>nums[i]</code> là số phần tử sẽ có trong mảng tại thời điểm <code>i</code>. Ngoài ra, bạn được cho một số nguyên <code>k</code>, là số lần <strong>tối đa</strong> mà bạn có thể <strong>thay đổi kích thước</strong> mảng (thành <strong>bất kỳ</strong> kích thước nào).</p>

<p>Kích thước của mảng tại thời điểm <code>t</code>, <code>size<sub>t</sub></code>, phải lớn hơn hoặc bằng <code>nums[t]</code> vì mảng cần có đủ chỗ để chứa tất cả phần tử. <strong>Dung lượng lãng phí</strong> tại thời điểm <code>t</code> được định nghĩa là <code>size<sub>t</sub> - nums[t]</code>, còn <strong>tổng</strong> dung lượng lãng phí là <strong>tổng</strong> dung lượng lãng phí tại mọi thời điểm <code>t</code> với <code>0 &lt;= t &lt; nums.length</code>.</p>

<p>Hãy trả về <em><strong>nhỏ nhất</strong> <strong>tổng dung lượng lãng phí</strong> nếu bạn có thể thay đổi kích thước mảng nhiều nhất</em> <code>k</code> <em>lần</em>.</p>

<p><strong>Lưu ý:</strong> Mảng có thể có <strong>bất kỳ kích thước nào</strong> lúc ban đầu và kích thước ban đầu <strong>không </strong>được tính vào số lần thay đổi kích thước.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,20], k = 0
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> size = [20,20].
Ta có thể đặt kích thước ban đầu là 20.
Tổng dung lượng lãng phí là (20 - 10) + (20 - 20) = 10.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,20,30], k = 1
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> size = [20,20,30].
Ta có thể đặt kích thước ban đầu là 20 và thay đổi thành 30 tại thời điểm 2.
Tổng dung lượng lãng phí là (20 - 10) + (20 - 20) + (30 - 30) = 10.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,20,15,30,20], k = 2
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> size = [10,20,20,30,30].
Ta có thể đặt kích thước ban đầu là 10, thay đổi thành 20 tại thời điểm 1 và thay đổi thành 30 tại thời điểm 3.
Tổng dung lượng lãng phí là (10 - 10) + (20 - 20) + (20 - 15) + (30 - 30) + (30 - 20) = 15.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 200</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>0 &lt;= k &lt;= nums.length - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> $k$ lần thay đổi kích thước chia mảng thành $k+1$ đoạn; mỗi đoạn lãng phí $\textit{max}\cdot\textit{len}-\textit{sum}$. Việc chọn các điểm cắt có số lượng mũ được thay bằng quy hoạch động vì $n\le 200$.
>
> Tính trước dung lượng lãng phí $g[i][j]$ của mọi đoạn trong $O(n^2)$, sau đó đặt $f[i][j]$ là dung lượng lãng phí nhỏ nhất của $i$ phần tử đầu tiên được chia thành $j$ đoạn và duyệt điểm cắt trước đó.
>
> Đáp án là $f[n][k+1]$.

<!-- thinking:end -->

Bài toán tương đương với việc chia mảng $\textit{nums}$ thành $k + 1$ đoạn. Dung lượng lãng phí của mỗi đoạn bằng giá trị lớn nhất trong đoạn đó nhân với độ dài đoạn, rồi trừ đi tổng các phần tử trong đoạn. Cộng dung lượng lãng phí của tất cả các đoạn, ta được tổng dung lượng lãng phí. Bằng cách cộng 1 vào $k$, thực chất ta chia mảng thành $k$ đoạn.

Do đó, ta định nghĩa mảng $\textit{g}[i][j]$ để biểu diễn dung lượng lãng phí của đoạn $\textit{nums}[i..j]$, bằng giá trị lớn nhất của $\textit{nums}[i..j]$ nhân với độ dài $\textit{nums}[i..j]$, rồi trừ đi tổng các phần tử trong $\textit{nums}[i..j]$. Ta duyệt $i$ trong khoảng $[0, n)$ và $j$ trong khoảng $[i, n)$, dùng biến $s$ để duy trì tổng các phần tử trong $\textit{nums}[i..j]$ và biến $\textit{mx}$ để duy trì giá trị lớn nhất trong $\textit{nums}[i..j]$. Khi đó, ta có:

$$ \textit{g}[i][j] = \textit{mx} \times (j - i + 1) - s $$

Tiếp theo, ta định nghĩa $\textit{f}[i][j]$ là dung lượng lãng phí nhỏ nhất khi chia $i$ phần tử đầu tiên thành $j$ đoạn. Ta khởi tạo $\textit{f}[0][0] = 0$ và các vị trí còn lại bằng vô cùng. Ta duyệt $i$ trong khoảng $[1, n]$ và $j$ trong khoảng $[1, k]$, sau đó duyệt phần tử cuối cùng $h$ của $j - 1$ đoạn trước đó. Khi đó:

$$ \textit{f}[i][j] = \min(\textit{f}[i][j], \textit{f}[h][j - 1] + \textit{g}[h][i - 1]) $$

Đáp án cuối cùng là $\textit{f}[n][k]$.

Độ phức tạp thời gian là $O(n^2 \times k)$, độ phức tạp không gian là $O(n \times (n + k))$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSpaceWastedKResizing(self, nums: List[int], k: int) -> int:
        k += 1
        n = len(nums)
        g = [[0] * n for _ in range(n)]
        for i in range(n):
            s = mx = 0
            for j in range(i, n):
                s += nums[j]
                mx = max(mx, nums[j])
                g[i][j] = mx * (j - i + 1) - s
        f = [[inf] * (k + 1) for _ in range(n + 1)]
        f[0][0] = 0
        for i in range(1, n + 1):
            for j in range(1, k + 1):
                for h in range(i):
                    f[i][j] = min(f[i][j], f[h][j - 1] + g[h][i - 1])
        return f[-1][-1]
```

#### Java

```java
class Solution {
    public int minSpaceWastedKResizing(int[] nums, int k) {
        ++k;
        int n = nums.length;
        int[][] g = new int[n][n];
        for (int i = 0; i < n; ++i) {
            int s = 0, mx = 0;
            for (int j = i; j < n; ++j) {
                s += nums[j];
                mx = Math.max(mx, nums[j]);
                g[i][j] = mx * (j - i + 1) - s;
            }
        }
        int[][] f = new int[n + 1][k + 1];
        int inf = 0x3f3f3f3f;
        for (int i = 0; i < f.length; ++i) {
            Arrays.fill(f[i], inf);
        }
        f[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= k; ++j) {
                for (int h = 0; h < i; ++h) {
                    f[i][j] = Math.min(f[i][j], f[h][j - 1] + g[h][i - 1]);
                }
            }
        }
        return f[n][k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSpaceWastedKResizing(vector<int>& nums, int k) {
        ++k;
        int n = nums.size();
        vector<vector<int>> g(n, vector<int>(n));
        for (int i = 0; i < n; ++i) {
            int s = 0, mx = 0;
            for (int j = i; j < n; ++j) {
                mx = max(mx, nums[j]);
                s += nums[j];
                g[i][j] = mx * (j - i + 1) - s;
            }
        }
        int inf = 0x3f3f3f3f;
        vector<vector<int>> f(n + 1, vector<int>(k + 1, inf));
        f[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= k; ++j) {
                for (int h = 0; h < i; ++h) {
                    f[i][j] = min(f[i][j], f[h][j - 1] + g[h][i - 1]);
                }
            }
        }
        return f[n][k];
    }
};
```

#### Go

```go
func minSpaceWastedKResizing(nums []int, k int) int {
	k++
	n := len(nums)
	g := make([][]int, n)
	for i := range g {
		g[i] = make([]int, n)
	}
	for i := 0; i < n; i++ {
		s, mx := 0, 0
		for j := i; j < n; j++ {
			s += nums[j]
			mx = max(mx, nums[j])
			g[i][j] = mx*(j-i+1) - s
		}
	}
	f := make([][]int, n+1)
	inf := 0x3f3f3f3f
	for i := range f {
		f[i] = make([]int, k+1)
		for j := range f[i] {
			f[i][j] = inf
		}
	}
	f[0][0] = 0
	for i := 1; i <= n; i++ {
		for j := 1; j <= k; j++ {
			for h := 0; h < i; h++ {
				f[i][j] = min(f[i][j], f[h][j-1]+g[h][i-1])
			}
		}
	}
	return f[n][k]
}
```

#### TypeScript

```ts
function minSpaceWastedKResizing(nums: number[], k: number): number {
    k += 1;
    const n = nums.length;
    const g: number[][] = Array.from({ length: n }, () => Array(n).fill(0));

    for (let i = 0; i < n; i++) {
        let s = 0,
            mx = 0;
        for (let j = i; j < n; j++) {
            s += nums[j];
            mx = Math.max(mx, nums[j]);
            g[i][j] = mx * (j - i + 1) - s;
        }
    }

    const inf = Number.POSITIVE_INFINITY;
    const f: number[][] = Array.from({ length: n + 1 }, () => Array(k + 1).fill(inf));
    f[0][0] = 0;

    for (let i = 1; i <= n; i++) {
        for (let j = 1; j <= k; j++) {
            for (let h = 0; h < i; h++) {
                f[i][j] = Math.min(f[i][j], f[h][j - 1] + g[h][i - 1]);
            }
        }
    }

    return f[n][k];
}
```

#### Rust

```rust
impl Solution {
    pub fn min_space_wasted_k_resizing(nums: Vec<i32>, k: i32) -> i32 {
        let mut k = k + 1;
        let n = nums.len();
        let mut g = vec![vec![0; n]; n];

        for i in 0..n {
            let (mut s, mut mx) = (0, 0);
            for j in i..n {
                s += nums[j];
                mx = mx.max(nums[j]);
                g[i][j] = mx * (j as i32 - i as i32 + 1) - s;
            }
        }

        let inf = 0x3f3f3f3f;
        let mut f = vec![vec![inf; (k + 1) as usize]; n + 1];
        f[0][0] = 0;

        for i in 1..=n {
            for j in 1..=k as usize {
                for h in 0..i {
                    f[i][j] = f[i][j].min(f[h][j - 1] + g[h][i - 1]);
                }
            }
        }

        f[n][k as usize]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
