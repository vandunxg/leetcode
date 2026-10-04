---
comments: true
difficulty: Easy
rating: 1242
source: Weekly Contest 348 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [2716. Minimize String Length](https://leetcode.com/problems/minimize-string-length)

[中文文档](/solution/2700-2799/2716.Minimize%20String%20Length/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>, ta có hai loại thao tác:</p>

<ol>
	<li>Chọn một chỉ số <code>i</code> trong chuỗi, gọi <code>c</code> là ký tự ở vị trí <code>i</code>. <strong>Xóa</strong> <strong>lần xuất hiện gần nhất</strong> của <code>c</code> ở <strong>bên trái</strong> <code>i</code> (nếu tồn tại).</li>
	<li>Chọn một chỉ số <code>i</code> trong chuỗi, gọi <code>c</code> là ký tự ở vị trí <code>i</code>. <strong>Xóa</strong> <strong>lần xuất hiện gần nhất</strong> của <code>c</code> ở <strong>bên phải</strong> <code>i</code> (nếu tồn tại).</li>
</ol>

<p>Nhiệm vụ của bạn là <strong>tối thiểu hóa</strong> độ dài của <code>s</code> bằng cách thực hiện các thao tác trên không hoặc nhiều lần.</p>

<p>Trả về một số nguyên biểu thị độ dài của chuỗi <strong>sau khi tối thiểu hóa</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aaabc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ol>
	<li>Thao tác 2: chọn <code>i = 1</code>, khi đó <code>c</code> là &#39;a&#39;, rồi xóa <code>s[2]</code> vì đây là ký tự &#39;a&#39; gần <code>s[1]</code> nhất ở bên phải.<br />
	<code>s</code> trở thành &quot;aabc&quot; sau thao tác này.</li>
	<li>Thao tác 1: chọn <code>i = 1</code>, khi đó <code>c</code> là &#39;a&#39;, rồi xóa <code>s[0]</code> vì đây là ký tự &#39;a&#39; gần <code>s[1]</code> nhất ở bên trái.<br />
	<code>s</code> trở thành &quot;abc&quot; sau thao tác này.</li>
</ol>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;cbbd&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ol>
	<li>Thao tác 1: chọn <code>i = 2</code>, khi đó <code>c</code> là &#39;b&#39;, rồi xóa <code>s[1]</code> vì đây là ký tự &#39;b&#39; gần <code>s[1]</code> nhất ở bên trái.<br />
	<code>s</code> trở thành &quot;cbd&quot; sau thao tác này.</li>
</ol>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;baadccab&quot;</span></p>

<p><strong>Đầu ra:</strong> 4</p>

<p><strong>Giải thích:</strong></p>

<ol>
	<li>Thao tác 1: chọn <code>i = 6</code>, khi đó <code>c</code> là &#39;a&#39;, rồi xóa <code>s[2]</code> vì đây là ký tự &#39;a&#39; gần <code>s[6]</code> nhất ở bên trái.<br />
	<code>s</code> trở thành &quot;badccab&quot; sau thao tác này.</li>
	<li>Thao tác 2: chọn <code>i = 0</code>, khi đó <code>c</code> là &#39;b&#39;, rồi xóa <code>s[6]</code> vì đây là ký tự &#39;b&#39; gần <code>s[0]</code> nhất ở bên phải.<br />
	<code>s</code> trở thành &quot;badcca&quot; sau thao tác này.</li>
	<li>Thao tác 2: chọn <code>i = 3</code>, khi đó <code>c</code> là &#39;c&#39;, rồi xóa <code>s[4]</code> vì đây là ký tự &#39;c&#39; gần <code>s[3]</code> nhất ở bên phải.<br />
	<code>s</code> trở thành &quot;badca&quot; sau thao tác này.</li>
	<li>Thao tác 1: chọn <code>i = 4</code>, khi đó <code>c</code> là &#39;a&#39;, rồi xóa <code>s[1]</code> vì đây là ký tự &#39;a&#39; gần <code>s[4]</code> nhất ở bên trái.<br />
	<code>s</code> trở thành &quot;bdca&quot; sau thao tác này.</li>
</ol>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác xóa các lần xuất hiện của một ký tự ở một phía của bản sao được chọn, nên nếu mô phỏng các lần xóa thì ta sẽ phải quét lại chuỗi. Tuy nhiên, một ký tự không bao giờ bị xóa hoàn toàn.
>
> Mỗi ký tự phân biệt còn lại đúng một lần, vì vậy đáp án là số lượng ký tự khác nhau, tức kích thước của một set được tạo từ $s$.

<!-- thinking:end -->

Bài toán thực chất có thể được chuyển thành việc tìm số lượng ký tự khác nhau trong chuỗi. Vì vậy, ta chỉ cần đếm số lượng ký tự phân biệt trong chuỗi.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $\textit{s}$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự. Trong trường hợp này, đó là các chữ cái tiếng Anh viết thường, nên $|\Sigma|=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimizedStringLength(self, s: str) -> int:
        return len(set(s))
```

#### Java

```java
class Solution {
    public int minimizedStringLength(String s) {
        Set<Character> ss = new HashSet<>();
        for (int i = 0; i < s.length(); ++i) {
            ss.add(s.charAt(i));
        }
        return ss.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimizedStringLength(string s) {
        return unordered_set<char>(s.begin(), s.end()).size();
    }
};
```

#### Go

```go
func minimizedStringLength(s string) int {
	ss := map[rune]struct{}{}
	for _, c := range s {
		ss[c] = struct{}{}
	}
	return len(ss)
}
```

#### TypeScript

```ts
function minimizedStringLength(s: string): number {
    return new Set(s.split('')).size;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn minimized_string_length(s: String) -> i32 {
        let ss: HashSet<char> = s.chars().collect();
        ss.len() as i32
    }
}
```

#### C#

```cs
public class Solution {
    public int MinimizedStringLength(string s) {
        return new HashSet<char>(s).Count;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
