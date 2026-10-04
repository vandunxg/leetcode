---
comments: true
difficulty: Hard
rating: 2857
source: Weekly Contest 362 Q4
tags:
    - Math
    - String
    - Dynamic Programming
    - String Matching
---

<!-- problem:start -->

# [2851. String Transformation](https://leetcode.com/problems/string-transformation)

[中文文档](/solution/2800-2899/2851.String%20Transformation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>t</code> có cùng độ dài <code>n</code>. Bạn có thể thực hiện thao tác sau trên chuỗi <code>s</code>:</p>

<ul>
	<li>Xóa một <strong>hậu tố</strong> của <code>s</code> có độ dài <code>l</code>, trong đó <code>0 &lt; l &lt; n</code>, rồi thêm hậu tố đó vào đầu <code>s</code>.<br />
	Ví dụ, với <code>s = &#39;abcd&#39;</code>, trong một thao tác, bạn có thể xóa hậu tố <code>&#39;cd&#39;</code> và thêm nó vào trước <code>s</code>, khi đó <code>s = &#39;cdab&#39;</code>.</li>
</ul>

<p>Bạn cũng được cho một số nguyên <code>k</code>. Hãy trả về <em>số cách biến đổi </em><code>s</code><em> thành </em><code>t</code><em> sau <strong>đúng</strong> </em><code>k</code><em> thao tác.</em></p>

<p>Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;, t = &quot;cdab&quot;, k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Cách thứ nhất:
Trong thao tác đầu tiên, chọn hậu tố bắt đầu từ chỉ số = 3, khi đó s = &quot;dabc&quot;.
Trong thao tác thứ hai, chọn hậu tố bắt đầu từ chỉ số = 3, khi đó s = &quot;cdab&quot;.

Cách thứ hai:
Trong thao tác đầu tiên, chọn hậu tố bắt đầu từ chỉ số = 1, khi đó s = &quot;bcda&quot;.
Trong thao tác thứ hai, chọn hậu tố bắt đầu từ chỉ số = 1, khi đó s = &quot;cdab&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ababab&quot;, t = &quot;ababab&quot;, k = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Cách thứ nhất:
Chọn hậu tố bắt đầu từ chỉ số = 2, khi đó s = &quot;ababab&quot;.

Cách thứ hai:
Chọn hậu tố bắt đầu từ chỉ số = 4, khi đó s = &quot;ababab&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>15</sup></code></li>
	<li><code>s.length == t.length</code></li>
	<li><code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác xoay $s$ một lần, vì vậy một chuỗi được xác định bởi chỉ số mới của $s[0]$. Mảng Z của $s+t+t$ liệt kê các phép xoay bằng $t$. Sau $k$ bước, ta chỉ cần phân biệt phép xoay ban đầu với các phép xoay còn lại; truy hồi hai trạng thái đó có thể biểu diễn bằng lũy thừa ma trận, sau đó cộng các độ lệch khớp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
"""
DP, Z-algorithm, Fast mod.
Approach
How to represent a string?
Each operation is just a rotation. Each result string can be represented by an integer from 0 to n - 1. Namely, it's just the new index of s[0].
How to find the integer(s) that can represent string t?
Create a new string s + t + t (length = 3 * n).
Use Z-algorithm (or KMP), for each n <= index < 2 * n, calculate the maximum prefix length that each substring starts from index can match, if the length >= n, then (index - n) is a valid integer representation.
How to get the result?
It's a very obvious DP.
If we use an integer to represent a string, we only need to consider the transition from zero to non-zero and from non-zero to zero. In other words, all the non-zero strings should have the same result.
So let dp[t][i = 0/1] be the number of ways to get the zero/nonzero string
after excatly t steps.
Then
dp[t][0] = dp[t - 1][1] * (n - 1).
All the non zero strings can make it.
dp[t][1] = dp[t - 1][0] + dp[t - 1] * (n - 2).
For a particular non zero string, all the other non zero strings and zero string can make it.
We have dp[0][0] = 1 and dp[0][1] = 0
Use matrix multiplication.
How to calculate dp[k][x = 0, 1] faster?
Use matrix multiplication
vector (dp[t - 1][0], dp[t - 1][1])
multiplies matrix
[0 1]
[n - 1 n - 2]
== vector (dp[t][0], dp[t - 1][1]).
So we just need to calculate the kth power of the matrix which can be done by fast power algorith.
Complexity
Time complexity:
O(n + logk)
Space complexity:
O(n)
"""


class Solution:
    M: int = 1000000007

    def add(self, x: int, y: int) -> int:
        x += y
        if x >= self.M:
            x -= self.M
        return x

    def mul(self, x: int, y: int) -> int:
        return int(x * y % self.M)

    def getZ(self, s: str) -> List[int]:
        n = len(s)
        z = [0] * n
        left = right = 0
        for i in range(1, n):
            if i <= right and z[i - left] <= right - i:
                z[i] = z[i - left]
            else:
                z_i = max(0, right - i + 1)
                while i + z_i < n and s[i + z_i] == s[z_i]:
                    z_i += 1
                z[i] = z_i
            if i + z[i] - 1 > right:
                left = i
                right = i + z[i] - 1
        return z

    def matrixMultiply(self, a: List[List[int]], b: List[List[int]]) -> List[List[int]]:
        m = len(a)
        n = len(a[0])
        p = len(b[0])
        r = [[0] * p for _ in range(m)]
        for i in range(m):
            for j in range(p):
                for k in range(n):
                    r[i][j] = self.add(r[i][j], self.mul(a[i][k], b[k][j]))
        return r

    def matrixPower(self, a: List[List[int]], y: int) -> List[List[int]]:
        n = len(a)
        r = [[0] * n for _ in range(n)]
        for i in range(n):
            r[i][i] = 1
        x = [a[i][:] for i in range(n)]
        while y > 0:
            if y & 1:
                r = self.matrixMultiply(r, x)
            x = self.matrixMultiply(x, x)
            y >>= 1
        return r

    def numberOfWays(self, s: str, t: str, k: int) -> int:
        n = len(s)
        dp = self.matrixPower([[0, 1], [n - 1, n - 2]], k)[0]
        s += t + t
        z = self.getZ(s)
        m = n + n
        result = 0
        for i in range(n, m):
            if z[i] >= n:
                result = self.add(result, dp[0] if i - n == 0 else dp[1])
        return result
```

#### Java

```java
class Solution {
    private static final int M = 1000000007;

    private int add(int x, int y) {
        if ((x += y) >= M) {
            x -= M;
        }
        return x;
    }

    private int mul(long x, long y) {
        return (int) (x * y % M);
    }

    private int[] getZ(String s) {
        int n = s.length();
        int[] z = new int[n];
        for (int i = 1, left = 0, right = 0; i < n; ++i) {
            if (i <= right && z[i - left] <= right - i) {
                z[i] = z[i - left];
            } else {
                int z_i = Math.max(0, right - i + 1);
                while (i + z_i < n && s.charAt(i + z_i) == s.charAt(z_i)) {
                    z_i++;
                }
                z[i] = z_i;
            }
            if (i + z[i] - 1 > right) {
                left = i;
                right = i + z[i] - 1;
            }
        }
        return z;
    }

    private int[][] matrixMultiply(int[][] a, int[][] b) {
        int m = a.length, n = a[0].length, p = b[0].length;
        int[][] r = new int[m][p];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < p; ++j) {
                for (int k = 0; k < n; ++k) {
                    r[i][j] = add(r[i][j], mul(a[i][k], b[k][j]));
                }
            }
        }
        return r;
    }

    private int[][] matrixPower(int[][] a, long y) {
        int n = a.length;
        int[][] r = new int[n][n];
        for (int i = 0; i < n; ++i) {
            r[i][i] = 1;
        }
        int[][] x = new int[n][n];
        for (int i = 0; i < n; ++i) {
            System.arraycopy(a[i], 0, x[i], 0, n);
        }
        while (y > 0) {
            if ((y & 1) == 1) {
                r = matrixMultiply(r, x);
            }
            x = matrixMultiply(x, x);
            y >>= 1;
        }
        return r;
    }

    public int numberOfWays(String s, String t, long k) {
        int n = s.length();
        int[] dp = matrixPower(new int[][] {{0, 1}, {n - 1, n - 2}}, k)[0];
        s += t + t;
        int[] z = getZ(s);
        int m = n + n;
        int result = 0;
        for (int i = n; i < m; ++i) {
            if (z[i] >= n) {
                result = add(result, dp[i - n == 0 ? 0 : 1]);
            }
        }
        return result;
    }
}
```

#### C++

```cpp
class Solution {
    const int M = 1000000007;

    int add(int x, int y) {
        if ((x += y) >= M) {
            x -= M;
        }
        return x;
    }

    int mul(long long x, long long y) {
        return x * y % M;
    }

    vector<int> getz(const string& s) {
        const int n = s.length();
        vector<int> z(n);
        for (int i = 1, left = 0, right = 0; i < n; ++i) {
            if (i <= right && z[i - left] <= right - i) {
                z[i] = z[i - left];
            } else {
                for (z[i] = max(0, right - i + 1); i + z[i] < n && s[i + z[i]] == s[z[i]]; ++z[i])
                    ;
            }
            if (i + z[i] - 1 > right) {
                left = i;
                right = i + z[i] - 1;
            }
        }
        return z;
    }

    vector<vector<int>> mul(const vector<vector<int>>& a, const vector<vector<int>>& b) {
        const int m = a.size(), n = a[0].size(), p = b[0].size();
        vector<vector<int>> r(m, vector<int>(p));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < p; ++k) {
                    r[i][k] = add(r[i][k], mul(a[i][j], b[j][k]));
                }
            }
        }
        return r;
    }

    vector<vector<int>> pow(const vector<vector<int>>& a, long long y) {
        const int n = a.size();
        vector<vector<int>> r(n, vector<int>(n));
        for (int i = 0; i < n; ++i) {
            r[i][i] = 1;
        }
        auto x = a;
        for (; y; y >>= 1) {
            if (y & 1) {
                r = mul(r, x);
            }
            x = mul(x, x);
        }
        return r;
    }

public:
    int numberOfWays(string s, string t, long long k) {
        const int n = s.length();
        const auto dp = pow({{0, 1}, {n - 1, n - 2}}, k)[0];
        s.append(t);
        s.append(t);
        const auto z = getz(s);
        const int m = n + n;
        int r = 0;
        for (int i = n; i < m; ++i) {
            if (z[i] >= n) {
                r = add(r, dp[!!(i - n)]);
            }
        }
        return r;
    }
};
```

#### Go

```go
func numberOfWays(s string, t string, k int64) int {
	const mod = 1_000_000_007
	add := func(x, y int) int {
		x += y
		if x >= mod {
			x -= mod
		}
		return x
	}
	mul := func(x, y int) int {
		return int(int64(x) * int64(y) % mod)
	}
	getZ := func(s string) []int {
		n := len(s)
		z := make([]int, n)
		left, right := 0, 0
		for i := 1; i < n; i++ {
			if i <= right && z[i-left] <= right-i {
				z[i] = z[i-left]
			} else {
				zI := right - i + 1
				if zI < 0 {
					zI = 0
				}
				for i+zI < n && s[i+zI] == s[zI] {
					zI++
				}
				z[i] = zI
			}
			if i+z[i]-1 > right {
				left = i
				right = i + z[i] - 1
			}
		}
		return z
	}
	matrixMultiply := func(a, b [][]int) [][]int {
		m, n, p := len(a), len(a[0]), len(b[0])
		r := make([][]int, m)
		for i := range r {
			r[i] = make([]int, p)
			for j := 0; j < p; j++ {
				for k := 0; k < n; k++ {
					r[i][j] = add(r[i][j], mul(a[i][k], b[k][j]))
				}
			}
		}
		return r
	}
	matrixPower := func(a [][]int, y int64) [][]int {
		n := len(a)
		r := make([][]int, n)
		x := make([][]int, n)
		for i := 0; i < n; i++ {
			r[i] = make([]int, n)
			x[i] = make([]int, n)
			r[i][i] = 1
			copy(x[i], a[i])
		}
		for y > 0 {
			if y&1 == 1 {
				r = matrixMultiply(r, x)
			}
			x = matrixMultiply(x, x)
			y >>= 1
		}
		return r
	}
	n := len(s)
	dp := matrixPower([][]int{{0, 1}, {n - 1, n - 2}}, k)[0]
	s += t + t
	z := getZ(s)
	result := 0
	for i := n; i < n+n; i++ {
		if z[i] >= n {
			if i-n == 0 {
				result = add(result, dp[0])
			} else {
				result = add(result, dp[1])
			}
		}
	}
	return result
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
