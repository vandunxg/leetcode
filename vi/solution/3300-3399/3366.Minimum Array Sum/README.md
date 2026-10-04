---
comments: true
difficulty: Medium
rating: 2040
source: Weekly Contest 425 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3366. Minimum Array Sum](https://leetcode.com/problems/minimum-array-sum)

[中文文档](/solution/3300-3399/3366.Minimum%20Array%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và ba số nguyên <code>k</code>, <code>op1</code> và <code>op2</code>.</p>

<p>Bạn có thể thực hiện các thao tác sau trên <code>nums</code>:</p>

<ul>
	<li><strong>Thao tác 1</strong>: Chọn một chỉ số <code>i</code> và chia <code>nums[i]</code> cho 2, <strong>làm tròn lên</strong> đến số nguyên gần nhất. Bạn có thể thực hiện thao tác này nhiều nhất <code>op1</code> lần và không quá <strong>một lần</strong> trên mỗi chỉ số.</li>
	<li><strong>Thao tác 2</strong>: Chọn một chỉ số <code>i</code> và trừ <code>k</code> khỏi <code>nums[i]</code>, nhưng chỉ khi <code>nums[i]</code> lớn hơn hoặc bằng <code>k</code>. Bạn có thể thực hiện thao tác này nhiều nhất <code>op2</code> lần và không quá <strong>một lần</strong> trên mỗi chỉ số.</li>
</ul>

<p><strong>Lưu ý:</strong> Cả hai thao tác đều có thể được áp dụng trên cùng một chỉ số, nhưng mỗi thao tác chỉ được áp dụng nhiều nhất một lần.</p>

<p>Trả về <strong>tổng</strong> <strong>nhỏ nhất</strong> có thể có của tất cả phần tử trong <code>nums</code> sau khi thực hiện một số thao tác bất kỳ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,8,3,19,3], k = 3, op1 = 1, op2 = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">23</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Áp dụng thao tác 2 cho <code>nums[1] = 8</code>, thu được <code>nums[1] = 5</code>.</li>
	<li>Áp dụng thao tác 1 cho <code>nums[3] = 19</code>, thu được <code>nums[3] = 10</code>.</li>
	<li>Mảng kết quả là <code>[2, 5, 3, 10, 3]</code>, có tổng nhỏ nhất là 23 sau khi áp dụng các thao tác.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4,3], k = 3, op1 = 2, op2 = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Áp dụng thao tác 1 cho <code>nums[0] = 2</code>, thu được <code>nums[0] = 1</code>.</li>
	<li>Áp dụng thao tác 1 cho <code>nums[1] = 4</code>, thu được <code>nums[1] = 2</code>.</li>
	<li>Áp dụng thao tác 2 cho <code>nums[2] = 3</code>, thu được <code>nums[2] = 0</code>.</li>
	<li>Mảng kết quả là <code>[1, 2, 0]</code>, có tổng nhỏ nhất là 3 sau khi áp dụng các thao tác.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code><font face="monospace">0 &lt;= nums[i] &lt;= 10<sup>5</sup></font></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= op1, op2 &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giá trị có thể được chia đôi (làm tròn lên) hoặc giảm đi $d$, với giới hạn số lần tương ứng là $\textit{op1}$ và $\textit{op2}$. Vì $n \le 100$, ta có thể sử dụng quy hoạch động 3 chiều.
>
> $f[i][j][k]$ là tổng nhỏ nhất sau khi xử lý $i$ số, sử dụng $j$ lần chia đôi và $k$ lần trừ. Với cùng một số, cần thử cả hai thứ tự áp dụng hai thao tác.
>
> Đáp án là giá trị nhỏ nhất trong lớp cuối cùng.

<!-- thinking:end -->

Để thuận tiện, ta ký hiệu $k$ đã cho là $d$.

Tiếp theo, ta định nghĩa $f[i][j][k]$ là tổng nhỏ nhất của $i$ phần tử đầu tiên khi sử dụng $j$ thao tác loại 1 và $k$ thao tác loại 2. Ban đầu, $f[0][0][0] = 0$, các giá trị còn lại $f[i][j][k] = +\infty$.

Xét cách chuyển trạng thái cho $f[i][j][k]$. Ta có thể liệt kê số thứ $i$ là $x$, sau đó xét ảnh hưởng của $x$ lên $f[i][j][k]$:

- Nếu $x$ không sử dụng thao tác 1 hoặc thao tác 2, thì $f[i][j][k] = f[i-1][j][k] + x$;
- Nếu $j \gt 0$, ta có thể sử dụng thao tác 1. Khi đó, $f[i][j][k] = \min(f[i][j][k], f[i-1][j-1][k] + \lceil \frac{x+1}{2} \rceil)$;
- Nếu $k \gt 0$ và $x \geq d$, ta có thể sử dụng thao tác 2. Khi đó, $f[i][j][k] = \min(f[i][j][k], f[i-1][j][k-1] + (x - d))$;
- Nếu $j \gt 0$ và $k \gt 0$, ta có thể sử dụng cả thao tác 1 và thao tác 2. Nếu sử dụng thao tác 1 trước, $x$ trở thành $\lceil \frac{x+1}{2} \rceil$. Nếu $x \geq d$, ta có thể sử dụng thao tác 2. Khi đó, $f[i][j][k] = \min(f[i][j][k], f[i-1][j-1][k-1] + \lceil \frac{x+1}{2} \rceil - d)$. Nếu sử dụng thao tác 2 trước, $x$ trở thành $x - d$. Nếu $x \geq d$, ta có thể sử dụng thao tác 1. Khi đó, $f[i][j][k] = \min(f[i][j][k], f[i-1][j-1][k-1] + \lceil \frac{x-d+1}{2} \rceil)$.

Đáp án cuối cùng là $\min_{j=0}^{op1} \min_{k=0}^{op2} f[n][j][k]$. Nếu giá trị này là $+\infty$, trả về $-1$.

Độ phức tạp thời gian là $O(n \times \textit{op1} \times \textit{op2})$, và độ phức tạp không gian là $O(n \times \textit{op1} \times \textit{op2})$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minArraySum(self, nums: List[int], d: int, op1: int, op2: int) -> int:
        n = len(nums)
        f = [[[inf] * (op2 + 1) for _ in range(op1 + 1)] for _ in range(n + 1)]
        f[0][0][0] = 0
        for i, x in enumerate(nums, 1):
            for j in range(op1 + 1):
                for k in range(op2 + 1):
                    f[i][j][k] = f[i - 1][j][k] + x
                    if j > 0:
                        f[i][j][k] = min(f[i][j][k], f[i - 1][j - 1][k] + (x + 1) // 2)
                    if k > 0 and x >= d:
                        f[i][j][k] = min(f[i][j][k], f[i - 1][j][k - 1] + (x - d))
                    if j > 0 and k > 0:
                        y = (x + 1) // 2
                        if y >= d:
                            f[i][j][k] = min(f[i][j][k], f[i - 1][j - 1][k - 1] + y - d)
                        if x >= d:
                            f[i][j][k] = min(
                                f[i][j][k], f[i - 1][j - 1][k - 1] + (x - d + 1) // 2
                            )
        ans = inf
        for j in range(op1 + 1):
            for k in range(op2 + 1):
                ans = min(ans, f[n][j][k])
        return ans
```

#### Java

```java
class Solution {
    public int minArraySum(int[] nums, int d, int op1, int op2) {
        int n = nums.length;
        int[][][] f = new int[n + 1][op1 + 1][op2 + 1];
        final int inf = 1 << 29;
        for (var g : f) {
            for (var h : g) {
                Arrays.fill(h, inf);
            }
        }
        f[0][0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            int x = nums[i - 1];
            for (int j = 0; j <= op1; ++j) {
                for (int k = 0; k <= op2; ++k) {
                    f[i][j][k] = f[i - 1][j][k] + x;
                    if (j > 0) {
                        f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j - 1][k] + (x + 1) / 2);
                    }
                    if (k > 0 && x >= d) {
                        f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j][k - 1] + (x - d));
                    }
                    if (j > 0 && k > 0) {
                        int y = (x + 1) / 2;
                        if (y >= d) {
                            f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j - 1][k - 1] + (y - d));
                        }
                        if (x >= d) {
                            f[i][j][k]
                                = Math.min(f[i][j][k], f[i - 1][j - 1][k - 1] + (x - d + 1) / 2);
                        }
                    }
                }
            }
        }
        int ans = inf;
        for (int j = 0; j <= op1; ++j) {
            for (int k = 0; k <= op2; ++k) {
                ans = Math.min(ans, f[n][j][k]);
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
    int minArraySum(vector<int>& nums, int d, int op1, int op2) {
        int n = nums.size();
        int f[n + 1][op1 + 1][op2 + 1];
        memset(f, 0x3f, sizeof f);
        f[0][0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            int x = nums[i - 1];
            for (int j = 0; j <= op1; ++j) {
                for (int k = 0; k <= op2; ++k) {
                    f[i][j][k] = f[i - 1][j][k] + x;
                    if (j > 0) {
                        f[i][j][k] = min(f[i][j][k], f[i - 1][j - 1][k] + (x + 1) / 2);
                    }
                    if (k > 0 && x >= d) {
                        f[i][j][k] = min(f[i][j][k], f[i - 1][j][k - 1] + (x - d));
                    }
                    if (j > 0 && k > 0) {
                        int y = (x + 1) / 2;
                        if (y >= d) {
                            f[i][j][k] = min(f[i][j][k], f[i - 1][j - 1][k - 1] + (y - d));
                        }
                        if (x >= d) {
                            f[i][j][k] = min(f[i][j][k], f[i - 1][j - 1][k - 1] + (x - d + 1) / 2);
                        }
                    }
                }
            }
        }
        int ans = INT_MAX;
        for (int j = 0; j <= op1; ++j) {
            for (int k = 0; k <= op2; ++k) {
                ans = min(ans, f[n][j][k]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minArraySum(nums []int, d int, op1 int, op2 int) int {
	n := len(nums)
	const inf = int(1e9)
	f := make([][][]int, n+1)
	for i := range f {
		f[i] = make([][]int, op1+1)
		for j := range f[i] {
			f[i][j] = make([]int, op2+1)
			for k := range f[i][j] {
				f[i][j][k] = inf
			}
		}
	}
	f[0][0][0] = 0
	for i := 1; i <= n; i++ {
		x := nums[i-1]
		for j := 0; j <= op1; j++ {
			for k := 0; k <= op2; k++ {
				f[i][j][k] = f[i-1][j][k] + x
				if j > 0 {
					f[i][j][k] = min(f[i][j][k], f[i-1][j-1][k]+(x+1)/2)
				}
				if k > 0 && x >= d {
					f[i][j][k] = min(f[i][j][k], f[i-1][j][k-1]+(x-d))
				}
				if j > 0 && k > 0 {
					y := (x + 1) / 2
					if y >= d {
						f[i][j][k] = min(f[i][j][k], f[i-1][j-1][k-1]+(y-d))
					}
					if x >= d {
						f[i][j][k] = min(f[i][j][k], f[i-1][j-1][k-1]+(x-d+1)/2)
					}
				}
			}
		}
	}
	ans := inf
	for j := 0; j <= op1; j++ {
		for k := 0; k <= op2; k++ {
			ans = min(ans, f[n][j][k])
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minArraySum(nums: number[], d: number, op1: number, op2: number): number {
    const n = nums.length;
    const inf = Number.MAX_SAFE_INTEGER;

    const f: number[][][] = Array.from({ length: n + 1 }, () =>
        Array.from({ length: op1 + 1 }, () => Array(op2 + 1).fill(inf)),
    );
    f[0][0][0] = 0;

    for (let i = 1; i <= n; i++) {
        const x = nums[i - 1];
        for (let j = 0; j <= op1; j++) {
            for (let k = 0; k <= op2; k++) {
                f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j][k] + x);
                if (j > 0) {
                    f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j - 1][k] + Math.floor((x + 1) / 2));
                }
                if (k > 0 && x >= d) {
                    f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j][k - 1] + (x - d));
                }
                if (j > 0 && k > 0) {
                    const y = Math.floor((x + 1) / 2);
                    if (y >= d) {
                        f[i][j][k] = Math.min(f[i][j][k], f[i - 1][j - 1][k - 1] + (y - d));
                    }
                    if (x >= d) {
                        f[i][j][k] = Math.min(
                            f[i][j][k],
                            f[i - 1][j - 1][k - 1] + Math.floor((x - d + 1) / 2),
                        );
                    }
                }
            }
        }
    }

    let ans = inf;
    for (let j = 0; j <= op1; j++) {
        for (let l = 0; l <= op2; l++) {
            ans = Math.min(ans, f[n][j][l]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
