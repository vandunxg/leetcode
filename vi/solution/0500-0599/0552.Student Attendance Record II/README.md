---
comments: true
difficulty: Hard
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [552. Student Attendance Record II](https://leetcode.com/problems/student-attendance-record-ii)

[中文文档](/solution/0500-0599/0552.Student%20Attendance%20Record%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng điểm danh của học sinh có thể được biểu diễn bằng một chuỗi, trong đó mỗi ký tự cho biết hôm đó học sinh vắng, đi muộn hay có mặt. Chuỗi chỉ gồm ba ký tự sau:</p>

<ul>
	<li><code>&#39;A&#39;</code>: Vắng mặt.</li>
	<li><code>&#39;L&#39;</code>: Đi muộn.</li>
	<li><code>&#39;P&#39;</code>: Có mặt.</li>
</ul>

<p>Học sinh đủ điều kiện nhận thưởng chuyên cần nếu đáp ứng <strong>cả hai</strong> tiêu chí sau:</p>

<ul>
	<li>Tổng số ngày học sinh vắng mặt (<code>&#39;A&#39;</code>) <strong>ít hơn</strong> 2 ngày.</li>
	<li>Học sinh <strong>không bao giờ</strong> đi muộn (<code>&#39;L&#39;</code>) trong 3 ngày <strong>liên tiếp</strong> trở lên.</li>
</ul>

<p>Cho số nguyên <code>n</code>, hãy trả về <em><strong>số lượng</strong> bảng điểm danh có độ dài</em> <code>n</code><em> giúp học sinh đủ điều kiện nhận thưởng chuyên cần. Kết quả có thể rất lớn, vì vậy hãy trả về giá trị <strong>modulo</strong> </em><code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Có 8 bảng điểm danh độ dài 2 đủ điều kiện nhận thưởng:
&quot;PP&quot;, &quot;AP&quot;, &quot;PA&quot;, &quot;LP&quot;, &quot;PL&quot;, &quot;AL&quot;, &quot;LA&quot;, &quot;LL&quot;
Chỉ có &quot;AA&quot; không đủ điều kiện vì có 2 ngày vắng mặt (số ngày phải ít hơn 2).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 3
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10101
<strong>Đầu ra:</strong> 183236316
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Bảng điểm danh hợp lệ độ dài $n$ bị giới hạn về số ngày vắng mặt và số ngày đi muộn liên tiếp. Không thể liệt kê $3^n$ chuỗi khi $n$ có thể lên đến $10^5$.
>
> Chỉ cần sáu trạng thái: số ngày vắng đã dùng ($0$ hoặc $1$) và số ngày đi muộn liên tiếp hiện tại ($0$, $1$ hoặc $2$). Cách đệ quy top-down vẫn tạo chuỗi $n$ lần gọi và tràn stack khi $n$ lớn nhất.
>
> Vì vậy, ta xử lý các ngày theo chiều ngược lại. $f(j,k)$ là số cách xếp các ngày còn lại khi đã dùng $j$ ngày vắng và có chuỗi đi muộn hiện tại dài $k$. Sau ngày cuối, giá trị này bằng $1$. Lùi lại một ngày, ta có thể chọn `P` (đưa chuỗi đi muộn về $0$), chọn `A` nếu $j=0$, hoặc chọn `L` nếu $k<2$. Đáp án là $f(0,0)$ modulo $10^9+7$.

<!-- thinking:end -->

Gọi $f(j,k)$ là số cách điền tất cả các ngày còn lại khi đã dùng $j$ ngày vắng và chuỗi đi muộn hiện tại dài $k$. Khi không còn ngày nào, $f(j,k)=1$. Sau $n$ bước tính ngược, đáp án là $f(0,0)$.

Mỗi bước cập nhật bảng theo ba lựa chọn:

- present, which resets the streak: $f(j,0)$;
- absent, only when $j=0$: $f(1,0)$;
- đi muộn, chỉ khi $k<2$: $f(j,k+1)$.

Ghi các giá trị mới vào một bảng riêng để không ghi đè dữ liệu của ngày trước. Lấy modulo $10^9+7$ cho mỗi tổng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkRecord(self, n: int) -> int:
        mod = 10**9 + 7
        f = [[1] * 3 for _ in range(2)]
        for _ in range(n):
            g = [[0] * 3 for _ in range(2)]
            for j in range(2):
                for k in range(3):
                    ans = f[j][0]
                    if j == 0:
                        ans += f[1][0]
                    if k < 2:
                        ans += f[j][k + 1]
                    g[j][k] = ans % mod
            f = g
        return f[0][0]
```

#### Java

```java
class Solution {
    public int checkRecord(int n) {
        final int mod = (int) 1e9 + 7;
        int[][] f = {{1, 1, 1}, {1, 1, 1}};
        for (int i = 0; i < n; ++i) {
            int[][] g = new int[2][3];
            for (int j = 0; j < 2; ++j) {
                for (int k = 0; k < 3; ++k) {
                    int ans = f[j][0];
                    if (j == 0) {
                        ans = (ans + f[1][0]) % mod;
                    }
                    if (k < 2) {
                        ans = (ans + f[j][k + 1]) % mod;
                    }
                    g[j][k] = ans % mod;
                }
            }
            f = g;
        }
        return f[0][0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int checkRecord(int n) {
        const int mod = 1e9 + 7;
        int f[2][3] = {{1, 1, 1}, {1, 1, 1}};
        for (int i = 0; i < n; ++i) {
            int g[2][3]{};
            for (int j = 0; j < 2; ++j) {
                for (int k = 0; k < 3; ++k) {
                    int ans = f[j][0];
                    if (j == 0) {
                        ans = (ans + f[1][0]) % mod;
                    }
                    if (k < 2) {
                        ans = (ans + f[j][k + 1]) % mod;
                    }
                    g[j][k] = ans % mod;
                }
            }
            for (int j = 0; j < 2; ++j) {
                for (int k = 0; k < 3; ++k) {
                    f[j][k] = g[j][k];
                }
            }
        }
        return f[0][0];
    }
};
```

#### Go

```go
func checkRecord(n int) int {
	const mod = int(1e9 + 7)
	f := [2][3]int{{1, 1, 1}, {1, 1, 1}}
	for i := 0; i < n; i++ {
		var g [2][3]int
		for j := 0; j < 2; j++ {
			for k := 0; k < 3; k++ {
				ans := f[j][0]
				if j == 0 {
					ans = (ans + f[1][0]) % mod
				}
				if k < 2 {
					ans = (ans + f[j][k+1]) % mod
				}
				g[j][k] = ans % mod
			}
		}
		f = g
	}
	return f[0][0]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách trước chỉ lưu sáu số đếm cho phần hậu tố. Cách này lưu trạng thái của mọi prefix để chỉ số ngày được thể hiện rõ ràng.
>
> $dp[i][j][k]$ là số cách điểm danh cho $i+1$ ngày đầu tiên với $j$ ngày vắng và chuỗi đi muộn dài $k$. Các chuyển trạng thái thêm `A`, `L` hoặc `P` dựa trên ngày $i-1$. Cộng mọi trạng thái $(j,k)$ ở ngày cuối.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkRecord(self, n: int) -> int:
        mod = int(1e9 + 7)
        dp = [[[0, 0, 0], [0, 0, 0]] for _ in range(n)]

        # base case
        dp[0][0][0] = dp[0][0][1] = dp[0][1][0] = 1

        for i in range(1, n):
            # A
            dp[i][1][0] = (dp[i - 1][0][0] + dp[i - 1][0][1] + dp[i - 1][0][2]) % mod
            # L
            dp[i][0][1] = dp[i - 1][0][0]
            dp[i][0][2] = dp[i - 1][0][1]
            dp[i][1][1] = dp[i - 1][1][0]
            dp[i][1][2] = dp[i - 1][1][1]
            # P
            dp[i][0][0] = (dp[i - 1][0][0] + dp[i - 1][0][1] + dp[i - 1][0][2]) % mod
            dp[i][1][0] = (
                dp[i][1][0] + dp[i - 1][1][0] + dp[i - 1][1][1] + dp[i - 1][1][2]
            ) % mod

        ans = 0
        for j in range(2):
            for k in range(3):
                ans = (ans + dp[n - 1][j][k]) % mod
        return ans
```

#### Java

```java
class Solution {
    private static final int MOD = 1000000007;

    public int checkRecord(int n) {
        long[][][] dp = new long[n][2][3];

        // base case
        dp[0][0][0] = 1;
        dp[0][0][1] = 1;
        dp[0][1][0] = 1;

        for (int i = 1; i < n; i++) {
            // A
            dp[i][1][0] = (dp[i - 1][0][0] + dp[i - 1][0][1] + dp[i - 1][0][2]) % MOD;
            // L
            dp[i][0][1] = dp[i - 1][0][0];
            dp[i][0][2] = dp[i - 1][0][1];
            dp[i][1][1] = dp[i - 1][1][0];
            dp[i][1][2] = dp[i - 1][1][1];
            // P
            dp[i][0][0] = (dp[i - 1][0][0] + dp[i - 1][0][1] + dp[i - 1][0][2]) % MOD;
            dp[i][1][0] = (dp[i][1][0] + dp[i - 1][1][0] + dp[i - 1][1][1] + dp[i - 1][1][2]) % MOD;
        }

        long ans = 0;
        for (int j = 0; j < 2; j++) {
            for (int k = 0; k < 3; k++) {
                ans = (ans + dp[n - 1][j][k]) % MOD;
            }
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
constexpr int MOD = 1e9 + 7;

class Solution {
public:
    int checkRecord(int n) {
        using ll = long long;
        vector<vector<vector<ll>>> dp(n, vector<vector<ll>>(2, vector<ll>(3)));

        // base case
        dp[0][0][0] = dp[0][0][1] = dp[0][1][0] = 1;

        for (int i = 1; i < n; ++i) {
            // A
            dp[i][1][0] = (dp[i - 1][0][0] + dp[i - 1][0][1] + dp[i - 1][0][2]) % MOD;
            // L
            dp[i][0][1] = dp[i - 1][0][0];
            dp[i][0][2] = dp[i - 1][0][1];
            dp[i][1][1] = dp[i - 1][1][0];
            dp[i][1][2] = dp[i - 1][1][1];
            // P
            dp[i][0][0] = (dp[i - 1][0][0] + dp[i - 1][0][1] + dp[i - 1][0][2]) % MOD;
            dp[i][1][0] = (dp[i][1][0] + dp[i - 1][1][0] + dp[i - 1][1][1] + dp[i - 1][1][2]) % MOD;
        }

        ll ans = 0;
        for (int j = 0; j < 2; ++j) {
            for (int k = 0; k < 3; ++k) {
                ans = (ans + dp[n - 1][j][k]) % MOD;
            }
        }
        return ans;
    }
};
```

#### Go

```go
const _mod int = 1e9 + 7

func checkRecord(n int) int {
	dp := make([][][]int, n)
	for i := 0; i < n; i++ {
		dp[i] = make([][]int, 2)
		for j := 0; j < 2; j++ {
			dp[i][j] = make([]int, 3)
		}
	}

	// base case
	dp[0][0][0] = 1
	dp[0][0][1] = 1
	dp[0][1][0] = 1

	for i := 1; i < n; i++ {
		// A
		dp[i][1][0] = (dp[i-1][0][0] + dp[i-1][0][1] + dp[i-1][0][2]) % _mod
		// L
		dp[i][0][1] = dp[i-1][0][0]
		dp[i][0][2] = dp[i-1][0][1]
		dp[i][1][1] = dp[i-1][1][0]
		dp[i][1][2] = dp[i-1][1][1]
		// P
		dp[i][0][0] = (dp[i-1][0][0] + dp[i-1][0][1] + dp[i-1][0][2]) % _mod
		dp[i][1][0] = (dp[i][1][0] + dp[i-1][1][0] + dp[i-1][1][1] + dp[i-1][1][2]) % _mod
	}

	var ans int
	for j := 0; j < 2; j++ {
		for k := 0; k < 3; k++ {
			ans = (ans + dp[n-1][j][k]) % _mod
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
