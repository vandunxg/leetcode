---
comments: true
difficulty: Medium
rating: 1806
source: Weekly Contest 421 Q2
tags:
    - Hash Table
    - Math
    - String
    - Dynamic Programming
    - Counting
---

<!-- problem:start -->

# [3335. Total Characters in String After Transformations I](https://leetcode.com/problems/total-characters-in-string-after-transformations-i)

[中文文档](/solution/3300-3399/3335.Total%20Characters%20in%20String%20After%20Transformations%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> và một số nguyên <code>t</code>, biểu thị số lần <strong>biến đổi</strong> cần thực hiện. Trong một lần <strong>biến đổi</strong>, mọi ký tự trong <code>s</code> được thay thế theo các quy tắc sau:</p>

<ul>
    <li>Nếu ký tự là <code>&#39;z&#39;</code>, thay thế nó bằng chuỗi <code>&quot;ab&quot;</code>.</li>
    <li>Nếu không, thay thế nó bằng ký tự <strong>tiếp theo</strong> trong bảng chữ cái. Ví dụ, <code>&#39;a&#39;</code> được thay thế bằng <code>&#39;b&#39;</code>, <code>&#39;b&#39;</code> được thay thế bằng <code>&#39;c&#39;</code>, và cứ tiếp tục như vậy.</li>
</ul>

<p>Trả về <strong>độ dài</strong> của chuỗi kết quả sau <strong>đúng</strong> <code>t</code> lần biến đổi.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án theo <strong>modulo</strong><!-- notionvc: eb142f2b-b818-4064-8be5-e5a36b07557a --> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcyy&quot;, t = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Biến đổi thứ nhất (t = 1)</strong>:

    <ul>
    <li><code>&#39;a&#39;</code> trở thành <code>&#39;b&#39;</code></li>
    <li><code>&#39;b&#39;</code> trở thành <code>&#39;c&#39;</code></li>
    <li><code>&#39;c&#39;</code> trở thành <code>&#39;d&#39;</code></li>
    <li><code>&#39;y&#39;</code> trở thành <code>&#39;z&#39;</code></li>
    <li><code>&#39;y&#39;</code> trở thành <code>&#39;z&#39;</code></li>
    <li>Chuỗi sau lần biến đổi thứ nhất: <code>&quot;bcdzz&quot;</code></li>
    </ul>
    </li>
    <li><strong>Biến đổi thứ hai (t = 2)</strong>:
    <ul>
    <li><code>&#39;b&#39;</code> trở thành <code>&#39;c&#39;</code></li>
    <li><code>&#39;c&#39;</code> trở thành <code>&#39;d&#39;</code></li>
    <li><code>&#39;d&#39;</code> trở thành <code>&#39;e&#39;</code></li>
    <li><code>&#39;z&#39;</code> trở thành <code>&quot;ab&quot;</code></li>
    <li><code>&#39;z&#39;</code> trở thành <code>&quot;ab&quot;</code></li>
    <li>Chuỗi sau lần biến đổi thứ hai: <code>&quot;cdeabab&quot;</code></li>
    </ul>
    </li>
    <li><strong>Độ dài cuối cùng của chuỗi</strong>: Chuỗi là <code>&quot;cdeabab&quot;</code>, có 7 ký tự.</li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;azbk&quot;, t = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Biến đổi thứ nhất (t = 1)</strong>:

    <ul>
    <li><code>&#39;a&#39;</code> trở thành <code>&#39;b&#39;</code></li>
    <li><code>&#39;z&#39;</code> trở thành <code>&quot;ab&quot;</code></li>
    <li><code>&#39;b&#39;</code> trở thành <code>&#39;c&#39;</code></li>
    <li><code>&#39;k&#39;</code> trở thành <code>&#39;l&#39;</code></li>
    <li>Chuỗi sau lần biến đổi thứ nhất: <code>&quot;babcl&quot;</code></li>
    </ul>
    </li>
    <li><strong>Độ dài cuối cùng của chuỗi</strong>: Chuỗi là <code>&quot;babcl&quot;</code>, có 5 ký tự.</li>

</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
    <li><code>1 &lt;= t &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Truy hồi

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước dịch một chữ cái về phía trước, còn $\texttt{z}$ trở thành $\texttt{ab}$. Với $t \le 10^5$, ta không thể xây dựng lại cả chuỗi.
>
> Chỉ có số lần xuất hiện của $26$ chữ cái là quan trọng. $f[i][j]$ là số lần xuất hiện của chữ cái $j$ sau $i$ bước: $\texttt{z}$ tạo ra $\texttt{a}$ và $\texttt{b}$, còn mọi chữ cái khác đến từ chữ cái đứng trước nó.
>
> Đáp án là tổng các phần tử của $f[t]$, lấy modulo $10^9+7$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số lần xuất hiện của chữ cái thứ $j$ trong bảng chữ cái sau $i$ lần biến đổi. Ban đầu, $f[0][j]$ là số lần xuất hiện của chữ cái thứ $j$ trong chuỗi $s$.

Sau mỗi lần biến đổi, số lần xuất hiện của chữ cái thứ $j$ trong bảng chữ cái được tính như sau:

$$
\begin{align*}
f[i][0] &= f[i - 1][25] \\
f[i][1] &= f[i - 1][0] + f[i - 1][25] \\
f[i][2] &= f[i - 1][1] \\
f[i][3] &= f[i - 1][2] \\
&\vdots \\
f[i][25] &= f[i - 1][24]
\end{align*}
$$

Đáp án là $f[t][0] + f[t][1] + \ldots + f[t][25]$.

Vì đáp án có thể rất lớn, ta lấy kết quả theo modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(t \times |\Sigma|)$, và độ phức tạp không gian là $O(t \times |\Sigma|)$, trong đó $|\Sigma|$ là kích thước của bảng chữ cái.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lengthAfterTransformations(self, s: str, t: int) -> int:
        f = [[0] * 26 for _ in range(t + 1)]
        for c in s:
            f[0][ord(c) - ord("a")] += 1
        for i in range(1, t + 1):
            f[i][0] = f[i - 1][25]
            f[i][1] = f[i - 1][0] + f[i - 1][25]
            for j in range(2, 26):
                f[i][j] = f[i - 1][j - 1]
        mod = 10**9 + 7
        return sum(f[t]) % mod
```

#### Java

```java
class Solution {
    public int lengthAfterTransformations(String s, int t) {
        final int mod = (int) 1e9 + 7;
        int[][] f = new int[t + 1][26];
        for (char c : s.toCharArray()) {
            f[0][c - 'a']++;
        }
        for (int i = 1; i <= t; ++i) {
            f[i][0] = f[i - 1][25] % mod;
            f[i][1] = (f[i - 1][0] + f[i - 1][25]) % mod;
            for (int j = 2; j < 26; j++) {
                f[i][j] = f[i - 1][j - 1] % mod;
            }
        }

        int ans = 0;
        for (int j = 0; j < 26; ++j) {
            ans = (ans + f[t][j]) % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int lengthAfterTransformations(string s, int t) {
        const int mod = 1e9 + 7;
        vector<vector<int>> f(t + 1, vector<int>(26, 0));

        for (char c : s) {
            f[0][c - 'a']++;
        }

        for (int i = 1; i <= t; ++i) {
            f[i][0] = f[i - 1][25] % mod;
            f[i][1] = (f[i - 1][0] + f[i - 1][25]) % mod;
            for (int j = 2; j < 26; ++j) {
                f[i][j] = f[i - 1][j - 1] % mod;
            }
        }

        int ans = 0;
        for (int j = 0; j < 26; ++j) {
            ans = (ans + f[t][j]) % mod;
        }

        return ans;
    }
};
```

#### Go

```go
func lengthAfterTransformations(s string, t int) int {
    const mod = 1_000_000_007
    f := make([][]int, t+1)
    for i := range f {
        f[i] = make([]int, 26)
    }

    for _, c := range s {
        f[0][c-'a']++
    }

    for i := 1; i <= t; i++ {
        f[i][0] = f[i-1][25] % mod
        f[i][1] = (f[i-1][0] + f[i-1][25]) % mod
        for j := 2; j < 26; j++ {
            f[i][j] = f[i-1][j-1] % mod
        }
    }

    ans := 0
    for j := 0; j < 26; j++ {
        ans = (ans + f[t][j]) % mod
    }
    return ans
}
```

#### TypeScript

```ts
function lengthAfterTransformations(s: string, t: number): number {
    const mod = 1_000_000_007;
    const f: number[][] = Array.from({ length: t + 1 }, () => Array(26).fill(0));

    for (const c of s) {
        f[0][c.charCodeAt(0) - 'a'.charCodeAt(0)]++;
    }

    for (let i = 1; i <= t; i++) {
        f[i][0] = f[i - 1][25] % mod;
        f[i][1] = (f[i - 1][0] + f[i - 1][25]) % mod;
        for (let j = 2; j < 26; j++) {
            f[i][j] = f[i - 1][j - 1] % mod;
        }
    }

    let ans = 0;
    for (let j = 0; j < 26; j++) {
        ans = (ans + f[t][j]) % mod;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
