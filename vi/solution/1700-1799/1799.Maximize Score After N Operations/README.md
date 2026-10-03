---
comments: true
difficulty: Hard
rating: 2072
source: Biweekly Contest 48 Q4
tags:
    - Bit Manipulation
    - Array
    - Math
    - Dynamic Programming
    - Backtracking
    - Bitmask
    - Number Theory
---

<!-- problem:start -->

# [1799. Maximize Score After N Operations](https://leetcode.com/problems/maximize-score-after-n-operations)

[中文文档](/solution/1700-1799/1799.Maximize%20Score%20After%20N%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>nums</code>, một mảng số nguyên dương có kích thước <code>2 * n</code>. Bạn phải thực hiện <code>n</code> thao tác trên mảng này.</p>

<p>Trong thao tác thứ <code>i<sup>th</sup></code> <strong>(đánh chỉ số từ 1)</strong>, bạn sẽ:</p>

<ul>
	<li>Chọn hai phần tử <code>x</code> và <code>y</code>.</li>
	<li>Nhận điểm số <code>i * gcd(x, y)</code>.</li>
	<li>Xóa <code>x</code> và <code>y</code> khỏi <code>nums</code>.</li>
</ul>

<p>Trả về <em>điểm số lớn nhất có thể nhận được sau khi thực hiện </em><code>n</code><em> thao tác.</em></p>

<p>Hàm <code>gcd(x, y)</code> là ước chung lớn nhất của <code>x</code> và <code>y</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2]
<strong>Output:</strong> 1
<strong>Giải thích:</strong>&nbsp;Lựa chọn thao tác tối ưu là:
(1 * gcd(1, 2)) = 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [3,4,6,8]
<strong>Output:</strong> 11
<strong>Giải thích:</strong>&nbsp;Lựa chọn thao tác tối ưu là:
(1 * gcd(3, 6)) + (2 * gcd(4, 8)) = 3 + 8 = 11
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3,4,5,6]
<strong>Output:</strong> 14
<strong>Giải thích:</strong>&nbsp;Lựa chọn thao tác tối ưu là:
(1 * gcd(1, 5)) + (2 * gcd(2, 4)) + (3 * gcd(3, 6)) = 1 + 4 + 9 = 14
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 7</code></li>
	<li><code>nums.length == 2 * n</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nén trạng thái + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước chọn hai số còn lại và nhận $i\cdot\gcd$ ở bước thứ $i$. Vì $n\le 7$ nên $m=2n\le 14$, và DP trên tập con có thể thử các cách ghép cặp.
>
> Tính trước $\gcd$ của từng cặp. $f[k]$ là điểm tốt nhất khi dùng các phần tử trong mask $k$. Khi popcount là số chẵn, thử bỏ một cặp $i,j$ và cộng $\textit{cnt}/2\cdot g[i][j]$.
>
> Đáp án là $f[2^m-1]$ với mask đầy đủ.

<!-- thinking:end -->

Ta có thể tiền xử lý ước chung lớn nhất của mọi cặp số trong mảng `nums`, lưu vào mảng hai chiều $g$, trong đó $g[i][j]$ là ước chung lớn nhất của $nums[i]$ và $nums[j]$.

Sau đó định nghĩa $f[k]$ là điểm số lớn nhất có thể đạt được khi trạng thái sau thao tác hiện tại là $k$. Gọi $m$ là số phần tử của mảng `nums`, có tổng cộng $2^m$ trạng thái, tức $k$ nằm trong $[0, 2^m - 1]$.

Duyệt mọi trạng thái từ nhỏ đến lớn. Với mỗi trạng thái $k$, trước hết xác định số bit $1$ trong biểu diễn nhị phân, ký hiệu là $cnt$, có phải số chẵn không. Nếu có, thực hiện:

Duyệt các vị trí có bit bằng 1 trong $k$, giả sử là $i$ và $j$, rồi cho hai phần tử ở vị trí $i$ và $j$ thực hiện một thao tác. Điểm nhận được là $\frac{cnt}{2} \times g[i][j]$, từ đó cập nhật giá trị lớn nhất của $f[k]$.

Đáp án cuối cùng là $f[2^m - 1]$.

Độ phức tạp thời gian là $O(2^m \times m^2)$ và độ phức tạp không gian là $O(2^m)$. Ở đây, $m$ là số phần tử trong mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, nums: List[int]) -> int:
        m = len(nums)
        f = [0] * (1 << m)
        g = [[0] * m for _ in range(m)]
        for i in range(m):
            for j in range(i + 1, m):
                g[i][j] = gcd(nums[i], nums[j])
        for k in range(1 << m):
            if (cnt := k.bit_count()) % 2 == 0:
                for i in range(m):
                    if k >> i & 1:
                        for j in range(i + 1, m):
                            if k >> j & 1:
                                f[k] = max(
                                    f[k],
                                    f[k ^ (1 << i) ^ (1 << j)] + cnt // 2 * g[i][j],
                                )
        return f[-1]
```

#### Java

```java
class Solution {
    public int maxScore(int[] nums) {
        int m = nums.length;
        int[][] g = new int[m][m];
        for (int i = 0; i < m; ++i) {
            for (int j = i + 1; j < m; ++j) {
                g[i][j] = gcd(nums[i], nums[j]);
            }
        }
        int[] f = new int[1 << m];
        for (int k = 0; k < 1 << m; ++k) {
            int cnt = Integer.bitCount(k);
            if (cnt % 2 == 0) {
                for (int i = 0; i < m; ++i) {
                    if (((k >> i) & 1) == 1) {
                        for (int j = i + 1; j < m; ++j) {
                            if (((k >> j) & 1) == 1) {
                                f[k] = Math.max(
                                    f[k], f[k ^ (1 << i) ^ (1 << j)] + cnt / 2 * g[i][j]);
                            }
                        }
                    }
                }
            }
        }
        return f[(1 << m) - 1];
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxScore(vector<int>& nums) {
        int m = nums.size();
        int g[m][m];
        for (int i = 0; i < m; ++i) {
            for (int j = i + 1; j < m; ++j) {
                g[i][j] = gcd(nums[i], nums[j]);
            }
        }
        int f[1 << m];
        memset(f, 0, sizeof f);
        for (int k = 0; k < 1 << m; ++k) {
            int cnt = __builtin_popcount(k);
            if (cnt % 2 == 0) {
                for (int i = 0; i < m; ++i) {
                    if (k >> i & 1) {
                        for (int j = i + 1; j < m; ++j) {
                            if (k >> j & 1) {
                                f[k] = max(f[k], f[k ^ (1 << i) ^ (1 << j)] + cnt / 2 * g[i][j]);
                            }
                        }
                    }
                }
            }
        }
        return f[(1 << m) - 1];
    }
};
```

#### Go

```go
func maxScore(nums []int) int {
	m := len(nums)
	g := [14][14]int{}
	for i := 0; i < m; i++ {
		for j := i + 1; j < m; j++ {
			g[i][j] = gcd(nums[i], nums[j])
		}
	}
	f := make([]int, 1<<m)
	for k := 0; k < 1<<m; k++ {
		cnt := bits.OnesCount(uint(k))
		if cnt%2 == 0 {
			for i := 0; i < m; i++ {
				if k>>i&1 == 1 {
					for j := i + 1; j < m; j++ {
						if k>>j&1 == 1 {
							f[k] = max(f[k], f[k^(1<<i)^(1<<j)]+cnt/2*g[i][j])
						}
					}
				}
			}
		}
	}
	return f[1<<m-1]
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### TypeScript

```ts
function maxScore(nums: number[]): number {
    const m = nums.length;
    const f: number[] = new Array(1 << m).fill(0);
    const g: number[][] = new Array(m).fill(0).map(() => new Array(m).fill(0));
    for (let i = 0; i < m; ++i) {
        for (let j = i + 1; j < m; ++j) {
            g[i][j] = gcd(nums[i], nums[j]);
        }
    }
    for (let k = 0; k < 1 << m; ++k) {
        const cnt = bitCount(k);
        if (cnt % 2 === 0) {
            for (let i = 0; i < m; ++i) {
                if ((k >> i) & 1) {
                    for (let j = i + 1; j < m; ++j) {
                        if ((k >> j) & 1) {
                            const t = f[k ^ (1 << i) ^ (1 << j)] + ~~(cnt / 2) * g[i][j];
                            f[k] = Math.max(f[k], t);
                        }
                    }
                }
            }
        }
    }
    return f[(1 << m) - 1];
}

function gcd(a: number, b: number): number {
    return b ? gcd(b, a % b) : a;
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
