---
comments: true
difficulty: Hard
rating: 2817
source: Biweekly Contest 59 Q4
tags:
    - String
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [1977. Number of Ways to Separate Numbers](https://leetcode.com/problems/number-of-ways-to-separate-numbers)

[中文文档](/solution/1900-1999/1977.Number%20of%20Ways%20to%20Separate%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đã viết nhiều số nguyên <strong>dương</strong> vào một chuỗi có tên là <code>num</code>. Tuy nhiên, bạn nhận ra rằng mình đã quên thêm dấu phẩy để phân tách các số. Bạn nhớ rằng danh sách các số nguyên là <strong>không giảm</strong> và <strong>không</strong> có số nguyên nào bắt đầu bằng số 0.</p>

<p>Hãy trả về <em><strong>số lượng danh sách số nguyên có thể có</strong> mà bạn đã viết ra để tạo thành chuỗi </em><code>num</code>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;327&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Bạn có thể đã viết các số:
3, 27
327
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;094&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không số nào có thể bắt đầu bằng số 0 và tất cả các số đều phải dương.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;0&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không số nào có thể bắt đầu bằng số 0 và tất cả các số đều phải dương.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 3500</code></li>
	<li><code>num</code> chỉ gồm các chữ số từ <code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta chia chuỗi chữ số thành các số nguyên không giảm và không có số 0 ở đầu. Với $n\le 3500$, số lượng cách cắt tăng theo cấp số nhân, nhưng có thể chuyển bài toán thành quy hoạch động $O(n^2)$.
>
> $dp[i][j]$ là số cách tạo thành $i$ chữ số đầu tiên, trong đó phần cuối có độ dài $j$. Một phần trước ngắn hơn luôn nhỏ hơn và được cộng thông qua tổng tiền tố; các phần có cùng độ dài được so sánh bằng bảng LCP.
>
> $dp[i][j]$ cũng lưu thông tin của tiền tố đó, nên đáp án là $dp[n][n]$.

<!-- thinking:end -->

Gọi $dp[i][j]$ là số cách phân hoạch $i$ ký tự đầu tiên của chuỗi `num` sao cho số cuối cùng có độ dài $j$. Rõ ràng, đáp án là $\sum_{j=0}^{n} dp[n][j]$. Giá trị khởi tạo là $dp[0][0] = 1$.

Với $dp[i][j]$, số cuối cùng trước đó phải kết thúc tại vị trí $i-j$. Ta có thể liệt kê $dp[i-j][k]$, trong đó $k \le j$. Với trường hợp $k < j$, tức là số cách có độ dài nhỏ hơn $j$, ta có thể cộng trực tiếp vào $dp[i][j]$, nghĩa là $dp[i][j] = \sum_{k=0}^{j-1} dp[i-j][k]$. Vì số trước đó ngắn hơn nên nó nhỏ hơn số hiện tại. Có thể dùng tổng tiền tố để tối ưu.

Tuy nhiên, khi $k = j$, ta cần so sánh kích thước của hai số có cùng độ dài. Nếu số trước đó lớn hơn số hiện tại thì trường hợp này không hợp lệ và không được cộng vào $dp[i][j]$. Ngược lại, ta có thể cộng nó vào $dp[i][j]$. Ta có thể tiền xử lý "tiền tố chung dài nhất" trong thời gian $O(n^2)$, sau đó so sánh kích thước của hai số có cùng độ dài trong thời gian $O(1)$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài của chuỗi `num`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfCombinations(self, num: str) -> int:
        def cmp(i, j, k):
            x = lcp[i][j]
            return x >= k or num[i + x] >= num[j + x]

        mod = 10**9 + 7
        n = len(num)
        lcp = [[0] * (n + 1) for _ in range(n + 1)]
        for i in range(n - 1, -1, -1):
            for j in range(n - 1, -1, -1):
                if num[i] == num[j]:
                    lcp[i][j] = 1 + lcp[i + 1][j + 1]

        dp = [[0] * (n + 1) for _ in range(n + 1)]
        dp[0][0] = 1
        for i in range(1, n + 1):
            for j in range(1, i + 1):
                v = 0
                if num[i - j] != '0':
                    if i - j - j >= 0 and cmp(i - j, i - j - j, j):
                        v = dp[i - j][j]
                    else:
                        v = dp[i - j][min(j - 1, i - j)]
                dp[i][j] = (dp[i][j - 1] + v) % mod
        return dp[n][n]
```

#### Java

```java
class Solution {
    public int numberOfCombinations(String num) {
        final int mod = (int) 1e9 + 7;
        int n = num.length();
        int[][] lcp = new int[n + 1][n + 1];
        for (int i = n - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                if (num.charAt(i) == num.charAt(j)) {
                    lcp[i][j] = 1 + lcp[i + 1][j + 1];
                }
            }
        }
        int[][] dp = new int[n + 1][n + 1];
        dp[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= i; ++j) {
                int v = 0;
                if (num.charAt(i - j) != '0') {
                    if (i - j - j >= 0) {
                        int x = lcp[i - j][i - j - j];
                        if (x >= j || num.charAt(i - j + x) >= num.charAt(i - j - j + x)) {
                            v = dp[i - j][j];
                        }
                    }
                    if (v == 0) {
                        v = dp[i - j][Math.min(j - 1, i - j)];
                    }
                }
                dp[i][j] = (dp[i][j - 1] + v) % mod;
            }
        }
        return dp[n][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfCombinations(string num) {
        const int mod = 1e9 + 7;
        int n = num.size();
        vector<vector<int>> lcp(n + 1, vector<int>(n + 1));
        for (int i = n - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                if (num[i] == num[j]) {
                    lcp[i][j] = 1 + lcp[i + 1][j + 1];
                }
            }
        }
        auto cmp = [&](int i, int j, int k) {
            int x = lcp[i][j];
            return x >= k || num[i + x] >= num[j + x];
        };
        vector<vector<int>> dp(n + 1, vector<int>(n + 1));
        dp[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= i; ++j) {
                int v = 0;
                if (num[i - j] != '0') {
                    if (i - j - j >= 0 && cmp(i - j, i - j - j, j)) {
                        v = dp[i - j][j];
                    } else {
                        v = dp[i - j][min(j - 1, i - j)];
                    }
                }
                dp[i][j] = (dp[i][j - 1] + v) % mod;
            }
        }
        return dp[n][n];
    }
};
```

#### Go

```go
func numberOfCombinations(num string) int {
	n := len(num)
	lcp := make([][]int, n+1)
	dp := make([][]int, n+1)
	for i := range lcp {
		lcp[i] = make([]int, n+1)
		dp[i] = make([]int, n+1)
	}
	for i := n - 1; i >= 0; i-- {
		for j := n - 1; j >= 0; j-- {
			if num[i] == num[j] {
				lcp[i][j] = 1 + lcp[i+1][j+1]
			}
		}
	}
	cmp := func(i, j, k int) bool {
		x := lcp[i][j]
		return x >= k || num[i+x] >= num[j+x]
	}
	dp[0][0] = 1
	var mod int = 1e9 + 7
	for i := 1; i <= n; i++ {
		for j := 1; j <= i; j++ {
			v := 0
			if num[i-j] != '0' {
				if i-j-j >= 0 && cmp(i-j, i-j-j, j) {
					v = dp[i-j][j]
				} else {
					v = dp[i-j][min(j-1, i-j)]
				}
			}
			dp[i][j] = (dp[i][j-1] + v) % mod
		}
	}
	return dp[n][n]
}
```

#### TypeScript

```ts
function numberOfCombinations(num: string): number {
    const n: number = num.length;
    const mod: number = 1_000_000_007;

    const lcp: number[][] = Array.from({ length: n + 1 }, () => Array(n + 1).fill(0));
    const dp: number[][] = Array.from({ length: n + 1 }, () => Array(n + 1).fill(0));

    for (let i = n - 1; i >= 0; i--) {
        for (let j = n - 1; j >= 0; j--) {
            if (num[i] === num[j]) {
                lcp[i][j] = 1 + lcp[i + 1][j + 1];
            }
        }
    }

    function cmp(i: number, j: number, k: number): boolean {
        const x: number = lcp[i][j];
        return x >= k || num[i + x] >= num[j + x];
    }

    dp[0][0] = 1;

    for (let i = 1; i <= n; i++) {
        for (let j = 1; j <= i; j++) {
            let v: number = 0;
            if (num[i - j] !== '0') {
                if (i - j - j >= 0 && cmp(i - j, i - j - j, j)) {
                    v = dp[i - j][j];
                } else {
                    v = dp[i - j][Math.min(j - 1, i - j)];
                }
            }
            dp[i][j] = (dp[i][j - 1] + v) % mod;
        }
    }

    return dp[n][n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
