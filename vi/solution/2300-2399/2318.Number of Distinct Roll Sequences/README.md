---
comments: true
difficulty: Hard
rating: 2090
source: Biweekly Contest 81 Q4
tags:
    - Memoization
    - Dynamic Programming
---

<!-- problem:start -->

# [2318. Number of Distinct Roll Sequences](https://leetcode.com/problems/number-of-distinct-roll-sequences)

[中文文档](/solution/2300-2399/2318.Number%20of%20Distinct%20Roll%20Sequences/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>. Bạn gieo một con xúc xắc công bằng có 6 mặt <code>n</code> lần. Hãy xác định tổng số <strong>chuỗi khác nhau</strong> có thể tạo ra sao cho thỏa mãn các điều kiện sau:</p>

<ol>
	<li><strong>Ước chung lớn nhất</strong> của mọi cặp giá trị <strong>liền kề</strong> trong chuỗi bằng <code>1</code>.</li>
	<li>Có <strong>ít nhất</strong> <code>2</code> lần gieo ở giữa các lần gieo có <strong>cùng</strong> giá trị. Cụ thể hơn, nếu giá trị của lần gieo thứ <code>i<sup>th</sup></code> <strong>bằng</strong> giá trị của lần gieo thứ <code>j<sup>th</sup></code>, thì <code>abs(i - j) &gt; 2</code>.</li>
</ol>

<p>Hãy trả về <em><strong>tổng số</strong> chuỗi khác nhau có thể tạo ra</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Hai chuỗi được xem là khác nhau nếu có ít nhất một phần tử khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> 184
<strong>Giải thích:</strong> Một số chuỗi có thể tạo ra là (1, 2, 3, 4), (6, 1, 2, 3), (1, 2, 3, 1), v.v.
Một số chuỗi không hợp lệ là (1, 2, 1, 3), (1, 2, 3, 6).
(1, 2, 1, 3) không hợp lệ vì lần gieo thứ nhất và thứ ba có cùng giá trị, đồng thời abs(1 - 3) = 2 (i và j được đánh số từ 1).
(1, 2, 3, 6) không hợp lệ vì ước chung lớn nhất của 3 và 6 = 3.
Có tổng cộng 184 chuỗi khác nhau có thể tạo ra, nên ta trả về 184.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 22
<strong>Giải thích:</strong> Một số chuỗi có thể tạo ra là (1, 2), (2, 1), (3, 2).
Một số chuỗi không hợp lệ là (3, 6), (2, 4) vì ước chung lớn nhất khác 1.
Có tổng cộng 22 chuỗi khác nhau có thể tạo ra, nên ta trả về 22.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với các chuỗi có độ dài $n \le 10^4$, các mặt liền kề phải nguyên tố cùng nhau và khác nhau, đồng thời mặt cách đó hai vị trí cũng phải khác nhau. Đệ quy thuần túy sẽ tính lặp lại các hậu tố.
>
> Tính hợp lệ chỉ phụ thuộc vào hai mặt cuối cùng. Gọi $dp[k][i][j]$ là số chuỗi độ dài $k$ kết thúc bằng $i,j$, được chuyển từ một mặt trước đó thỏa mãn các điều kiện về ước chung lớn nhất và tính khác nhau. Vì chỉ có sáu mặt nên bảng này có kích thước nhỏ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distinctSequences(self, n: int) -> int:
        if n == 1:
            return 6
        mod = 10**9 + 7
        dp = [[[0] * 6 for _ in range(6)] for _ in range(n + 1)]
        for i in range(6):
            for j in range(6):
                if gcd(i + 1, j + 1) == 1 and i != j:
                    dp[2][i][j] = 1
        for k in range(3, n + 1):
            for i in range(6):
                for j in range(6):
                    if gcd(i + 1, j + 1) == 1 and i != j:
                        for h in range(6):
                            if gcd(h + 1, i + 1) == 1 and h != i and h != j:
                                dp[k][i][j] += dp[k - 1][h][i]
        ans = 0
        for i in range(6):
            for j in range(6):
                ans += dp[-1][i][j]
        return ans % mod
```

#### Java

```java
class Solution {
    public int distinctSequences(int n) {
        if (n == 1) {
            return 6;
        }
        int mod = (int) 1e9 + 7;
        int[][][] dp = new int[n + 1][6][6];
        for (int i = 0; i < 6; ++i) {
            for (int j = 0; j < 6; ++j) {
                if (gcd(i + 1, j + 1) == 1 && i != j) {
                    dp[2][i][j] = 1;
                }
            }
        }
        for (int k = 3; k <= n; ++k) {
            for (int i = 0; i < 6; ++i) {
                for (int j = 0; j < 6; ++j) {
                    if (gcd(i + 1, j + 1) == 1 && i != j) {
                        for (int h = 0; h < 6; ++h) {
                            if (gcd(h + 1, i + 1) == 1 && h != i && h != j) {
                                dp[k][i][j] = (dp[k][i][j] + dp[k - 1][h][i]) % mod;
                            }
                        }
                    }
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < 6; ++i) {
            for (int j = 0; j < 6; ++j) {
                ans = (ans + dp[n][i][j]) % mod;
            }
        }
        return ans;
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
    int distinctSequences(int n) {
        if (n == 1) return 6;
        int mod = 1e9 + 7;
        vector<vector<vector<int>>> dp(n + 1, vector<vector<int>>(6, vector<int>(6)));
        for (int i = 0; i < 6; ++i)
            for (int j = 0; j < 6; ++j)
                if (gcd(i + 1, j + 1) == 1 && i != j)
                    dp[2][i][j] = 1;
        for (int k = 3; k <= n; ++k)
            for (int i = 0; i < 6; ++i)
                for (int j = 0; j < 6; ++j)
                    if (gcd(i + 1, j + 1) == 1 && i != j)
                        for (int h = 0; h < 6; ++h)
                            if (gcd(h + 1, i + 1) == 1 && h != i && h != j)
                                dp[k][i][j] = (dp[k][i][j] + dp[k - 1][h][i]) % mod;
        int ans = 0;
        for (int i = 0; i < 6; ++i)
            for (int j = 0; j < 6; ++j)
                ans = (ans + dp[n][i][j]) % mod;
        return ans;
    }

    int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
};
```

#### Go

```go
func distinctSequences(n int) int {
	if n == 1 {
		return 6
	}
	dp := make([][][]int, n+1)
	for k := range dp {
		dp[k] = make([][]int, 6)
		for i := range dp[k] {
			dp[k][i] = make([]int, 6)
		}
	}
	for i := 0; i < 6; i++ {
		for j := 0; j < 6; j++ {
			if gcd(i+1, j+1) == 1 && i != j {
				dp[2][i][j] = 1
			}
		}
	}
	mod := int(1e9) + 7
	for k := 3; k <= n; k++ {
		for i := 0; i < 6; i++ {
			for j := 0; j < 6; j++ {
				if gcd(i+1, j+1) == 1 && i != j {
					for h := 0; h < 6; h++ {
						if gcd(h+1, i+1) == 1 && h != i && h != j {
							dp[k][i][j] = (dp[k][i][j] + dp[k-1][h][i]) % mod
						}
					}
				}
			}
		}
	}
	ans := 0
	for i := 0; i < 6; i++ {
		for j := 0; j < 6; j++ {
			ans = (ans + dp[n][i][j]) % mod
		}
	}
	return ans
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
