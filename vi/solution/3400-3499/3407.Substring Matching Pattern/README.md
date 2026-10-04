---
comments: true
difficulty: Easy
rating: 1472
source: Biweekly Contest 147 Q1
tags:
    - String
    - String Matching
---

<!-- problem:start -->

# [3407. Substring Matching Pattern](https://leetcode.com/problems/substring-matching-pattern)

[中文文档](/solution/3400-3499/3407.Substring%20Matching%20Pattern/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một chuỗi mẫu <code>p</code>, trong đó <code>p</code> chứa <strong>đúng một</strong> ký tự <code>&#39;*&#39;</code>.</p>

<p>Ký tự <code>&#39;*&#39;</code> trong <code>p</code> có thể được thay thế bằng một chuỗi gồm không hoặc nhiều ký tự.</p>

<p>Trả về <code>true</code> nếu có thể biến <code>p</code> thành một <span data-keyword="substring">chuỗi con</span> của <code>s</code>, và <code>false</code> nếu không thể.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;leetcode&quot;, p = &quot;ee*e&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thay <code>&#39;*&#39;</code> bằng <code>&quot;tcod&quot;</code>, ta được chuỗi con <code>&quot;eetcode&quot;</code> khớp với mẫu.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;car&quot;, p = &quot;c*v&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có chuỗi con nào khớp với mẫu.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;luck&quot;, p = &quot;u*&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi con <code>&quot;u&quot;</code>, <code>&quot;uc&quot;</code> và <code>&quot;uck&quot;</code> khớp với mẫu.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 50</code></li>
    <li><code>1 &lt;= p.length &lt;= 50 </code></li>
    <li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
    <li><code>p</code> chỉ chứa các chữ cái tiếng Anh viết thường và đúng một ký tự <code>&#39;*&#39;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: String Matching

<!-- thinking:start -->

> **Tư duy**
>
> $p$ chứa một ký tự $*$ duy nhất, và cả hai chuỗi đều có độ dài không vượt quá $50$, nên ngay cả backtracking cũng đáp ứng được giới hạn. Ta chỉ cần kiểm tra các phần ký tự cố định ở hai bên dấu sao xuất hiện theo đúng thứ tự trong $s$, với khoảng cách ở giữa có thể có hoặc không.
>
> Việc khớp $p$ như một regex trên toàn bộ $s$ sẽ nhầm “chuỗi con” với “khớp toàn bộ chuỗi”. Mẫu sau khi thay dấu sao chỉ cần là một chuỗi con nào đó của $s$.
>
> Vì vậy, ta tách $p$ tại $*$, rồi dùng $\textit{find}$ để tìm lần lượt từng phần cố định từ trái sang phải, chỉ di chuyển con trỏ bắt đầu về phía trước. Nếu thiếu bất kỳ phần nào, kết quả là false.

<!-- thinking:end -->

Theo mô tả bài toán, `*` có thể được thay thế bằng một chuỗi gồm không hoặc nhiều ký tự, nên ta có thể tách chuỗi mẫu $p$ tại `*` thành một số chuỗi con. Nếu các chuỗi con này xuất hiện theo đúng thứ tự trong chuỗi $s$, thì $p$ có thể trở thành một chuỗi con của $s$.

Do đó, trước tiên ta khởi tạo con trỏ $i$ tại đầu chuỗi $s$, sau đó duyệt qua từng chuỗi con $t$ thu được bằng cách tách chuỗi mẫu $p$ tại `*`. Với mỗi $t$, ta tìm nó trong $s$ bắt đầu từ vị trí $i$. Nếu tìm thấy, ta đưa con trỏ $i$ đến cuối $t$ và tiếp tục tìm chuỗi con tiếp theo. Nếu không tìm thấy, điều đó có nghĩa là chuỗi mẫu $p$ không thể trở thành một chuỗi con của $s$, và ta trả về $\text{false}$. Nếu tìm thấy tất cả các chuỗi con, ta trả về $\text{true}$.

Độ phức tạp thời gian là $O(n \times m)$, còn độ phức tạp không gian là $O(m)$, trong đó $n$ và $m$ lần lượt là độ dài của các chuỗi $s$ và $p$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hasMatch(self, s: str, p: str) -> bool:
        i = 0
        for t in p.split("*"):
            j = s.find(t, i)
            if j == -1:
                return False
            i = j + len(t)
        return True
```

#### Java

```java
class Solution {
    public boolean hasMatch(String s, String p) {
        int i = 0;
        for (String t : p.split("\\*")) {
            int j = s.indexOf(t, i);
            if (j == -1) {
                return false;
            }
            i = j + t.length();
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool hasMatch(string s, string p) {
        int i = 0;
        int pos = 0;
        int start = 0, end;
        while ((end = p.find("*", start)) != string::npos) {
            string t = p.substr(start, end - start);
            pos = s.find(t, i);
            if (pos == string::npos) {
                return false;
            }
            i = pos + t.length();
            start = end + 1;
        }
        string t = p.substr(start);
        pos = s.find(t, i);
        if (pos == string::npos) {
            return false;
        }
        return true;
    }
};
```

#### Go

```go
func hasMatch(s string, p string) bool {
    i := 0
    for _, t := range strings.Split(p, "*") {
        j := strings.Index(s[i:], t)
        if j == -1 {
            return false
        }
        i += j + len(t)
    }
    return true
}
```

#### TypeScript

```ts
function hasMatch(s: string, p: string): boolean {
    let i = 0;
    for (const t of p.split('*')) {
        const j = s.indexOf(t, i);
        if (j === -1) {
            return false;
        }
        i = j + t.length;
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn has_match(s: String, p: String) -> bool {
        let mut i = 0usize;
        for t in p.split('*') {
            if let Some(j) = s[i..].find(t) {
                i += j + t.len();
            } else {
                return false;
            }
        }
        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
