---
comments: true
difficulty: Hard
rating: 2010
source: Weekly Contest 458 Q3
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [3614. Process String with Special Operations II](https://leetcode.com/problems/process-string-with-special-operations-ii)

[中文文档](/solution/3600-3699/3614.Process%20String%20with%20Special%20Operations%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường và các ký tự đặc biệt: <code>&#39;*&#39;</code>, <code>&#39;#&#39;</code> và <code>&#39;%&#39;</code>.</p>

<p>Cho thêm một số nguyên <code>k</code>.</p>

<p>Hãy xây dựng một chuỗi mới <code>result</code> bằng cách xử lý <code>s</code> từ trái sang phải theo các quy tắc sau:</p>

<ul>
    <li>Nếu ký tự là một chữ cái tiếng Anh <strong>viết thường</strong>, thêm ký tự đó vào <code>result</code>.</li>
    <li><code>&#39;*&#39;</code> <strong>xóa</strong> ký tự cuối cùng khỏi <code>result</code>, nếu ký tự đó tồn tại.</li>
    <li><code>&#39;#&#39;</code> <strong>nhân đôi</strong> <code>result</code> hiện tại và <strong>nối</strong> nó vào chính nó.</li>
    <li><code>&#39;%&#39;</code> <strong>đảo ngược</strong> <code>result</code> hiện tại.</li>
</ul>

<p>Trả về ký tự thứ <code>k<sup>th</sup></code> trong chuỗi cuối cùng <code>result</code>. Nếu <code>k</code> nằm ngoài phạm vi của <code>result</code>, trả về <code>&#39;.&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;a#b%*&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;a&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th style="border: 1px solid black;"><code>i</code></th>
            <th style="border: 1px solid black;"><code>s[i]</code></th>
            <th style="border: 1px solid black;">Thao tác</th>
            <th style="border: 1px solid black;"><code>result</code> hiện tại</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;"><code>&#39;a&#39;</code></td>
            <td style="border: 1px solid black;">Thêm <code>&#39;a&#39;</code></td>
            <td style="border: 1px solid black;"><code>&quot;a&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;"><code>&#39;#&#39;</code></td>
            <td style="border: 1px solid black;">Nhân đôi <code>result</code></td>
            <td style="border: 1px solid black;"><code>&quot;aa&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;"><code>&#39;b&#39;</code></td>
            <td style="border: 1px solid black;">Thêm <code>&#39;b&#39;</code></td>
            <td style="border: 1px solid black;"><code>&quot;aab&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">3</td>
            <td style="border: 1px solid black;"><code>&#39;%&#39;</code></td>
            <td style="border: 1px solid black;">Đảo ngược <code>result</code></td>
            <td style="border: 1px solid black;"><code>&quot;baa&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">4</td>
            <td style="border: 1px solid black;"><code>&#39;*&#39;</code></td>
            <td style="border: 1px solid black;">Xóa ký tự cuối cùng</td>
            <td style="border: 1px solid black;"><code>&quot;ba&quot;</code></td>
        </tr>
    </tbody>
</table>

<p>Chuỗi <code>result</code> cuối cùng là <code>&quot;ba&quot;</code>. Ký tự tại chỉ số <code>k = 1</code> là <code>&#39;a&#39;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;cd%#*#&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;d&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th style="border: 1px solid black;"><code>i</code></th>
            <th style="border: 1px solid black;"><code>s[i]</code></th>
            <th style="border: 1px solid black;">Thao tác</th>
            <th style="border: 1px solid black;"><code>result</code> hiện tại</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;"><code>&#39;c&#39;</code></td>
            <td style="border: 1px solid black;">Thêm <code>&#39;c&#39;</code></td>
            <td style="border: 1px solid black;"><code>&quot;c&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;"><code>&#39;d&#39;</code></td>
            <td style="border: 1px solid black;">Thêm <code>&#39;d&#39;</code></td>
            <td style="border: 1px solid black;"><code>&quot;cd&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;"><code>&#39;%&#39;</code></td>
            <td style="border: 1px solid black;">Đảo ngược <code>result</code></td>
            <td style="border: 1px solid black;"><code>&quot;dc&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">3</td>
            <td style="border: 1px solid black;"><code>&#39;#&#39;</code></td>
            <td style="border: 1px solid black;">Nhân đôi <code>result</code></td>
            <td style="border: 1px solid black;"><code>&quot;dcdc&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">4</td>
            <td style="border: 1px solid black;"><code>&#39;*&#39;</code></td>
            <td style="border: 1px solid black;">Xóa ký tự cuối cùng</td>
            <td style="border: 1px solid black;"><code>&quot;dcd&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">5</td>
            <td style="border: 1px solid black;"><code>&#39;#&#39;</code></td>
            <td style="border: 1px solid black;">Nhân đôi <code>result</code></td>
            <td style="border: 1px solid black;"><code>&quot;dcddcd&quot;</code></td>
        </tr>
    </tbody>
</table>

<p>Chuỗi <code>result</code> cuối cùng là <code>&quot;dcddcd&quot;</code>. Ký tự tại chỉ số <code>k = 3</code> là <code>&#39;d&#39;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;z*#&quot;, k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;.&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th style="border: 1px solid black;"><code>i</code></th>
            <th style="border: 1px solid black;"><code>s[i]</code></th>
            <th style="border: 1px solid black;">Thao tác</th>
            <th style="border: 1px solid black;"><code>result</code> hiện tại</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;"><code>&#39;z&#39;</code></td>
            <td style="border: 1px solid black;">Thêm <code>&#39;z&#39;</code></td>
            <td style="border: 1px solid black;"><code>&quot;z&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;"><code>&#39;*&#39;</code></td>
            <td style="border: 1px solid black;">Xóa ký tự cuối cùng</td>
            <td style="border: 1px solid black;"><code>&quot;&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;"><code>&#39;#&#39;</code></td>
            <td style="border: 1px solid black;">Nhân đôi chuỗi</td>
            <td style="border: 1px solid black;"><code>&quot;&quot;</code></td>
        </tr>
    </tbody>
</table>

<p>Chuỗi <code>result</code> cuối cùng là <code>&quot;&quot;</code>. Vì chỉ số <code>k = 0</code> nằm ngoài phạm vi, đầu ra là <code>&#39;.&#39;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và các ký tự đặc biệt <code>&#39;*&#39;</code>, <code>&#39;#&#39;</code> và <code>&#39;%&#39;</code>.</li>
    <li><code>0 &lt;= k &lt;= 10<sup>15</sup></code></li>
    <li>Độ dài của <code>result</code> sau khi xử lý <code>s</code> không vượt quá <code>10<sup>15</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Theo dõi ngược

<!-- thinking:start -->

> **Tư duy**
>
> Không giống bài trước, ta chỉ cần biết chỉ số $k$ trong result, nhưng `#` làm tăng gấp đôi độ dài nên không thể dựng trực tiếp chuỗi.
>
> Khi duyệt xuôi, ta chỉ cần theo dõi độ dài $m$: chữ cái tăng thêm một, `*` giảm một nhưng không nhỏ hơn $0$, còn `#` dịch trái. Nếu $k\ge m$, trả về `'.'`.
>
> Khi duyệt ngược, ta ánh xạ $k$ về chữ cái nguồn. Với `#`, chia đôi $m$ và trừ đi nửa đầu nếu $k$ nằm ở nửa sau; `%` đưa $k$ về $m-1-k$; với một chữ cái, giảm $m$, và khi $k=m$ sau phép giảm đó thì ta đã tìm thấy ký tự cần trả về.

<!-- thinking:end -->

Trước tiên, ta tính độ dài $m$ của chuỗi kết quả đã xử lý $\textit{result}$. Nếu $k \geq m$, nghĩa là $k$ vượt quá các chỉ số hợp lệ của chuỗi kết quả, nên ta trả về '.'.

Sau đó, ta duyệt chuỗi $s$ theo thứ tự ngược và xử lý từng ký tự theo các trường hợp sau:

1. Nếu $s[i]$ là '\*', ta tăng $m$ lên $1$.
2. Nếu $s[i]$ là '#', ta chia $m$ cho $2$. Khi đó, nếu $k \geq m$, ta trừ $m$ khỏi $k$.
3. Nếu $s[i]$ là '%', ta cập nhật $k$ thành $m - 1 - k$.
4. Nếu không, $s[i]$ là một chữ cái. Ta giảm $m$ đi $1$. Nếu $k = m$, nghĩa là ta đã tìm thấy ký tự thứ $k$, nên trả về $s[i]$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def processStr(self, s: str, k: int) -> str:
        m = 0
        for c in s:
            if c == "*":
                m = max(0, m - 1)
            elif c == "#":
                m <<= 1
            elif c != "%":
                m += 1
        if k >= m:
            return "."
        for c in reversed(s):
            if c == "*":
                m += 1
            elif c == "#":
                m //= 2
                if k >= m:
                    k -= m
            elif c == "%":
                k = m - 1 - k
            else:
                m -= 1
                if k == m:
                    return c
```

#### Java

```java
class Solution {
    public char processStr(String s, long k) {
        long m = 0;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == '*') {
                m = Math.max(0, m - 1);
            } else if (c == '#') {
                m <<= 1;
            } else if (c != '%') {
                m += 1;
            }
        }
        if (k >= m) {
            return '.';
        }
        for (int i = s.length() - 1;; i--) {
            char c = s.charAt(i);
            if (c == '*') {
                m += 1;
            } else if (c == '#') {
                m /= 2;
                if (k >= m) {
                    k -= m;
                }
            } else if (c == '%') {
                k = m - 1 - k;
            } else {
                m -= 1;
                if (k == m) {
                    return c;
                }
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    char processStr(string s, long long k) {
        long long m = 0;
        for (char c : s) {
            if (c == '*') {
                m = max(0LL, m - 1);
            } else if (c == '#') {
                m <<= 1;
            } else if (c != '%') {
                m += 1;
            }
        }
        if (k >= m) {
            return '.';
        }
        for (int i = s.length() - 1;; i--) {
            char c = s[i];
            if (c == '*') {
                m += 1;
            } else if (c == '#') {
                m /= 2;
                if (k >= m) {
                    k -= m;
                }
            } else if (c == '%') {
                k = m - 1 - k;
            } else {
                m -= 1;
                if (k == m) {
                    return c;
                }
            }
        }
    }
};
```

#### Go

```go
func processStr(s string, k int64) byte {
    var m int64 = 0
    for i := 0; i < len(s); i++ {
        c := s[i]
        if c == '*' {
            if m-1 > 0 {
                m = m - 1
            } else {
                m = 0
            }
        } else if c == '#' {
            m <<= 1
        } else if c != '%' {
            m += 1
        }
    }
    if k >= m {
        return '.'
    }
    for i := len(s) - 1; ; i-- {
        c := s[i]
        if c == '*' {
            m += 1
        } else if c == '#' {
            m /= 2
            if k >= m {
                k -= m
            }
        } else if c == '%' {
            k = m - 1 - k
        } else {
            m -= 1
            if k == m {
                return c
            }
        }
    }
}
```

#### TypeScript

```ts
function processStr(s: string, k: number): string {
    let m = 0n;
    for (let i = 0; i < s.length; i++) {
        const c = s[i];
        if (c === '*') {
            const sub = m - 1n;
            m = sub > 0n ? sub : 0n;
        } else if (c === '#') {
            m <<= 1n;
        } else if (c !== '%') {
            m += 1n;
        }
    }
    if (BigInt(k) >= m) {
        return '.';
    }
    let bigK = BigInt(k);
    for (let i = s.length - 1; ; i--) {
        const c = s[i];
        if (c === '*') {
            m += 1n;
        } else if (c === '#') {
            m /= 2n;
            if (bigK >= m) {
                bigK -= m;
            }
        } else if (c === '%') {
            bigK = m - 1n - bigK;
        } else {
            m -= 1n;
            if (bigK === m) {
                return c;
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
