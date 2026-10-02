---
comments: true
difficulty: Hard
rating: 2175
source: Weekly Contest 185 Q4
tags:
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [1420. Build Array Where You Can Find The Maximum Exactly K Comparisons](https://leetcode.com/problems/build-array-where-you-can-find-the-maximum-exactly-k-comparisons)

[中文文档](/solution/1400-1499/1420.Build%20Array%20Where%20You%20Can%20Find%20The%20Maximum%20Exactly%20K%20Comparisons/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba số nguyên <code>n</code>, <code>m</code> và <code>k</code>. Xét thuật toán sau để tìm phần tử lớn nhất của một mảng gồm các số nguyên dương:</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1420.Build%20Array%20Where%20You%20Can%20Find%20The%20Maximum%20Exactly%20K%20Comparisons/images/e.png" style="width: 424px; height: 372px;" />
<p>Bạn cần xây dựng mảng arr thỏa mãn các tính chất sau:</p>

<ul>
	<li><code>arr</code> có đúng <code>n</code> số nguyên.</li>
	<li><code>1 &lt;= arr[i] &lt;= m</code> với <code>(0 &lt;= i &lt; n)</code>.</li>
	<li>Sau khi áp dụng thuật toán trên cho <code>arr</code>, giá trị <code>search_cost</code> bằng <code>k</code>.</li>
</ul>

<p>Trả về <em>số cách</em> xây dựng mảng <code>arr</code> thỏa mãn các điều kiện trên. Vì đáp án có thể rất lớn, <strong>phải</strong> tính đáp án theo modulo <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, m = 3, k = 1
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các mảng có thể có là [1, 1], [2, 1], [2, 2], [3, 1], [3, 2] [3, 3]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, m = 2, k = 3
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có mảng nào thỏa mãn các điều kiện trên.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 9, m = 1, k = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Mảng duy nhất có thể có là [1, 1, 1, 1, 1, 1, 1, 1, 1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 50</code></li>
	<li><code>1 &lt;= m &lt;= 100</code></li>
	<li><code>0 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Search cost là số lần prefix maximum được cập nhật. Vì $n\le 50$, $m\le 100$, ta dùng DP theo độ dài, cost và giá trị lớn nhất hiện tại.
>
> Nếu vị trí $i$ không làm tăng maximum, nó có $j$ lựa chọn và giữ nguyên cost. Nếu ghi một maximum mới $j$, ta cộng các trạng thái có maximum trước đó nhỏ hơn và cost nhỏ hơn một đơn vị.
>
> $k=0$ là không thể xảy ra. Tính tổng $dp[n][k][\cdot]$ để được đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numOfArrays(self, n: int, m: int, k: int) -> int:
        if k == 0:
            return 0
        dp = [[[0] * (m + 1) for _ in range(k + 1)] for _ in range(n + 1)]
        mod = 10**9 + 7
        for i in range(1, m + 1):
            dp[1][1][i] = 1
        for i in range(2, n + 1):
            for c in range(1, min(k + 1, i + 1)):
                for j in range(1, m + 1):
                    dp[i][c][j] = dp[i - 1][c][j] * j
                    for j0 in range(1, j):
                        dp[i][c][j] += dp[i - 1][c - 1][j0]
                        dp[i][c][j] %= mod
        ans = 0
        for i in range(1, m + 1):
            ans += dp[n][k][i]
            ans %= mod
        return ans
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int numOfArrays(int n, int m, int k) {
        if (k == 0) {
            return 0;
        }
        long[][][] dp = new long[n + 1][k + 1][m + 1];
        for (int i = 1; i <= m; ++i) {
            dp[1][1][i] = 1;
        }
        for (int i = 2; i <= n; ++i) {
            for (int c = 1; c <= Math.min(i, k); ++c) {
                for (int j = 1; j <= m; ++j) {
                    dp[i][c][j] = (dp[i - 1][c][j] * j) % MOD;
                    for (int j0 = 1; j0 < j; ++j0) {
                        dp[i][c][j] = (dp[i][c][j] + dp[i - 1][c - 1][j0]) % MOD;
                    }
                }
            }
        }
        long ans = 0;
        for (int i = 1; i <= m; ++i) {
            ans = (ans + dp[n][k][i]) % MOD;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numOfArrays(int n, int m, int k) {
        if (k == 0) return 0;
        int mod = 1e9 + 7;
        using ll = long long;
        vector<vector<vector<ll>>> dp(n + 1, vector<vector<ll>>(k + 1, vector<ll>(m + 1)));
        for (int i = 1; i <= m; ++i) dp[1][1][i] = 1;
        for (int i = 2; i <= n; ++i) {
            for (int c = 1; c <= min(i, k); ++c) {
                for (int j = 1; j <= m; ++j) {
                    dp[i][c][j] = (dp[i - 1][c][j] * j) % mod;
                    for (int j0 = 1; j0 < j; ++j0) {
                        dp[i][c][j] = (dp[i][c][j] + dp[i - 1][c - 1][j0]) % mod;
                    }
                }
            }
        }
        ll ans = 0;
        for (int i = 1; i <= m; ++i) ans = (ans + dp[n][k][i]) % mod;
        return (int) ans;
    }
};
```

#### Go

```go
func numOfArrays(n int, m int, k int) int {
	if k == 0 {
		return 0
	}
	mod := int(1e9) + 7
	dp := make([][][]int, n+1)
	for i := range dp {
		dp[i] = make([][]int, k+1)
		for j := range dp[i] {
			dp[i][j] = make([]int, m+1)
		}
	}
	for i := 1; i <= m; i++ {
		dp[1][1][i] = 1
	}
	for i := 2; i <= n; i++ {
		for c := 1; c <= k && c <= i; c++ {
			for j := 1; j <= m; j++ {
				dp[i][c][j] = (dp[i-1][c][j] * j) % mod
				for j0 := 1; j0 < j; j0++ {
					dp[i][c][j] = (dp[i][c][j] + dp[i-1][c-1][j0]) % mod
				}
			}
		}
	}
	ans := 0
	for i := 1; i <= m; i++ {
		ans = (ans + dp[n][k][i]) % mod
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
