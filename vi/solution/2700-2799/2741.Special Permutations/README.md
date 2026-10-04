---
comments: true
difficulty: Medium
rating: 2020
source: Weekly Contest 350 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [2741. Special Permutations](https://leetcode.com/problems/special-permutations)

[中文文档](/solution/2700-2799/2741.Special%20Permutations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> gồm&nbsp;<code>n</code>&nbsp;số nguyên dương <strong>phân biệt</strong>. Một hoán vị của&nbsp;<code>nums</code>&nbsp;được gọi là đặc biệt nếu:</p>

<ul>
	<li>Với mọi chỉ số&nbsp;<code>0 &lt;= i &lt; n - 1</code>, hoặc&nbsp;<code>nums[i] % nums[i+1] == 0</code>, hoặc&nbsp;<code>nums[i+1] % nums[i] == 0</code>.</li>
</ul>

<p>Trả về <em>tổng số hoán vị đặc biệt.&nbsp;</em>Vì đáp án có thể rất lớn, hãy trả về kết quả theo <strong>modulo&nbsp;</strong><code>10<sup>9&nbsp;</sup>+ 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,6]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> [3,6,2] và [2,6,3] là hai hoán vị đặc biệt của nums.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> [3,1,4] và [4,1,3] là hai hoán vị đặc biệt của nums.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 14</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động nén trạng thái

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các hoán vị trong đó mọi cặp phần tử kề nhau đều có một giá trị chia hết cho giá trị kia. Vì $n\le 14$, việc duyệt $n!$ là quá lớn, nhưng tập các phần tử đã dùng và chỉ số cuối cùng là đủ để xác định phần còn lại.
>
> Gọi $f[i][j]$ là số cách sử dụng mask $i$ và kết thúc tại chỉ số $j$. Với mask chỉ có một bit, giá trị là $1$; các trường hợp khác lấy tổng $f[i\oplus 2^j][k]$ trên các chỉ số trước đó $k$ thỏa mãn điều kiện chia hết. Cộng các giá trị của mask đầy đủ rồi lấy modulo $10^9+7$.

<!-- thinking:end -->

Ta nhận thấy độ dài tối đa của mảng trong bài toán không vượt quá $14$. Do đó, ta có thể dùng một số nguyên để biểu diễn trạng thái hiện tại, trong đó bit thứ $i$ là $1$ nếu số thứ $i$ trong mảng đã được chọn, và là $0$ nếu chưa được chọn.

Ta định nghĩa $f[i][j]$ là số cách mà trạng thái các số nguyên đã chọn hiện tại là $i$, và chỉ số của số nguyên được chọn cuối cùng là $j$. Ban đầu, $f[0][0]=0$, và đáp án là $\sum_{j=0}^{n-1}f[2^n-1][j]$.

Xét $f[i][j]$, nếu hiện tại chỉ có một số được chọn thì $f[i][j]=1$. Ngược lại, ta có thể liệt kê chỉ số $k$ của số được chọn cuối cùng. Nếu các số tương ứng với $k$ và $j$ thỏa mãn yêu cầu của bài toán, thì $f[i][j]$ có thể được chuyển từ $f[i \oplus 2^j][k]$. Cụ thể:

$$
f[i][j]=
\begin{cases}
1, & i=2^j\\
\sum_{k=0}^{n-1}f[i \oplus 2^j][k], & i \neq 2^j \textit{ and nums}[j] \textit{ and nums}[k] \textit{ meet the requirements of the problem}\\
\end{cases}
$$

Đáp án cuối cùng là $\sum_{j=0}^{n-1}f[2^n-1][j]$. Lưu ý rằng đáp án có thể rất lớn, vì vậy ta cần lấy modulo $10^9+7$.

Độ phức tạp thời gian là $O(n^2 \times 2^n)$, và độ phức tạp không gian là $O(n \times 2^n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def specialPerm(self, nums: List[int]) -> int:
        mod = 10**9 + 7
        n = len(nums)
        m = 1 << n
        f = [[0] * n for _ in range(m)]
        for i in range(1, m):
            for j, x in enumerate(nums):
                if i >> j & 1:
                    ii = i ^ (1 << j)
                    if ii == 0:
                        f[i][j] = 1
                        continue
                    for k, y in enumerate(nums):
                        if x % y == 0 or y % x == 0:
                            f[i][j] = (f[i][j] + f[ii][k]) % mod
        return sum(f[-1]) % mod
```

#### Java

```java
class Solution {
    public int specialPerm(int[] nums) {
        final int mod = (int) 1e9 + 7;
        int n = nums.length;
        int m = 1 << n;
        int[][] f = new int[m][n];
        for (int i = 1; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    int ii = i ^ (1 << j);
                    if (ii == 0) {
                        f[i][j] = 1;
                        continue;
                    }
                    for (int k = 0; k < n; ++k) {
                        if (nums[j] % nums[k] == 0 || nums[k] % nums[j] == 0) {
                            f[i][j] = (f[i][j] + f[ii][k]) % mod;
                        }
                    }
                }
            }
        }
        int ans = 0;
        for (int x : f[m - 1]) {
            ans = (ans + x) % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int specialPerm(vector<int>& nums) {
        const int mod = 1e9 + 7;
        int n = nums.size();
        int m = 1 << n;
        int f[m][n];
        memset(f, 0, sizeof(f));
        for (int i = 1; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    int ii = i ^ (1 << j);
                    if (ii == 0) {
                        f[i][j] = 1;
                        continue;
                    }
                    for (int k = 0; k < n; ++k) {
                        if (nums[j] % nums[k] == 0 || nums[k] % nums[j] == 0) {
                            f[i][j] = (f[i][j] + f[ii][k]) % mod;
                        }
                    }
                }
            }
        }
        int ans = 0;
        for (int x : f[m - 1]) {
            ans = (ans + x) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func specialPerm(nums []int) (ans int) {
	const mod int = 1e9 + 7
	n := len(nums)
	m := 1 << n
	f := make([][]int, m)
	for i := range f {
		f[i] = make([]int, n)
	}
	for i := 1; i < m; i++ {
		for j, x := range nums {
			if i>>j&1 == 1 {
				ii := i ^ (1 << j)
				if ii == 0 {
					f[i][j] = 1
					continue
				}
				for k, y := range nums {
					if x%y == 0 || y%x == 0 {
						f[i][j] = (f[i][j] + f[ii][k]) % mod
					}
				}
			}
		}
	}
	for _, x := range f[m-1] {
		ans = (ans + x) % mod
	}
	return
}
```

#### TypeScript

```ts
function specialPerm(nums: number[]): number {
    const mod = 1e9 + 7;
    const n = nums.length;
    const m = 1 << n;
    const f = Array.from({ length: m }, () => Array(n).fill(0));

    for (let i = 1; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (((i >> j) & 1) === 1) {
                const ii = i ^ (1 << j);
                if (ii === 0) {
                    f[i][j] = 1;
                    continue;
                }
                for (let k = 0; k < n; ++k) {
                    if (nums[j] % nums[k] === 0 || nums[k] % nums[j] === 0) {
                        f[i][j] = (f[i][j] + f[ii][k]) % mod;
                    }
                }
            }
        }
    }

    return f[m - 1].reduce((acc, x) => (acc + x) % mod);
}
```

#### Rust

```rust
impl Solution {
    pub fn special_perm(nums: Vec<i32>) -> i32 {
        const MOD: i32 = 1_000_000_007;
        let n = nums.len();
        let m = 1 << n;
        let mut f = vec![vec![0; n]; m];

        for i in 1..m {
            for j in 0..n {
                if (i >> j) & 1 == 1 {
                    let ii = i ^ (1 << j);
                    if ii == 0 {
                        f[i][j] = 1;
                        continue;
                    }
                    for k in 0..n {
                        if nums[j] % nums[k] == 0 || nums[k] % nums[j] == 0 {
                            f[i][j] = (f[i][j] + f[ii][k]) % MOD;
                        }
                    }
                }
            }
        }

        let mut ans = 0;
        for &x in &f[m - 1] {
            ans = (ans + x) % MOD;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
