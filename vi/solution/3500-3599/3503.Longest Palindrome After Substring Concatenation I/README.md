---
comments: true
difficulty: Medium
rating: 1548
source: Weekly Contest 443 Q2
tags:
    - Two Pointers
    - String
    - Dynamic Programming
    - Enumeration
---

<!-- problem:start -->

# [3503. Longest Palindrome After Substring Concatenation I](https://leetcode.com/problems/longest-palindrome-after-substring-concatenation-i)

[Tài liệu tiếng Trung](/solution/3500-3599/3503.Longest%20Palindrome%20After%20Substring%20Concatenation%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>t</code>.</p>

<p>Bạn có thể tạo một chuỗi mới bằng cách chọn một <span data-keyword="substring">substring</span> từ <code>s</code> (có thể rỗng) và một substring từ <code>t</code> (có thể rỗng), sau đó nối chúng <strong>theo đúng thứ tự</strong>.</p>

<p>Trả về độ dài của <span data-keyword="palindrome-string">palindrome</span> <strong>dài nhất</strong> có thể tạo theo cách này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;a&quot;, t = &quot;a&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Nối <code>&quot;a&quot;</code> từ <code>s</code> và <code>&quot;a&quot;</code> từ <code>t</code> cho kết quả <code>&quot;aa&quot;</code>, đây là một palindrome có độ dài 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;, t = &quot;def&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì mọi ký tự đều khác nhau, palindrome dài nhất là bất kỳ ký tự đơn nào, nên đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;b&quot;, t = &quot;aaaa&quot;</span></p>

<p><strong>Đầu ra:</strong> 4</p>

<p><strong>Giải thích:</strong></p>

<p>Chọn &quot;<code>aaaa</code>&quot; từ <code>t</code> cho palindrome dài nhất, nên đáp án là 4.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcde&quot;, t = &quot;ecdba&quot;</span></p>

<p><strong>Đầu ra:</strong> 5</p>

<p><strong>Giải thích:</strong></p>

<p>Nối <code>&quot;abc&quot;</code> từ <code>s</code> và <code>&quot;ba&quot;</code> từ <code>t</code> cho kết quả <code>&quot;abcba&quot;</code>, đây là một palindrome có độ dài 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length, t.length &lt;= 30</code></li>
    <li><code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê tâm palindrome + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Palindrome dài nhất nằm hoàn toàn trong $s$ hoặc $t$ có thể được tìm bằng cách mở rộng quanh các tâm, nhưng phép nối có thể lấy một đoạn từ mỗi chuỗi rồi mở rộng một lõi palindrome ở một phía.
>
> Sau khi đảo ngược $t$, tiền tố chung của $s$ và $t$ đã đảo ngược là một cặp đối xứng từ hai chuỗi ban đầu. $f[i][j]$ lưu độ dài phần khớp đó; nối thêm palindrome dài nhất bắt đầu tại $i$ trong $s$ hoặc tại $j$ trong $t$ đã đảo ngược sẽ bao quát cả trường hợp “chỉ một phía” lẫn “khớp rồi mở rộng”.

<!-- thinking:end -->

Theo mô tả bài toán, chuỗi palindrome sau khi nối có thể được tạo hoàn toàn từ chuỗi $s$, hoàn toàn từ chuỗi $t$, hoặc kết hợp cả hai chuỗi $s$ và $t$. Ngoài ra, mỗi chuỗi $s$ hoặc $t$ có thể có thêm các substring palindrome.

Do đó, trước tiên chúng ta đảo ngược chuỗi $t$ và tiền xử lý các mảng $\textit{g1}$ và $\textit{g2}$, trong đó $\textit{g1}[i]$ biểu diễn độ dài của substring palindrome dài nhất bắt đầu tại chỉ số $i$ trong chuỗi $s$, còn $\textit{g2}[i]$ biểu diễn độ dài của substring palindrome dài nhất bắt đầu tại chỉ số $i$ trong chuỗi $t$.

Chúng ta có thể khởi tạo đáp án $\textit{ans}$ bằng giá trị lớn nhất trong $\textit{g1}$ và $\textit{g2}$.

Tiếp theo, ta định nghĩa $\textit{f}[i][j]$ là độ dài của substring palindrome kết thúc tại ký tự thứ $i$ của chuỗi $s$ và ký tự thứ $j$ của chuỗi $t$.

Với $\textit{f}[i][j]$, nếu $s[i - 1]$ bằng $t[j - 1]$, thì $\textit{f}[i][j] = \textit{f}[i - 1][j - 1] + 1$. Sau đó, ta cập nhật đáp án:

$$
\textit{ans} = \max(\textit{ans}, \textit{f}[i][j] \times 2 + (0 \text{ if } i \geq m \text{ else } \textit{g1}[i])) \\
\textit{ans} = \max(\textit{ans}, \textit{f}[i][j] \times 2 + (0 \text{ if } j \geq n \text{ else } \textit{g2}[j]))
$$

Cuối cùng, ta trả về đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(m \times (m + n))$, và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là độ dài của các chuỗi $s$ và $t$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestPalindrome(self, s: str, t: str) -> int:
        def expand(s: str, g: List[int], l: int, r: int):
            while l >= 0 and r < len(s) and s[l] == s[r]:
                g[l] = max(g[l], r - l + 1)
                l, r = l - 1, r + 1

        def calc(s: str) -> List[int]:
            n = len(s)
            g = [0] * n
            for i in range(n):
                expand(s, g, i, i)
                expand(s, g, i, i + 1)
            return g

        m, n = len(s), len(t)
        t = t[::-1]
        g1, g2 = calc(s), calc(t)
        ans = max(*g1, *g2)
        f = [[0] * (n + 1) for _ in range(m + 1)]
        for i, a in enumerate(s, 1):
            for j, b in enumerate(t, 1):
                if a == b:
                    f[i][j] = f[i - 1][j - 1] + 1
                    ans = max(ans, f[i][j] * 2 + (0 if i >= m else g1[i]))
                    ans = max(ans, f[i][j] * 2 + (0 if j >= n else g2[j]))
        return ans
```

#### Java

```java
class Solution {
    public int longestPalindrome(String S, String T) {
        char[] s = S.toCharArray();
        char[] t = new StringBuilder(T).reverse().toString().toCharArray();
        int m = s.length, n = t.length;
        int[] g1 = calc(s), g2 = calc(t);
        int ans = Math.max(Arrays.stream(g1).max().getAsInt(), Arrays.stream(g2).max().getAsInt());
        int[][] f = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (s[i - 1] == t[j - 1]) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                    ans = Math.max(ans, f[i][j] * 2 + (i < m ? g1[i] : 0));
                    ans = Math.max(ans, f[i][j] * 2 + (j < n ? g2[j] : 0));
                }
            }
        }
        return ans;
    }

    private void expand(char[] s, int[] g, int l, int r) {
        while (l >= 0 && r < s.length && s[l] == s[r]) {
            g[l] = Math.max(g[l], r - l + 1);
            --l;
            ++r;
        }
    }

    private int[] calc(char[] s) {
        int n = s.length;
        int[] g = new int[n];
        for (int i = 0; i < n; ++i) {
            expand(s, g, i, i);
            expand(s, g, i, i + 1);
        }
        return g;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestPalindrome(string s, string t) {
        int m = s.size(), n = t.size();
        ranges::reverse(t);
        vector<int> g1 = calc(s), g2 = calc(t);
        int ans = max(ranges::max(g1), ranges::max(g2));
        vector<vector<int>> f(m + 1, vector<int>(n + 1));
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (s[i - 1] == t[j - 1]) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                    ans = max(ans, f[i][j] * 2 + (i < m ? g1[i] : 0));
                    ans = max(ans, f[i][j] * 2 + (j < n ? g2[j] : 0));
                }
            }
        }
        return ans;
    }

private:
    void expand(const string& s, vector<int>& g, int l, int r) {
        while (l >= 0 && r < s.size() && s[l] == s[r]) {
            g[l] = max(g[l], r - l + 1);
            --l;
            ++r;
        }
    }

    vector<int> calc(const string& s) {
        int n = s.size();
        vector<int> g(n, 0);
        for (int i = 0; i < n; ++i) {
            expand(s, g, i, i);
            expand(s, g, i, i + 1);
        }
        return g;
    }
};
```

#### Go

```go
func longestPalindrome(s, t string) int {
    m, n := len(s), len(t)
    t = reverse(t)

    g1, g2 := calc(s), calc(t)
    ans := max(slices.Max(g1), slices.Max(g2))

    f := make([][]int, m+1)
    for i := range f {
        f[i] = make([]int, n+1)
    }

    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if s[i-1] == t[j-1] {
                f[i][j] = f[i-1][j-1] + 1
                a, b := 0, 0
                if i < m {
                    a = g1[i]
                }
                if j < n {
                    b = g2[j]
                }
                ans = max(ans, f[i][j]*2+a)
                ans = max(ans, f[i][j]*2+b)
            }
        }
    }
    return ans
}

func calc(s string) []int {
    n, g := len(s), make([]int, len(s))
    for i := 0; i < n; i++ {
        expand(s, g, i, i)
        expand(s, g, i, i+1)
    }
    return g
}

func expand(s string, g []int, l, r int) {
    for l >= 0 && r < len(s) && s[l] == s[r] {
        g[l] = max(g[l], r-l+1)
        l, r = l-1, r+1
    }
}

func reverse(s string) string {
    r := []rune(s)
    slices.Reverse(r)
    return string(r)
}
```

#### TypeScript

```ts
function longestPalindrome(s: string, t: string): number {
    function expand(s: string, g: number[], l: number, r: number): void {
        while (l >= 0 && r < s.length && s[l] === s[r]) {
            g[l] = Math.max(g[l], r - l + 1);
            l--;
            r++;
        }
    }

    function calc(s: string): number[] {
        const n = s.length;
        const g: number[] = Array(n).fill(0);
        for (let i = 0; i < n; i++) {
            expand(s, g, i, i);
            expand(s, g, i, i + 1);
        }
        return g;
    }

    const m = s.length,
        n = t.length;
    t = t.split('').reverse().join('');
    const g1 = calc(s);
    const g2 = calc(t);
    let ans = Math.max(...g1, ...g2);

    const f: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (s[i - 1] === t[j - 1]) {
                f[i][j] = f[i - 1][j - 1] + 1;
                ans = Math.max(ans, f[i][j] * 2 + (i >= m ? 0 : g1[i]));
                ans = Math.max(ans, f[i][j] * 2 + (j >= n ? 0 : g2[j]));
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
