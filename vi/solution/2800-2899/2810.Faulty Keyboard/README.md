---
comments: true
difficulty: Easy
rating: 1192
source: Weekly Contest 357 Q1
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [2810. Faulty Keyboard](https://leetcode.com/problems/faulty-keyboard)

[中文文档](/solution/2800-2899/2810.Faulty%20Keyboard/README.md)

## Mô tả

<!-- description:start -->

<p>Bàn phím laptop của bạn bị lỗi. Mỗi khi bạn gõ ký tự <code>&#39;i&#39;</code>, nó sẽ đảo ngược chuỗi bạn đã nhập. Việc gõ các ký tự khác vẫn hoạt động như bình thường.</p>

<p>Cho một chuỗi <code>s</code> được đánh chỉ số từ <strong>0</strong>, và bạn gõ lần lượt từng ký tự của <code>s</code> bằng bàn phím bị lỗi.</p>

<p>Trả về <em>chuỗi cuối cùng hiển thị trên màn hình laptop của bạn.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;string&quot;
<strong>Đầu ra:</strong> &quot;rtsng&quot;
<strong>Giải thích:</strong>
Sau khi gõ ký tự đầu tiên, nội dung trên màn hình là &quot;s&quot;.
Sau khi gõ ký tự thứ hai, nội dung là &quot;st&quot;.
Sau khi gõ ký tự thứ ba, nội dung là &quot;str&quot;.
Vì ký tự thứ tư là &#39;i&#39;, nội dung bị đảo ngược và trở thành &quot;rts&quot;.
Sau khi gõ ký tự thứ năm, nội dung là &quot;rtsn&quot;.
Sau khi gõ ký tự thứ sáu, nội dung là &quot;rtsng&quot;.
Do đó, ta trả về &quot;rtsng&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;poiinter&quot;
<strong>Đầu ra:</strong> &quot;ponter&quot;
<strong>Giải thích:</strong>
Sau khi gõ ký tự đầu tiên, nội dung trên màn hình là &quot;p&quot;.
Sau khi gõ ký tự thứ hai, nội dung là &quot;po&quot;.
Vì ký tự thứ ba bạn gõ là &#39;i&#39;, nội dung bị đảo ngược và trở thành &quot;op&quot;.
Vì ký tự thứ tư bạn gõ là &#39;i&#39;, nội dung bị đảo ngược và trở thành &quot;po&quot;.
Sau khi gõ ký tự thứ năm, nội dung là &quot;pon&quot;.
Sau khi gõ ký tự thứ sáu, nội dung là &quot;pont&quot;.
Sau khi gõ ký tự thứ bảy, nội dung là &quot;ponte&quot;.
Sau khi gõ ký tự thứ tám, nội dung là &quot;ponter&quot;.
Do đó, ta trả về &quot;ponter&quot;.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>s[0] != &#39;i&#39;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi `i` sẽ đảo ngược nội dung hiện tại trên màn hình. Với $n$ không lớn, chỉ cần dùng một list để thêm phần tử và đảo ngược tại chỗ; không cần dùng deque với ràng buộc này.

<!-- thinking:end -->

Ta mô phỏng trực tiếp quá trình nhập liệu bằng bàn phím, sử dụng một mảng ký tự $t$ để lưu nội dung trên màn hình, ban đầu $t$ rỗng.

Với mỗi ký tự $c$ trong chuỗi $s$, nếu $c$ không phải ký tự $'i'$, ta thêm $c$ vào cuối $t$; ngược lại, ta đảo ngược toàn bộ các ký tự trong $t$.

Đáp án cuối cùng là chuỗi được tạo thành từ các ký tự trong $t$.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def finalString(self, s: str) -> str:
        t = []
        for c in s:
            if c == "i":
                t = t[::-1]
            else:
                t.append(c)
        return "".join(t)
```

#### Java

```java
class Solution {
    public String finalString(String s) {
        StringBuilder t = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (c == 'i') {
                t.reverse();
            } else {
                t.append(c);
            }
        }
        return t.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string finalString(string s) {
        string t;
        for (char c : s) {
            if (c == 'i') {
                reverse(t.begin(), t.end());
            } else {
                t.push_back(c);
            }
        }
        return t;
    }
};
```

#### Go

```go
func finalString(s string) string {
	t := []rune{}
	for _, c := range s {
		if c == 'i' {
			for i, j := 0, len(t)-1; i < j; i, j = i+1, j-1 {
				t[i], t[j] = t[j], t[i]
			}
		} else {
			t = append(t, c)
		}
	}
	return string(t)
}
```

#### TypeScript

```ts
function finalString(s: string): string {
    const t: string[] = [];
    for (const c of s) {
        if (c === 'i') {
            t.reverse();
        } else {
            t.push(c);
        }
    }
    return t.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn final_string(s: String) -> String {
        let mut t = Vec::new();
        for c in s.chars() {
            if c == 'i' {
                t.reverse();
            } else {
                t.push(c);
            }
        }
        t.into_iter().collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
