---
comments: true
difficulty: Easy
rating: 1251
source: Biweekly Contest 187 Q1
tags:
    - Two Pointers
    - String
    - Sorting
---

<!-- problem:start -->

# [3992. Rearrange String to Avoid Character Pair](https://leetcode.com/problems/rearrange-string-to-avoid-character-pair)

[中文文档](/solution/3900-3999/3992.Rearrange%20String%20to%20Avoid%20Character%20Pair/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và hai chữ cái tiếng Anh thường phân biệt <code>x</code> và <code>y</code>.</p>

<p>Hãy sắp xếp lại các ký tự của <code>s</code> để tạo thành một chuỗi mới <code>t</code> sao cho:</p>

<ul>
	<li><code>t</code> là một <span data-keyword="permutation-string">hoán vị</span> của <code>s</code>.</li>
	<li>Mọi lần xuất hiện của <code>y</code> đều nằm trước mọi lần xuất hiện của <code>x</code> trong <code>t</code>.</li>
</ul>

<p>Trả về bất kỳ chuỗi hợp lệ nào <code>t</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aabc&quot;, x = &quot;a&quot;, y = &quot;c&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;cbaa&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi <code>&quot;cbaa&quot;</code> là một hoán vị của <code>&quot;aabc&quot;</code>, và mọi lần xuất hiện của <code>&#39;c&#39;</code> đều nằm trước mọi lần xuất hiện của <code>&#39;a&#39;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;dcab&quot;, x = &quot;d&quot;, y = &quot;b&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;cabd&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi <code>&quot;cabd&quot;</code> là một hoán vị của <code>&quot;dcab&quot;</code>, và mọi lần xuất hiện của <code>&#39;b&#39;</code> đều nằm trước mọi lần xuất hiện của <code>&#39;d&#39;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;axe&quot;, x = &quot;o&quot;, y = &quot;x&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;axe&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi <code>&quot;axe&quot;</code> đã hợp lệ. Vì <code>&#39;o&#39;</code> không xuất hiện trong chuỗi, điều kiện yêu cầu tự động được thỏa mãn.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh thường.</li>
	<li><code>x</code> và <code>y</code> là các chữ cái tiếng Anh thường.</li>
	<li><code>x != y</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Mọi $y$ phải đứng trước mọi $x$; các ký tự khác không bị ràng buộc. Chỉ cần đưa tất cả $y$s lên đầu là đủ.
>
> Hai con trỏ đổi chỗ từng $y$ vào vị trí $i$ rồi tăng $i$, nên prefix luôn chỉ gồm các $y$s. $n\le 100$.

<!-- thinking:end -->

Ta cần xây dựng một hoán vị $t$ của $s$ sao cho mọi lần xuất hiện của $y$ đều nằm trước mọi lần xuất hiện của $x$. Các ký tự khác không có thêm ràng buộc nào.

Vì vậy, chỉ cần đưa tất cả các lần xuất hiện của $y$ lên đầu chuỗi. Duyệt chuỗi bằng hai con trỏ: $i$ trỏ đến vị trí tiếp theo cần đặt một $y$, còn $j$ quét từ trái sang phải. Mỗi khi $t[j] = y$, đổi chỗ $t[i]$ với $t[j]$ rồi tăng $i$. Sau khi quét xong, prefix của $t$ chỉ gồm các $y$, nên hiển nhiên thỏa mãn yêu cầu mọi $y$ đều đứng trước mọi $x$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rearrangeString(self, s: str, x: str, y: str) -> str:
        t = list(s)
        i = 0
        for j, c in enumerate(t):
            if c == y:
                t[i], t[j] = c, t[i]
                i += 1
        return ''.join(t)
```

#### Java

```java
class Solution {
    public String rearrangeString(String s, char x, char y) {
        char[] t = s.toCharArray();
        int i = 0;
        for (int j = 0; j < t.length; j++) {
            if (t[j] == y) {
                char tmp = t[i];
                t[i] = t[j];
                t[j] = tmp;
                i++;
            }
        }
        return new String(t);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string rearrangeString(string s, char x, char y) {
        int i = 0;
        for (int j = 0; j < s.size(); j++) {
            if (s[j] == y) {
                swap(s[i], s[j]);
                i++;
            }
        }
        return s;
    }
};
```

#### Go

```go
func rearrangeString(s string, x byte, y byte) string {
	t := []byte(s)
	i := 0
	for j, c := range t {
		if c == y {
			t[i], t[j] = t[j], t[i]
			i++
		}
	}
	return string(t)
}
```

#### TypeScript

```ts
function rearrangeString(s: string, x: string, y: string): string {
    const t = s.split('');
    let i = 0;
    for (let j = 0; j < t.length; j++) {
        if (t[j] === y) {
            [t[i], t[j]] = [t[j], t[i]];
            i++;
        }
    }
    return t.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
