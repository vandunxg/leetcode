---
comments: true
difficulty: Easy
rating: 1250
source: Biweekly Contest 81 Q1
tags:
    - String
---

<!-- problem:start -->

# [2315. Count Asterisks](https://leetcode.com/problems/count-asterisks)

[中文文档](/solution/2300-2399/2315.Count%20Asterisks/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code>, trong đó cứ <strong>hai</strong> ký tự gạch đứng liên tiếp <code>&#39;|&#39;</code> được nhóm thành một <strong>cặp</strong>. Nói cách khác, ký tự <code>&#39;|&#39;</code> thứ 1<sup>st</sup> và thứ 2<sup>nd</sup> tạo thành một cặp, ký tự <code>&#39;|&#39;</code> thứ 3<sup>rd</sup> và thứ 4<sup>th</sup> tạo thành một cặp, v.v.</p>

<p>Trả về <em>số lượng </em><code>&#39;*&#39;</code><em> trong </em><code>s</code><em>, <strong>không tính</strong> các </em><code>&#39;*&#39;</code><em> nằm giữa mỗi cặp </em><code>&#39;|&#39;</code>.</p>

<p><strong>Lưu ý</strong> rằng mỗi <code>&#39;|&#39;</code> sẽ thuộc về <strong>chính xác</strong> một cặp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;l|*e*et|c**o|*de|&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các ký tự được xét được gạch chân: &quot;<u>l</u>|*e*et|<u>c**o</u>|*de|&quot;.
Các ký tự nằm giữa dấu &#39;|&#39; thứ nhất và thứ hai bị loại khỏi đáp án.
Ngoài ra, các ký tự nằm giữa dấu &#39;|&#39; thứ ba và thứ tư cũng bị loại khỏi đáp án.
Có 2 dấu hoa thị được xét. Do đó, ta trả về 2.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;iamprogrammer&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Trong ví dụ này, không có dấu hoa thị nào trong s. Do đó, ta trả về 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;yo|uar|e**|b|e***au|tifu|l&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Các ký tự được xét được gạch chân: &quot;<u>yo</u>|uar|<u>e**</u>|b|<u>e***au</u>|tifu|<u>l</u>&quot;. Có 5 dấu hoa thị được xét. Do đó, ta trả về 5.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 1000</code></li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường, dấu gạch đứng <code>&#39;|&#39;</code> và dấu hoa thị <code>&#39;*&#39;</code>.</li>
    <li><code>s</code> chứa một số lượng dấu gạch đứng <strong>chẵn</strong> <code>&#39;|&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các dấu gạch đứng được ghép thành từng cặp; ta đếm các dấu hoa thị nằm ngoài mọi cặp. $|s| \le 1000$, vì vậy chỉ cần duyệt một lần.
>
> Một cờ $\textit{ok}$ cho biết ta đang ở bên ngoài một cặp hay không. Đảo trạng thái của cờ mỗi khi gặp `|`, và chỉ đếm `*` khi cờ đang bật. Không cần tách chuỗi.

<!-- thinking:end -->

Ta định nghĩa một biến nguyên $\textit{ok}$ để cho biết có thể đếm khi gặp `*` hay không. Ban đầu, $\textit{ok}=1$, nghĩa là có thể đếm.

Duyệt chuỗi $s$. Nếu gặp `*`, ta quyết định có đếm hay không dựa trên giá trị của $\textit{ok}$. Nếu gặp `|`, ta đảo giá trị của $\textit{ok}$.

Cuối cùng, trả về kết quả đếm.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countAsterisks(self, s: str) -> int:
        ans, ok = 0, 1
        for c in s:
            if c == "*":
                ans += ok
            elif c == "|":
                ok ^= 1
        return ans
```

#### Java

```java
class Solution {
    public int countAsterisks(String s) {
        int ans = 0;
        for (int i = 0, ok = 1; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (c == '*') {
                ans += ok;
            } else if (c == '|') {
                ok ^= 1;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countAsterisks(string s) {
        int ans = 0, ok = 1;
        for (char& c : s) {
            if (c == '*') {
                ans += ok;
            } else if (c == '|') {
                ok ^= 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countAsterisks(s string) (ans int) {
    ok := 1
    for _, c := range s {
        if c == '*' {
            ans += ok
        } else if c == '|' {
            ok ^= 1
        }
    }
    return
}
```

#### TypeScript

```ts
function countAsterisks(s: string): number {
    let ans = 0;
    let ok = 1;
    for (const c of s) {
        if (c === '*') {
            ans += ok;
        } else if (c === '|') {
            ok ^= 1;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_asterisks(s: String) -> i32 {
        let mut ans = 0;
        let mut ok = 1;
        for &c in s.as_bytes() {
            if c == b'*' {
                ans += ok;
            } else if c == b'|' {
                ok ^= 1;
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int CountAsterisks(string s) {
        int ans = 0, ok = 1;
        foreach (char c in s) {
            if (c == '*') {
                ans += ok;
            } else if (c == '|') {
                ok ^= 1;
            }
        }
        return ans;
    }
}
```

#### C

```c
int countAsterisks(char* s) {
    int ans = 0;
    int ok = 1;
    for (int i = 0; s[i]; i++) {
        if (s[i] == '*') {
            ans += ok;
        } else if (s[i] == '|') {
            ok ^= 1;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
