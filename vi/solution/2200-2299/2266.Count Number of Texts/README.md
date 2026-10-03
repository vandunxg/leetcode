---
comments: true
difficulty: Medium
rating: 1856
source: Weekly Contest 292 Q3
tags:
    - Hash Table
    - Math
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2266. Count Number of Texts](https://leetcode.com/problems/count-number-of-texts)

[中文文档](/solution/2200-2299/2266.Count%20Number%20of%20Texts/README.md)

## Mô tả

<!-- description:start -->

<p>Alice nhắn tin cho Bob bằng điện thoại. <strong>Ánh xạ</strong> các chữ số với các chữ cái được hiển thị trong hình bên dưới.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2266.Count%20Number%20of%20Texts/images/1200px-telephone-keypad2svg.png" style="width: 200px; height: 162px;" />
<p>Để <strong>nhập</strong> một chữ cái, Alice phải <strong>nhấn</strong> phím của chữ số tương ứng <code>i</code> lần, trong đó <code>i</code> là vị trí của chữ cái trên phím.</p>

<ul>
	<li>Ví dụ, để nhập chữ cái <code>&#39;s&#39;</code>, Alice phải nhấn phím <code>&#39;7&#39;</code> bốn lần. Tương tự, để nhập chữ cái <code>&#39;k&#39;</code>, Alice phải nhấn phím <code>&#39;5&#39;</code> hai lần.</li>
	<li>Lưu ý rằng các chữ số <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code> không ánh xạ với chữ cái nào, nên Alice <strong>không</strong> sử dụng chúng.</li>
</ul>

<p>Tuy nhiên, do lỗi truyền tin, Bob không nhận được tin nhắn của Alice mà nhận được một <strong>chuỗi các phím đã nhấn</strong>.</p>

<ul>
	<li>Ví dụ, khi Alice gửi tin nhắn <code>&quot;bob&quot;</code>, Bob nhận được chuỗi <code>&quot;2266622&quot;</code>.</li>
</ul>

<p>Cho một chuỗi <code>pressedKeys</code> biểu diễn chuỗi mà Bob nhận được, hãy trả về <em><strong>tổng số tin nhắn có thể có</strong> mà Alice đã gửi</em>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> pressedKeys = &quot;22233&quot;
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong>
Các tin nhắn có thể mà Alice đã gửi là:
&quot;aaadd&quot;, &quot;abdd&quot;, &quot;badd&quot;, &quot;cdd&quot;, &quot;aaae&quot;, &quot;abe&quot;, &quot;bae&quot; và &quot;ce&quot;.
Vì có 8 tin nhắn có thể, ta trả về 8.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> pressedKeys = &quot;222222222222222222222222222222222222&quot;
<strong>Đầu ra:</strong> 82876089
<strong>Giải thích:</strong>
Có 2082876103 tin nhắn có thể mà Alice đã gửi.
Vì cần trả về đáp án theo modulo 10<sup>9</sup> + 7, ta trả về 2082876103 % (10<sup>9</sup> + 7) = 82876089.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= pressedKeys.length &lt;= 10<sup>5</sup></code></li>
	<li><code>pressedKeys</code> chỉ gồm các chữ số từ <code>&#39;2&#39;</code> đến <code>&#39;9&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Gom nhóm + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Một đoạn liên tiếp gồm cùng một phím có thể được chia thành các nhóm có độ dài không vượt quá số chữ cái của phím đó. Với $n \le 10^5$, không thể duyệt toàn bộ chuỗi. Các đoạn độc lập với nhau nên ta nhân số cách của chúng. Một đoạn có độ dài $m$ tạo thành một dãy truy hồi: các phím khác $7$ và $9$ có các bước $1..3$, còn hai phím đó có các bước $1..4$.
>
> Ta tiền xử lý $f[i]$ và $g[i]$ đến $10^5$, sau đó nhân kết quả của từng đoạn liên tiếp trong $\textit{groupby}$ theo modulo $10^9+7$.

<!-- thinking:end -->

Theo mô tả bài toán, với các ký tự giống nhau liên tiếp trong chuỗi $\textit{pressedKeys}$, ta có thể gom chúng thành các nhóm rồi tính số cách cho từng nhóm. Cuối cùng, ta nhân số cách của tất cả các nhóm.

Vấn đề chính là tính số cách cho từng nhóm.

Nếu một nhóm gồm các ký tự '7' hoặc '9', ta có thể coi $1$, $2$, $3$ hoặc $4$ ký tự cuối của nhóm là một chữ cái, sau đó giảm độ dài nhóm và chuyển thành một bài toán con nhỏ hơn.

Tương tự, nếu một nhóm gồm các ký tự '2', '3', '4', '5', '6' hoặc '8', ta có thể coi $1$, $2$ hoặc $3$ ký tự cuối của nhóm là một chữ cái, sau đó giảm độ dài nhóm và chuyển thành một bài toán con nhỏ hơn.

Do đó, ta định nghĩa $f[i]$ là số cách của một nhóm có độ dài $i$ gồm các ký tự giống nhau nhưng không phải '7' hoặc '9', và $g[i]$ là số cách của một nhóm có độ dài $i$ gồm các ký tự giống nhau là '7' hoặc '9'.

Ban đầu, $f[0] = f[1] = 1$, $f[2] = 2$, $f[3] = 4$, $g[0] = g[1] = 1$, $g[2] = 2$, $g[3] = 4$.

Với $i \ge 4$, ta có:

$$
\begin{aligned}
f[i] & = f[i-1] + f[i-2] + f[i-3] \\
g[i] & = g[i-1] + g[i-2] + g[i-3] + g[i-4]
\end{aligned}
$$

Cuối cùng, ta duyệt $\textit{pressedKeys}$, gom các ký tự giống nhau liên tiếp, tính số cách cho từng nhóm rồi nhân số cách của tất cả các nhóm.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $\textit{pressedKeys}$.

<!-- tabs:start -->

#### Python3

```python
mod = 10**9 + 7
f = [1, 1, 2, 4]
g = [1, 1, 2, 4]
for _ in range(100000):
    f.append((f[-1] + f[-2] + f[-3]) % mod)
    g.append((g[-1] + g[-2] + g[-3] + g[-4]) % mod)


class Solution:
    def countTexts(self, pressedKeys: str) -> int:
        ans = 1
        for c, s in groupby(pressedKeys):
            m = len(list(s))
            ans = ans * (g[m] if c in "79" else f[m]) % mod
        return ans
```

#### Java

```java
class Solution {
    private static final int N = 100010;
    private static final int MOD = (int) 1e9 + 7;
    private static long[] f = new long[N];
    private static long[] g = new long[N];
    static {
        f[0] = f[1] = 1;
        f[2] = 2;
        f[3] = 4;
        g[0] = g[1] = 1;
        g[2] = 2;
        g[3] = 4;
        for (int i = 4; i < N; ++i) {
            f[i] = (f[i - 1] + f[i - 2] + f[i - 3]) % MOD;
            g[i] = (g[i - 1] + g[i - 2] + g[i - 3] + g[i - 4]) % MOD;
        }
    }

    public int countTexts(String pressedKeys) {
        long ans = 1;
        for (int i = 0, n = pressedKeys.length(); i < n; ++i) {
            char c = pressedKeys.charAt(i);
            int j = i;
            while (j + 1 < n && pressedKeys.charAt(j + 1) == c) {
                ++j;
            }
            int cnt = j - i + 1;
            ans = c == '7' || c == '9' ? ans * g[cnt] : ans * f[cnt];
            ans %= MOD;
            i = j;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
const int mod = 1e9 + 7;
const int n = 1e5 + 10;
long long f[n], g[n];

int init = []() {
    f[0] = g[0] = 1;
    f[1] = g[1] = 1;
    f[2] = g[2] = 2;
    f[3] = g[3] = 4;
    for (int i = 4; i < n; ++i) {
        f[i] = (f[i - 1] + f[i - 2] + f[i - 3]) % mod;
        g[i] = (g[i - 1] + g[i - 2] + g[i - 3] + g[i - 4]) % mod;
    }
    return 0;
}();

class Solution {
public:
    int countTexts(string pressedKeys) {
        long long ans = 1;
        for (int i = 0, n = pressedKeys.length(); i < n; ++i) {
            char c = pressedKeys[i];
            int j = i;
            while (j + 1 < n && pressedKeys[j + 1] == c) {
                ++j;
            }
            int cnt = j - i + 1;
            ans = c == '7' || c == '9' ? ans * g[cnt] : ans * f[cnt];
            ans %= mod;
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
const mod int = 1e9 + 7
const n int = 1e5 + 10

var f = [n]int{1, 1, 2, 4}
var g = f

func init() {
	for i := 4; i < n; i++ {
		f[i] = (f[i-1] + f[i-2] + f[i-3]) % mod
		g[i] = (g[i-1] + g[i-2] + g[i-3] + g[i-4]) % mod
	}
}

func countTexts(pressedKeys string) int {
	ans := 1
	for i, j, n := 0, 0, len(pressedKeys); i < n; i++ {
		c := pressedKeys[i]
		j = i
		for j+1 < n && pressedKeys[j+1] == c {
			j++
		}
		cnt := j - i + 1
		if c == '7' || c == '9' {
			ans = ans * g[cnt] % mod
		} else {
			ans = ans * f[cnt] % mod
		}
		i = j
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
