---
comments: true
difficulty: Medium
rating: 1954
source: Weekly Contest 456 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3599. Partition Array to Minimize XOR](https://leetcode.com/problems/partition-array-to-minimize-xor)

[中文文档](/solution/3500-3599/3599.Partition%20Array%20to%20Minimize%20XOR/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Nhiệm vụ của bạn là chia <code>nums</code> thành <code>k</code><strong> </strong><strong><span data-keyword="subarray-nonempty">mảng con không rỗng</span></strong>. Với mỗi mảng con, hãy tính phép <strong>XOR</strong> bit của tất cả các phần tử trong đó.</p>

<p>Trả về giá trị <strong>nhỏ nhất</strong> có thể có của <strong>XOR lớn nhất</strong> trong số <code>k</code> mảng con này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cách chia tối ưu là <code>[1]</code> và <code>[2, 3]</code>.</p>

<ul>
	<li>XOR của mảng con thứ nhất là <code>1</code>.</li>
	<li>XOR của mảng con thứ hai là <code>2 XOR 3 = 1</code>.</li>
</ul>

<p>XOR lớn nhất trong các mảng con là 1, đây là giá trị nhỏ nhất có thể.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,3,2], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cách chia tối ưu là <code>[2]</code>, <code>[3, 3]</code> và <code>[2]</code>.</p>

<ul>
	<li>XOR của mảng con thứ nhất là <code>2</code>.</li>
	<li>XOR của mảng con thứ hai là <code>3 XOR 3 = 0</code>.</li>
	<li>XOR của mảng con thứ ba là <code>2</code>.</li>
</ul>

<p>XOR lớn nhất trong các mảng con là 2, đây là giá trị nhỏ nhất có thể.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,2,3,1], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cách chia tối ưu là <code>[1, 1]</code> và <code>[2, 3, 1]</code>.</p>

<ul>
	<li>XOR của mảng con thứ nhất là <code>1 XOR 1 = 0</code>.</li>
	<li>XOR của mảng con thứ hai là <code>2 XOR 3 XOR 1 = 0</code>.</li>
</ul>

<p>XOR lớn nhất trong các mảng con là 0, đây là giá trị nhỏ nhất có thể.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 250</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Chia mảng thành đúng $k$ mảng con và tối thiểu hóa XOR lớn nhất của một mảng con. Vì $n$ và $k$ không quá lớn, ta có thể dùng $f[i][j]$ — giá trị XOR lớn nhất nhỏ nhất khi dùng $j$ phần để chia $i$ phần tử đầu tiên.
>
> XOR tiền tố $g[i]$ giúp tính XOR của đoạn $[h+1,i]$ bằng $g[i]\oplus g[h]$. Ta duyệt vị trí cắt trước đó $h$ và lấy $\min_h \max(f[h][j-1], g[i]\oplus g[h])$. Đáp án là $f[n][k]$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là giá trị nhỏ nhất có thể có của XOR lớn nhất trong tất cả các cách chia $i$ phần tử đầu tiên thành $j$ mảng con. Ban đầu, đặt $f[0][0] = 0$, còn các trạng thái khác được đặt là $f[i][j] = +\infty$.

Để tính nhanh XOR của một mảng con, ta có thể sử dụng mảng XOR tiền tố $g$, trong đó $g[i]$ biểu diễn XOR của $i$ phần tử đầu tiên. Với mảng con $[h + 1...i]$ (các chỉ số bắt đầu từ $1$), giá trị XOR của nó là $g[i] \oplus g[h]$.

Tiếp theo, ta duyệt $i$ từ $1$ đến $n$, $j$ từ $1$ đến $\min(i, k)$ và $h$ từ $j - 1$ đến $i - 1$, trong đó $h$ là vị trí kết thúc của mảng con trước đó (các chỉ số bắt đầu từ $1$). Ta cập nhật $f[i][j]$ theo công thức chuyển trạng thái sau:

$$
f[i][j] = \min_{h \in [j - 1, i - 1]} \max(f[h][j - 1], g[i] \oplus g[h])
$$

Cuối cùng, trả về $f[n][k]$, đây là giá trị nhỏ nhất có thể có của XOR lớn nhất khi chia toàn bộ mảng thành $k$ mảng con.

Độ phức tạp thời gian là $O(n^2 \times k)$, và độ phức tạp không gian là $O(n \times k)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
min = lambda a, b: a if a < b else b
max = lambda a, b: a if a > b else b


class Solution:
    def minXor(self, nums: List[int], k: int) -> int:
        n = len(nums)
        g = [0] * (n + 1)
        for i, x in enumerate(nums, 1):
            g[i] = g[i - 1] ^ x

        f = [[inf] * (k + 1) for _ in range(n + 1)]
        f[0][0] = 0
        for i in range(1, n + 1):
            for j in range(1, min(i, k) + 1):
                for h in range(j - 1, i):
                    f[i][j] = min(f[i][j], max(f[h][j - 1], g[i] ^ g[h]))
        return f[n][k]
```

#### Java

```java
class Solution {
    public int minXor(int[] nums, int k) {
        int n = nums.length;
        int[] g = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            g[i] = g[i - 1] ^ nums[i - 1];
        }

        int[][] f = new int[n + 1][k + 1];
        for (int i = 0; i <= n; ++i) {
            Arrays.fill(f[i], Integer.MAX_VALUE);
        }
        f[0][0] = 0;

        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= Math.min(i, k); ++j) {
                for (int h = j - 1; h < i; ++h) {
                    f[i][j] = Math.min(f[i][j], Math.max(f[h][j - 1], g[i] ^ g[h]));
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
    int minXor(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> g(n + 1);
        for (int i = 1; i <= n; ++i) {
            g[i] = g[i - 1] ^ nums[i - 1];
        }

        const int inf = numeric_limits<int>::max();
        vector f(n + 1, vector(k + 1, inf));
        f[0][0] = 0;

        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= min(i, k); ++j) {
                for (int h = j - 1; h < i; ++h) {
                    f[i][j] = min(f[i][j], max(f[h][j - 1], g[i] ^ g[h]));
                }
            }
        }

        return f[n][k];
    }
};
```

#### Go

```go
func minXor(nums []int, k int) int {
	n := len(nums)
	g := make([]int, n+1)
	for i := 1; i <= n; i++ {
		g[i] = g[i-1] ^ nums[i-1]
	}

	const inf = math.MaxInt32
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, k+1)
		for j := range f[i] {
			f[i][j] = inf
		}
	}
	f[0][0] = 0

	for i := 1; i <= n; i++ {
		for j := 1; j <= min(i, k); j++ {
			for h := j - 1; h < i; h++ {
				f[i][j] = min(f[i][j], max(f[h][j-1], g[i]^g[h]))
			}
		}
	}

	return f[n][k]
}
```

#### TypeScript

```ts
function minXor(nums: number[], k: number): number {
    const n = nums.length;
    const g: number[] = Array(n + 1).fill(0);
    for (let i = 1; i <= n; ++i) {
        g[i] = g[i - 1] ^ nums[i - 1];
    }

    const inf = Number.MAX_SAFE_INTEGER;
    const f: number[][] = Array.from({ length: n + 1 }, () => Array(k + 1).fill(inf));
    f[0][0] = 0;

    for (let i = 1; i <= n; ++i) {
        for (let j = 1; j <= Math.min(i, k); ++j) {
            for (let h = j - 1; h < i; ++h) {
                f[i][j] = Math.min(f[i][j], Math.max(f[h][j - 1], g[i] ^ g[h]));
            }
        }
    }

    return f[n][k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
