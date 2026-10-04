---
comments: true
difficulty: Hard
rating: 2545
source: Biweekly Contest 139 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3287. Find the Maximum Sequence Value of Array](https://leetcode.com/problems/find-the-maximum-sequence-value-of-array)

[中文文档](/solution/3200-3299/3287.Find%20the%20Maximum%20Sequence%20Value%20of%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <strong>dương</strong> <code>k</code>.</p>

<p><strong>Giá trị</strong> của một dãy <code>seq</code> có kích thước <code>2 * x</code> được định nghĩa là:</p>

<ul>
	<li><code>(seq[0] OR seq[1] OR ... OR seq[x - 1]) XOR (seq[x] OR seq[x + 1] OR ... OR seq[2 * x - 1])</code>.</li>
</ul>

<p>Trả về <strong>giá trị</strong> <strong>lớn nhất</strong> của bất kỳ <span data-keyword="subsequence-array">dãy con</span> nào của <code>nums</code> có kích thước <code>2 * k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,6,7], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con <code>[2, 7]</code> có giá trị lớn nhất là <code>2 XOR 7 = 5</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,2,5,6,7], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con <code>[4, 5, 6, 7]</code> có giá trị lớn nhất là <code>(4 OR 5) XOR (6 OR 7) = 2</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 400</code></li>
	<li><code>1 &lt;= nums[i] &lt; 2<sup>7</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length / 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Phân rã tiền tố và hậu tố + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Chọn $k$ giá trị từ một tiền tố và $k$ giá trị từ hậu tố còn lại; tối đa hóa OR XOR OR. Vì $n\le 400$, $k\le n/2$ và các giá trị $<128$, quy hoạch động subset-OR tại một điểm chia là khả thi.
>
> $f[i][j][x]$ cho biết liệu có thể OR thành $x$ khi chọn $j$ phần tử trong $i$ phần tử đầu tiên hay không; $g$ là trạng thái tương tự cho hậu tố. Tại mỗi điểm chia $i\in[k,n-k]$, liệt kê các $x,y$ có thể đạt được và cập nhật $x\oplus y$. Không gian $2^7$ giúp các bảng luôn có kích thước nhỏ.

<!-- thinking:end -->

Ta chia dãy thành hai phần, gồm $k$ phần tử đầu tiên và $k$ phần tử cuối cùng, rồi tính tất cả các giá trị XOR có thể đạt được từ tiền tố và hậu tố.

Định nghĩa $f[i][j][x]$ biểu thị liệu có tồn tại một subset nhận được giá trị XOR bằng $x$ bằng cách chọn $j$ phần tử trong $i$ phần tử đầu tiên hay không. Định nghĩa $g[i][j][y]$ biểu thị liệu có tồn tại một subset nhận được giá trị XOR bằng $y$ bằng cách chọn $j$ phần tử bắt đầu từ chỉ số $i$ hay không.

Xét công thức chuyển của $f[i][j][x]$. Với phần tử thứ $i$ (bắt đầu từ $0$), ta có thể chọn hoặc không chọn phần tử đó, nên:

$$
f[i + 1][j][x] = f[i + 1][j][x] \lor f[i][j][x] \\
f[i + 1][j + 1][x \lor \text{nums}[i]] = f[i + 1][j + 1][x \lor \text{nums}[i]] \lor f[i][j][x]
$$

Tương tự, với công thức chuyển của $g[i][j][y]$, bắt đầu từ phần tử thứ $i$ (tính từ $n - 1$), ta có thể chọn hoặc không chọn phần tử đó, nên:

$$
g[i - 1][j][y] = g[i - 1][j][y] \lor g[i][j][y] \\
g[i - 1][j + 1][y \lor \text{nums}[i - 1]] = g[i - 1][j + 1][y \lor \text{nums}[i - 1]] \lor g[i][j][y]
$$

Cuối cùng, ta duyệt $i$ trong đoạn $[k, n - k]$. Với mỗi $i$, ta duyệt $x$ và $y$, trong đó $0 \leq x, y < 2^7$. Nếu cả $f[i][k][x]$ và $g[i][k][y]$ đều đúng, ta cập nhật đáp án theo công thức $\text{ans} = \max(\text{ans}, x \oplus y)$.

Độ phức tạp thời gian là $O(n \times m \times k)$, và độ phức tạp không gian là $O(n \times m \times k)$, trong đó $n$ là độ dài của mảng và $m = 2^7$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxValue(self, nums: List[int], k: int) -> int:
        m = 1 << 7
        n = len(nums)
        f = [[[False] * m for _ in range(k + 2)] for _ in range(n + 1)]
        f[0][0][0] = True
        for i in range(n):
            for j in range(k + 1):
                for x in range(m):
                    f[i + 1][j][x] |= f[i][j][x]
                    f[i + 1][j + 1][x | nums[i]] |= f[i][j][x]

        g = [[[False] * m for _ in range(k + 2)] for _ in range(n + 1)]
        g[n][0][0] = True
        for i in range(n, 0, -1):
            for j in range(k + 1):
                for y in range(m):
                    g[i - 1][j][y] |= g[i][j][y]
                    g[i - 1][j + 1][y | nums[i - 1]] |= g[i][j][y]

        ans = 0
        for i in range(k, n - k + 1):
            for x in range(m):
                if f[i][k][x]:
                    for y in range(m):
                        if g[i][k][y]:
                            ans = max(ans, x ^ y)
        return ans
```

#### Java

```java
class Solution {
    public int maxValue(int[] nums, int k) {
        int m = 1 << 7;
        int n = nums.length;
        boolean[][][] f = new boolean[n + 1][k + 2][m];
        f[0][0][0] = true;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j <= k; j++) {
                for (int x = 0; x < m; x++) {
                    if (f[i][j][x]) {
                        f[i + 1][j][x] = true;
                        f[i + 1][j + 1][x | nums[i]] = true;
                    }
                }
            }
        }

        boolean[][][] g = new boolean[n + 1][k + 2][m];
        g[n][0][0] = true;

        for (int i = n; i > 0; i--) {
            for (int j = 0; j <= k; j++) {
                for (int y = 0; y < m; y++) {
                    if (g[i][j][y]) {
                        g[i - 1][j][y] = true;
                        g[i - 1][j + 1][y | nums[i - 1]] = true;
                    }
                }
            }
        }

        int ans = 0;

        for (int i = k; i <= n - k; i++) {
            for (int x = 0; x < m; x++) {
                if (f[i][k][x]) {
                    for (int y = 0; y < m; y++) {
                        if (g[i][k][y]) {
                            ans = Math.max(ans, x ^ y);
                        }
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
    int maxValue(vector<int>& nums, int k) {
        int m = 1 << 7;
        int n = nums.size();

        vector<vector<vector<bool>>> f(n + 1, vector<vector<bool>>(k + 2, vector<bool>(m, false)));
        f[0][0][0] = true;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j <= k; j++) {
                for (int x = 0; x < m; x++) {
                    if (f[i][j][x]) {
                        f[i + 1][j][x] = true;
                        f[i + 1][j + 1][x | nums[i]] = true;
                    }
                }
            }
        }

        vector<vector<vector<bool>>> g(n + 1, vector<vector<bool>>(k + 2, vector<bool>(m, false)));
        g[n][0][0] = true;

        for (int i = n; i > 0; i--) {
            for (int j = 0; j <= k; j++) {
                for (int y = 0; y < m; y++) {
                    if (g[i][j][y]) {
                        g[i - 1][j][y] = true;
                        g[i - 1][j + 1][y | nums[i - 1]] = true;
                    }
                }
            }
        }

        int ans = 0;

        for (int i = k; i <= n - k; i++) {
            for (int x = 0; x < m; x++) {
                if (f[i][k][x]) {
                    for (int y = 0; y < m; y++) {
                        if (g[i][k][y]) {
                            ans = max(ans, x ^ y);
                        }
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
func maxValue(nums []int, k int) int {
	m := 1 << 7
	n := len(nums)

	f := make([][][]bool, n+1)
	for i := range f {
		f[i] = make([][]bool, k+2)
		for j := range f[i] {
			f[i][j] = make([]bool, m)
		}
	}
	f[0][0][0] = true

	for i := 0; i < n; i++ {
		for j := 0; j <= k; j++ {
			for x := 0; x < m; x++ {
				if f[i][j][x] {
					f[i+1][j][x] = true
					f[i+1][j+1][x|nums[i]] = true
				}
			}
		}
	}

	g := make([][][]bool, n+1)
	for i := range g {
		g[i] = make([][]bool, k+2)
		for j := range g[i] {
			g[i][j] = make([]bool, m)
		}
	}
	g[n][0][0] = true

	for i := n; i > 0; i-- {
		for j := 0; j <= k; j++ {
			for y := 0; y < m; y++ {
				if g[i][j][y] {
					g[i-1][j][y] = true
					g[i-1][j+1][y|nums[i-1]] = true
				}
			}
		}
	}

	ans := 0

	for i := k; i <= n-k; i++ {
		for x := 0; x < m; x++ {
			if f[i][k][x] {
				for y := 0; y < m; y++ {
					if g[i][k][y] {
						ans = max(ans, x^y)
					}
				}
			}
		}
	}

	return ans
}
```

#### TypeScript

```ts
function maxValue(nums: number[], k: number): number {
    const m = 1 << 7;
    const n = nums.length;

    const f: boolean[][][] = Array.from({ length: n + 1 }, () =>
        Array.from({ length: k + 2 }, () => Array(m).fill(false)),
    );
    f[0][0][0] = true;

    for (let i = 0; i < n; i++) {
        for (let j = 0; j <= k; j++) {
            for (let x = 0; x < m; x++) {
                if (f[i][j][x]) {
                    f[i + 1][j][x] = true;
                    f[i + 1][j + 1][x | nums[i]] = true;
                }
            }
        }
    }

    const g: boolean[][][] = Array.from({ length: n + 1 }, () =>
        Array.from({ length: k + 2 }, () => Array(m).fill(false)),
    );
    g[n][0][0] = true;

    for (let i = n; i > 0; i--) {
        for (let j = 0; j <= k; j++) {
            for (let y = 0; y < m; y++) {
                if (g[i][j][y]) {
                    g[i - 1][j][y] = true;
                    g[i - 1][j + 1][y | nums[i - 1]] = true;
                }
            }
        }
    }

    let ans = 0;

    for (let i = k; i <= n - k; i++) {
        for (let x = 0; x < m; x++) {
            if (f[i][k][x]) {
                for (let y = 0; y < m; y++) {
                    if (g[i][k][y]) {
                        ans = Math.max(ans, x ^ y);
                    }
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
