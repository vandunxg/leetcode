---
comments: true
difficulty: Hard
rating: 2025
source: Biweekly Contest 1 Q4
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [1067. Digit Count in Range 🔒](https://leetcode.com/problems/digit-count-in-range)

[中文文档](/solution/1000-1099/1067.Digit%20Count%20in%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên một chữ số <code>d</code> và hai số nguyên <code>low</code>, <code>high</code>. Hãy trả về <em>số lần chữ số </em><code>d</code><em> xuất hiện trong tất cả các số nguyên thuộc đoạn bao gồm cả hai đầu mút </em><code>[low, high]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> d = 1, low = 1, high = 13
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Chữ số d = 1 xuất hiện 6 lần trong các số 1, 10, 11, 12, 13.
Lưu ý, chữ số d = 1 xuất hiện hai lần trong số 11.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> d = 3, low = 100, high = 250
<strong>Đầu ra:</strong> 35
<strong>Giải thích:</strong> Chữ số d = 3 xuất hiện 35 lần trong các số 103,113,123,130,131,...,238,239,243.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= d &lt;= 9</code></li>
	<li><code>1 &lt;= low &lt;= high &lt;= 2 * 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Không thể duyệt từng số để đếm chữ số $d$ trong đoạn $[\textit{low},\textit{high}]$ vì giới hạn lên đến $2\times 10^8$. Ta tính kết quả bằng $f(\textit{high})-f(\textit{low}-1)$, trong đó $f(n)$ đếm số lần chữ số $d$ xuất hiện trong các số từ $1$ đến $n$.
>
> Digit DP theo dõi số lần $d$ đã xuất hiện, các số 0 ở đầu và giới hạn trên. Khi $d=0$, các số 0 ở đầu không được tính.
>
> Tách $n$ thành các chữ số rồi ghi nhớ kết quả theo trạng thái $\textit{dfs}(\textit{pos},\textit{cnt},\textit{lead},\textit{limit})$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def digitsCount(self, d: int, low: int, high: int) -> int:
        return self.f(high, d) - self.f(low - 1, d)

    def f(self, n, d):
        @cache
        def dfs(pos, cnt, lead, limit):
            if pos <= 0:
                return cnt
            up = a[pos] if limit else 9
            ans = 0
            for i in range(up + 1):
                if i == 0 and lead:
                    ans += dfs(pos - 1, cnt, lead, limit and i == up)
                else:
                    ans += dfs(pos - 1, cnt + (i == d), False, limit and i == up)
            return ans

        a = [0] * 11
        l = 0
        while n:
            l += 1
            a[l] = n % 10
            n //= 10
        return dfs(l, 0, True, True)
```

#### Java

```java
class Solution {
    private int d;
    private int[] a = new int[11];
    private int[][] dp = new int[11][11];

    public int digitsCount(int d, int low, int high) {
        this.d = d;
        return f(high) - f(low - 1);
    }

    private int f(int n) {
        for (var e : dp) {
            Arrays.fill(e, -1);
        }
        int len = 0;
        while (n > 0) {
            a[++len] = n % 10;
            n /= 10;
        }
        return dfs(len, 0, true, true);
    }

    private int dfs(int pos, int cnt, boolean lead, boolean limit) {
        if (pos <= 0) {
            return cnt;
        }
        if (!lead && !limit && dp[pos][cnt] != -1) {
            return dp[pos][cnt];
        }
        int up = limit ? a[pos] : 9;
        int ans = 0;
        for (int i = 0; i <= up; ++i) {
            if (i == 0 && lead) {
                ans += dfs(pos - 1, cnt, lead, limit && i == up);
            } else {
                ans += dfs(pos - 1, cnt + (i == d ? 1 : 0), false, limit && i == up);
            }
        }
        if (!lead && !limit) {
            dp[pos][cnt] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int d;
    int a[11];
    int dp[11][11];

    int digitsCount(int d, int low, int high) {
        this->d = d;
        return f(high) - f(low - 1);
    }

    int f(int n) {
        memset(dp, -1, sizeof dp);
        int len = 0;
        while (n) {
            a[++len] = n % 10;
            n /= 10;
        }
        return dfs(len, 0, true, true);
    }

    int dfs(int pos, int cnt, bool lead, bool limit) {
        if (pos <= 0) {
            return cnt;
        }
        if (!lead && !limit && dp[pos][cnt] != -1) {
            return dp[pos][cnt];
        }
        int up = limit ? a[pos] : 9;
        int ans = 0;
        for (int i = 0; i <= up; ++i) {
            if (i == 0 && lead) {
                ans += dfs(pos - 1, cnt, lead, limit && i == up);
            } else {
                ans += dfs(pos - 1, cnt + (i == d), false, limit && i == up);
            }
        }
        if (!lead && !limit) {
            dp[pos][cnt] = ans;
        }
        return ans;
    }
};
```

#### Go

```go
func digitsCount(d int, low int, high int) int {
	f := func(n int) int {
		a := make([]int, 11)
		dp := make([][]int, 11)
		for i := range dp {
			dp[i] = make([]int, 11)
			for j := range dp[i] {
				dp[i][j] = -1
			}
		}
		l := 0
		for n > 0 {
			l++
			a[l] = n % 10
			n /= 10
		}

		var dfs func(int, int, bool, bool) int
		dfs = func(pos, cnt int, lead, limit bool) int {
			if pos <= 0 {
				return cnt
			}
			if !lead && !limit && dp[pos][cnt] != -1 {
				return dp[pos][cnt]
			}
			up := 9
			if limit {
				up = a[pos]
			}
			ans := 0
			for i := 0; i <= up; i++ {
				if i == 0 && lead {
					ans += dfs(pos-1, cnt, lead, limit && i == up)
				} else {
					t := cnt
					if d == i {
						t++
					}
					ans += dfs(pos-1, t, false, limit && i == up)
				}
			}
			if !lead && !limit {
				dp[pos][cnt] = ans
			}
			return ans
		}

		return dfs(l, 0, true, true)
	}
	return f(high) - f(low-1)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
