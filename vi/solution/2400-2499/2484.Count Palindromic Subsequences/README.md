---
comments: true
difficulty: Hard
rating: 2223
source: Biweekly Contest 92 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2484. Count Palindromic Subsequences](https://leetcode.com/problems/count-palindromic-subsequences)

[中文文档](/solution/2400-2499/2484.Count%20Palindromic%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi các chữ số <code>s</code>, hãy trả về <em>số lượng <strong>dãy con đối xứng</strong> của </em><code>s</code><em> có độ dài </em><code>5</code>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Một chuỗi là <strong>đối xứng</strong> nếu khi đọc từ trái sang phải hay từ phải sang trái đều giống nhau.</li>
	<li><strong>Dãy con</strong> là một chuỗi có thể được tạo từ một chuỗi khác bằng cách xóa một số hoặc không xóa ký tự nào mà không thay đổi thứ tự của các ký tự còn lại.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;103301&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Có 6 dãy con độ dài 5 có thể tạo ra: &quot;10330&quot;,&quot;10331&quot;,&quot;10301&quot;,&quot;10301&quot;,&quot;13301&quot;,&quot;03301&quot;.
Trong đó, có hai dãy con (đều bằng &quot;10301&quot;) là đối xứng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0000000&quot;
<strong>Đầu ra:</strong> 21
<strong>Giải thích:</strong> Cả 21 dãy con đều là &quot;00000&quot;, vốn là một chuỗi đối xứng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;9999900000&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Chỉ có hai dãy con đối xứng là &quot;99999&quot; và &quot;00000&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Một dãy con đối xứng độ dài $5$ có dạng $abxba$. Với $n\le 10^4$ và các chữ số, ta cố định tâm $i$ và một cặp $(j,k)$; sau đó nhân số lần cặp $jk$ xuất hiện bên trái với số lần cặp đó xuất hiện bên phải.
>
> Prefix $\textit{pre}[i][j][k]$ và suffix $\textit{suf}$ đếm các cặp đó: khi gặp $v$, ta thêm $(j,v)$ với mọi $j$ xuất hiện trước đó. Cuối cùng, tính tổng các tích theo modulo $10^9+7$.

<!-- thinking:end -->

Độ phức tạp thời gian là $O(100 \times n)$, và độ phức tạp không gian là $O(100 \times n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPalindromes(self, s: str) -> int:
        mod = 10**9 + 7
        n = len(s)
        pre = [[[0] * 10 for _ in range(10)] for _ in range(n + 2)]
        suf = [[[0] * 10 for _ in range(10)] for _ in range(n + 2)]
        t = list(map(int, s))
        c = [0] * 10
        for i, v in enumerate(t, 1):
            for j in range(10):
                for k in range(10):
                    pre[i][j][k] = pre[i - 1][j][k]
            for j in range(10):
                pre[i][j][v] += c[j]
            c[v] += 1
        c = [0] * 10
        for i in range(n, 0, -1):
            v = t[i - 1]
            for j in range(10):
                for k in range(10):
                    suf[i][j][k] = suf[i + 1][j][k]
            for j in range(10):
                suf[i][j][v] += c[j]
            c[v] += 1
        ans = 0
        for i in range(1, n + 1):
            for j in range(10):
                for k in range(10):
                    ans += pre[i - 1][j][k] * suf[i + 1][j][k]
                    ans %= mod
        return ans
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int countPalindromes(String s) {
        int n = s.length();
        int[][][] pre = new int[n + 2][10][10];
        int[][][] suf = new int[n + 2][10][10];
        int[] t = new int[n];
        for (int i = 0; i < n; ++i) {
            t[i] = s.charAt(i) - '0';
        }
        int[] c = new int[10];
        for (int i = 1; i <= n; ++i) {
            int v = t[i - 1];
            for (int j = 0; j < 10; ++j) {
                for (int k = 0; k < 10; ++k) {
                    pre[i][j][k] = pre[i - 1][j][k];
                }
            }
            for (int j = 0; j < 10; ++j) {
                pre[i][j][v] += c[j];
            }
            c[v]++;
        }
        c = new int[10];
        for (int i = n; i > 0; --i) {
            int v = t[i - 1];
            for (int j = 0; j < 10; ++j) {
                for (int k = 0; k < 10; ++k) {
                    suf[i][j][k] = suf[i + 1][j][k];
                }
            }
            for (int j = 0; j < 10; ++j) {
                suf[i][j][v] += c[j];
            }
            c[v]++;
        }
        long ans = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < 10; ++j) {
                for (int k = 0; k < 10; ++k) {
                    ans += (long) pre[i - 1][j][k] * suf[i + 1][j][k];
                    ans %= MOD;
                }
            }
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;

    int countPalindromes(string s) {
        int n = s.size();
        int pre[n + 2][10][10];
        int suf[n + 2][10][10];
        memset(pre, 0, sizeof pre);
        memset(suf, 0, sizeof suf);
        int t[n];
        for (int i = 0; i < n; ++i) t[i] = s[i] - '0';
        int c[10] = {0};
        for (int i = 1; i <= n; ++i) {
            int v = t[i - 1];
            for (int j = 0; j < 10; ++j) {
                for (int k = 0; k < 10; ++k) {
                    pre[i][j][k] = pre[i - 1][j][k];
                }
            }
            for (int j = 0; j < 10; ++j) {
                pre[i][j][v] += c[j];
            }
            c[v]++;
        }
        memset(c, 0, sizeof c);
        for (int i = n; i > 0; --i) {
            int v = t[i - 1];
            for (int j = 0; j < 10; ++j) {
                for (int k = 0; k < 10; ++k) {
                    suf[i][j][k] = suf[i + 1][j][k];
                }
            }
            for (int j = 0; j < 10; ++j) {
                suf[i][j][v] += c[j];
            }
            c[v]++;
        }
        long ans = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < 10; ++j) {
                for (int k = 0; k < 10; ++k) {
                    ans += 1ll * pre[i - 1][j][k] * suf[i + 1][j][k];
                    ans %= mod;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countPalindromes(s string) int {
	n := len(s)
	pre := [10010][10][10]int{}
	suf := [10010][10][10]int{}
	t := make([]int, n)
	for i, c := range s {
		t[i] = int(c - '0')
	}
	c := [10]int{}
	for i := 1; i <= n; i++ {
		v := t[i-1]
		for j := 0; j < 10; j++ {
			for k := 0; k < 10; k++ {
				pre[i][j][k] = pre[i-1][j][k]
			}
		}
		for j := 0; j < 10; j++ {
			pre[i][j][v] += c[j]
		}
		c[v]++
	}
	c = [10]int{}
	for i := n; i > 0; i-- {
		v := t[i-1]
		for j := 0; j < 10; j++ {
			for k := 0; k < 10; k++ {
				suf[i][j][k] = suf[i+1][j][k]
			}
		}
		for j := 0; j < 10; j++ {
			suf[i][j][v] += c[j]
		}
		c[v]++
	}
	ans := 0
	const mod int = 1e9 + 7
	for i := 1; i <= n; i++ {
		for j := 0; j < 10; j++ {
			for k := 0; k < 10; k++ {
				ans += pre[i-1][j][k] * suf[i+1][j][k]
				ans %= mod
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
