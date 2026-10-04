---
comments: true
difficulty: Hard
rating: 2556
source: Weekly Contest 388 Q4
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3077. Maximum Strength of K Disjoint Subarrays](https://leetcode.com/problems/maximum-strength-of-k-disjoint-subarrays)

[中文文档](/solution/3000-3099/3077.Maximum%20Strength%20of%20K%20Disjoint%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên dương <strong>lẻ</strong> <code>k</code>.</p>

<p>Hãy chọn chính xác <b><code>k</code></b> <span data-keyword="subarray-nonempty">mảng con</span> <b><code>sub<sub>1</sub>, sub<sub>2</sub>, ..., sub<sub>k</sub></code></b> rời nhau từ <code>nums</code> sao cho phần tử cuối của <code>sub<sub>i</sub></code> nằm trước phần tử đầu của <code>sub<sub>{i+1}</sub></code> với mọi <code>1 &lt;= i &lt;= k-1</code>. Mục tiêu là tối đa hóa độ mạnh tổng cộng của chúng.</p>

<p>Độ mạnh của các mảng con được chọn được định nghĩa như sau:</p>

<p><code>strength = k * sum(sub<sub>1</sub>)- (k - 1) * sum(sub<sub>2</sub>) + (k - 2) * sum(sub<sub>3</sub>) - ... - 2 * sum(sub<sub>{k-1}</sub>) + sum(sub<sub>k</sub>)</code></p>

<p>Trong đó <b><code>sum(sub<sub>i</sub>)</code></b> là tổng các phần tử trong mảng con thứ <code>i</code>.</p>

<p>Hãy trả về <strong>độ mạnh lớn nhất</strong> có thể đạt được khi chọn chính xác <b><code>k</code></b> mảng con rời nhau từ <code>nums</code>.</p>

<p><strong>Lưu ý</strong> rằng các mảng con được chọn <strong>không</strong> cần bao phủ toàn bộ mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,-1,2], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">22</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cách chọn 3 mảng con tốt nhất có thể là: nums[0..2], nums[3..3] và nums[4..4]. Độ mạnh được tính như sau:</p>

<p><code>strength = 3 * (1 + 2 + 3) - 2 * (-1) + 2 = 22</code></p>

<p>&nbsp;</p>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong>Đầu vào:</strong> <span class="example-io">nums = [12,-2,-2,-2,-2], k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">64</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cách duy nhất để chọn 5 mảng con rời nhau là: nums[0..0], nums[1..1], nums[2..2], nums[3..3] và nums[4..4]. Độ mạnh được tính như sau:</p>

<p><code>strength = 5 * 12 - 4 * (-2) + 3 * (-2) - 2 * (-2) + (-2) = 64</code></p>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,-2,-3], k = </span>1</p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cách chọn 1 mảng con tốt nhất có thể là: nums[0..0]. Độ mạnh là -1.</p>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
	<li><code>1 &lt;= n * k &lt;= 10<sup>6</sup></code></li>
	<li><code>k</code> là số lẻ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta chọn $k$ mảng con rời nhau; mảng con thứ $j$ có trọng số $k-j+1$ và dấu xen kẽ. Vì $n \cdot k \le 10^6$, ta có thể dùng quy hoạch động $O(nk)$.
>
> Mỗi vị trí có thể được bỏ qua, tiếp tục đoạn hiện tại hoặc mở một đoạn mới. Chỉ khi mở đoạn mới thì chỉ số và dấu mới thay đổi.
>
> $f[i][j][0/1]$ là giá trị tốt nhất khi sử dụng $i$ số đầu tiên, chọn $j$ đoạn và xét xem chỉ số $i$ có được chọn hay không. Các chuyển trạng thái sử dụng $\textit{sign}$ và hệ số $k-j+1$.

<!-- thinking:end -->

Với số thứ $i$ là $nums[i - 1]$, nếu được chọn và thuộc mảng con thứ $j$, đóng góp của nó vào đáp án là $nums[i - 1] \times (k - j + 1) \times (-1)^{j+1}$. Ta ký hiệu $(-1)^{j+1}$ là $sign$, nên đóng góp của nó vào đáp án là $sign \times nums[i - 1] \times (k - j + 1)$.

Ta định nghĩa $f[i][j][0]$ là giá trị strength lớn nhất khi chọn $j$ mảng con từ $i$ số đầu tiên và không chọn số thứ $i$. Ta định nghĩa $f[i][j][1]$ là giá trị strength lớn nhất khi chọn $j$ mảng con từ $i$ số đầu tiên và chọn số thứ $i$. Ban đầu, $f[0][0][1] = 0$, các giá trị còn lại là $-\infty$.

Khi $i > 0$, ta xét cách chuyển trạng thái của $f[i][j]$.

Nếu không chọn số thứ $i$, số thứ $i-1$ có thể được chọn hoặc không được chọn, do đó $f[i][j][0] = \max(f[i-1][j][0], f[i-1][j][1])$.

Nếu chọn số thứ $i$, nếu số thứ $i-1$ và số thứ $i$ nằm trong cùng một mảng con, thì $f[i][j][1] = \max(f[i][j][1], f[i-1][j][1] + sign \times nums[i-1] \times (k - j + 1))$, ngược lại $f[i][j][1] = \max(f[i][j][1], \max(f[i-1][j-1][0], f[i-1][j-1][1]) + sign \times nums[i-1] \times (k - j + 1))$.

Đáp án cuối cùng là $\max(f[n][k][0], f[n][k][1])$.

Độ phức tạp thời gian là $O(n \times k)$ và độ phức tạp không gian là $O(n \times k)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumStrength(self, nums: List[int], k: int) -> int:
        n = len(nums)
        f = [[[-inf, -inf] for _ in range(k + 1)] for _ in range(n + 1)]
        f[0][0][0] = 0
        for i, x in enumerate(nums, 1):
            for j in range(k + 1):
                sign = 1 if j & 1 else -1
                f[i][j][0] = max(f[i - 1][j][0], f[i - 1][j][1])
                f[i][j][1] = max(f[i][j][1], f[i - 1][j][1] + sign * x * (k - j + 1))
                if j:
                    f[i][j][1] = max(
                        f[i][j][1], max(f[i - 1][j - 1]) + sign * x * (k - j + 1)
                    )
        return max(f[n][k])
```

#### Java

```java
class Solution {
    public long maximumStrength(int[] nums, int k) {
        int n = nums.length;
        long[][][] f = new long[n + 1][k + 1][2];
        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= k; j++) {
                Arrays.fill(f[i][j], Long.MIN_VALUE / 2);
            }
        }
        f[0][0][0] = 0;
        for (int i = 1; i <= n; i++) {
            int x = nums[i - 1];
            for (int j = 0; j <= k; j++) {
                long sign = (j & 1) == 1 ? 1 : -1;
                long val = sign * x * (k - j + 1);
                f[i][j][0] = Math.max(f[i - 1][j][0], f[i - 1][j][1]);
                f[i][j][1] = Math.max(f[i][j][1], f[i - 1][j][1] + val);
                if (j > 0) {
                    long t = Math.max(f[i - 1][j - 1][0], f[i - 1][j - 1][1]) + val;
                    f[i][j][1] = Math.max(f[i][j][1], t);
                }
            }
        }
        return Math.max(f[n][k][0], f[n][k][1]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumStrength(vector<int>& nums, int k) {
        int n = nums.size();
        long long f[n + 1][k + 1][2];
        memset(f, -0x3f3f3f3f3f3f3f3f, sizeof(f));
        f[0][0][0] = 0;
        for (int i = 1; i <= n; i++) {
            int x = nums[i - 1];
            for (int j = 0; j <= k; j++) {
                long long sign = (j & 1) == 1 ? 1 : -1;
                long long val = sign * x * (k - j + 1);
                f[i][j][0] = max(f[i - 1][j][0], f[i - 1][j][1]);
                f[i][j][1] = max(f[i][j][1], f[i - 1][j][1] + val);
                if (j > 0) {
                    long long t = max(f[i - 1][j - 1][0], f[i - 1][j - 1][1]) + val;
                    f[i][j][1] = max(f[i][j][1], t);
                }
            }
        }
        return max(f[n][k][0], f[n][k][1]);
    }
};
```

#### Go

```go
func maximumStrength(nums []int, k int) int64 {
	n := len(nums)
	f := make([][][]int64, n+1)
	const inf int64 = math.MinInt64 / 2
	for i := range f {
		f[i] = make([][]int64, k+1)
		for j := range f[i] {
			f[i][j] = []int64{inf, inf}
		}
	}
	f[0][0][0] = 0
	for i := 1; i <= n; i++ {
		x := nums[i-1]
		for j := 0; j <= k; j++ {
			sign := int64(-1)
			if j&1 == 1 {
				sign = 1
			}
			val := sign * int64(x) * int64(k-j+1)
			f[i][j][0] = max(f[i-1][j][0], f[i-1][j][1])
			f[i][j][1] = max(f[i][j][1], f[i-1][j][1]+val)
			if j > 0 {
				t := max(f[i-1][j-1][0], f[i-1][j-1][1]) + val
				f[i][j][1] = max(f[i][j][1], t)
			}
		}
	}
	return max(f[n][k][0], f[n][k][1])
}
```

#### TypeScript

```ts
function maximumStrength(nums: number[], k: number): number {
    const n: number = nums.length;
    const f: number[][][] = Array.from({ length: n + 1 }, () =>
        Array.from({ length: k + 1 }, () => [-Infinity, -Infinity]),
    );
    f[0][0][0] = 0;
    for (let i = 1; i <= n; i++) {
        const x: number = nums[i - 1];
        for (let j = 0; j <= k; j++) {
            const sign: number = (j & 1) === 1 ? 1 : -1;
            const val: number = sign * x * (k - j + 1);
            f[i][j][0] = Math.max(f[i - 1][j][0], f[i - 1][j][1]);
            f[i][j][1] = Math.max(f[i][j][1], f[i - 1][j][1] + val);
            if (j > 0) {
                f[i][j][1] = Math.max(f[i][j][1], Math.max(...f[i - 1][j - 1]) + val);
            }
        }
    }
    return Math.max(...f[n][k]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
