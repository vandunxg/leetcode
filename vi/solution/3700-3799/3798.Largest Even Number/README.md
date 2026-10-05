---
comments: true
difficulty: Easy
rating: 1365
source: Weekly Contest 483 Q1
tags:
    - String
---

<!-- problem:start -->

# [3798. Largest Even Number](https://leetcode.com/problems/largest-even-number)

[中文文档](/solution/3700-3799/3798.Largest%20Even%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các ký tự <code>&#39;1&#39;</code> và <code>&#39;2&#39;</code>.</p>

<p>Bạn có thể xóa tùy ý số ký tự khỏi <code>s</code> mà không thay đổi thứ tự của các ký tự còn lại.</p>

<p>Hãy trả về <strong>chuỗi kết quả lớn nhất có thể</strong> biểu diễn một số nguyên <strong>chẵn</strong>. Nếu không tồn tại chuỗi như vậy, hãy trả về chuỗi rỗng <code>&quot;&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1112&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;1112&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi đã biểu diễn số chẵn lớn nhất có thể, nên không cần xóa ký tự nào.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;221&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;22&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xóa ký tự <code>&#39;1&#39;</code> sẽ cho số chẵn lớn nhất có thể, là 22.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có cách nào tạo ra một số chẵn.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các ký tự <code>&#39;1&#39;</code> và <code>&#39;2&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi chỉ chứa `1` và `2`, nên số nguyên chẵn phải kết thúc bằng `2`. Giữ lại mọi ký tự sẽ tối đa hóa giá trị, vì vậy ta xóa các ký tự `1` ở cuối cho đến khi còn lại một `2`; nếu không có 2, kết quả là chuỗi rỗng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestEven(self, s: str) -> str:
        return s.rstrip("1")
```

#### Java

```java
class Solution {
    public String largestEven(String s) {
        int i = s.length();
        while (i > 0 && s.charAt(i - 1) == '1') {
            i--;
        }
        return s.substring(0, i);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string largestEven(string s) {
        while (!s.empty() && s.back() == '1') {
            s.pop_back();
        }
        return s;
    }
};
```

#### Go

```go
func largestEven(s string) string {
	return strings.TrimRight(s, "1")
}
```

#### TypeScript

```ts
function largestEven(s: string): string {
    return s.replace(/1+$/, '');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
