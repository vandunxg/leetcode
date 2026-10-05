---
comments: true
difficulty: Easy
rating: 1250
source: Biweekly Contest 175 Q1
tags:
    - Two Pointers
    - String
    - Simulation
---

<!-- problem:start -->

# [3823. Reverse Letters Then Special Characters in a String](https://leetcode.com/problems/reverse-letters-then-special-characters-in-a-string)

[中文文档](/solution/3800-3899/3823.Reverse%20Letters%20Then%20Special%20Characters%20in%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường và các ký tự đặc biệt.</p>

<p>Nhiệm vụ của bạn là thực hiện các bước <strong>theo thứ tự này</strong>:</p>

<ul>
	<li><strong>Đảo ngược</strong> các <strong>chữ cái viết thường</strong> và đặt chúng trở lại các vị trí ban đầu chứa chữ cái.</li>
	<li><strong>Đảo ngược</strong> các <strong>ký tự đặc biệt</strong> và đặt chúng trở lại các vị trí ban đầu chứa ký tự đặc biệt.</li>
</ul>

<p>Trả về chuỗi thu được sau khi thực hiện các phép đảo ngược.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;</span>)ebc#da@f(<span class="example-io">&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;</span>(fad@cb#e)<span class="example-io">&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các chữ cái trong chuỗi là <code>[&#39;e&#39;, &#39;b&#39;, &#39;c&#39;, &#39;d&#39;, &#39;a&#39;, &#39;f&#39;]</code>:

    <ul>
    	<li>Đảo ngược chúng ta được <code>[&#39;f&#39;, &#39;a&#39;, &#39;d&#39;, &#39;c&#39;, &#39;b&#39;, &#39;e&#39;]</code></li>
    	<li><code>s</code> trở thành <code>&quot;)fad#cb@e(&quot;</code></li>
    </ul>
    </li>
    <li>​​​​​​​Các ký tự đặc biệt trong chuỗi là <code>[&#39;)&#39;, &#39;#&#39;, &#39;@&#39;, &#39;(&#39;]</code>:
    <ul>
    	<li>Đảo ngược chúng ta được <code>[&#39;(&#39;, &#39;@&#39;, &#39;#&#39;, &#39;)&#39;]</code></li>
    	<li><code>s</code> trở thành <code><span class="example-io">&quot;</span>(fad@cb#e)<span class="example-io">&quot;</span></code></li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;z&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;z&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi chỉ chứa một chữ cái, và việc đảo ngược nó không làm thay đổi chuỗi. Chuỗi không có ký tự đặc biệt.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;!@#$%^&amp;*()&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;</span>)(*&amp;^%$#@!<span class="example-io">&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi không chứa chữ cái nào. Chuỗi chứa toàn bộ là ký tự đặc biệt, nên việc đảo ngược các ký tự đặc biệt cũng chính là đảo ngược toàn bộ chuỗi.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và các ký tự đặc biệt trong <code>&quot;!@#$%^&amp;*()&quot;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ cái và ký tự đặc biệt được đảo ngược trong chính các vị trí của loại đó, không chiếm vị trí của nhau. Vì $|s| \le 100$, ta có thể trích xuất rồi ghi chúng trở lại.
>
> Đảo ngược toàn bộ chuỗi sẽ làm xáo trộn ranh giới giữa hai loại ký tự. Ta trích xuất hai loại, sau đó lấy chúng từ phải sang trái.
>
> Đẩy chữ cái và ký tự đặc biệt vào hai stack, rồi duyệt chuỗi ban đầu và pop stack tương ứng.
>
> Cơ chế LIFO tạo ra thứ tự đảo ngược cho từng loại, đồng thời giữ nguyên mẫu vị trí của chúng.

<!-- thinking:end -->

Trước tiên, chúng ta lưu các chữ cái và ký tự đặc biệt trong chuỗi $s$ vào hai danh sách riêng biệt lần lượt là $a$ và $b$. Sau đó, chúng ta duyệt chuỗi $s$. Nếu vị trí hiện tại là một chữ cái, chúng ta lấy chữ cái cuối cùng từ danh sách $a$ và đặt nó trở lại vị trí đó; nếu không, chúng ta lấy ký tự đặc biệt cuối cùng từ danh sách $b$ và đặt nó trở lại vị trí đó.

Sau khi duyệt xong, chúng ta thu được chuỗi kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reverseByType(self, s: str) -> str:
        a = []
        b = []
        for c in s:
            if c.isalpha():
                a.append(c)
            else:
                b.append(c)
        return ''.join(a.pop() if c.isalpha() else b.pop() for c in s)
```

#### Java

```java
class Solution {
    public String reverseByType(String s) {
        StringBuilder a = new StringBuilder();
        StringBuilder b = new StringBuilder();
        char[] t = s.toCharArray();
        for (char c : t) {
            if (Character.isLetter(c)) {
                a.append(c);
            } else {
                b.append(c);
            }
        }
        int j = a.length(), k = b.length();
        for (int i = 0; i < t.length; ++i) {
            if (Character.isLetter(t[i])) {
                t[i] = a.charAt(--j);
            } else {
                t[i] = b.charAt(--k);
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
    string reverseByType(string s) {
        string a, b;

        for (char c : s) {
            if (isalpha(c)) {
                a.push_back(c);
            } else {
                b.push_back(c);
            }
        }

        int j = a.size(), k = b.size();
        for (int i = 0; i < s.size(); ++i) {
            if (isalpha(s[i])) {
                s[i] = a[--j];
            } else {
                s[i] = b[--k];
            }
        }

        return s;
    }
};
```

#### Go

```go
func reverseByType(s string) string {
	a := make([]byte, 0)
	b := make([]byte, 0)
	t := []byte(s)

	for _, c := range t {
		if (c >= 'A' && c <= 'Z') || (c >= 'a' && c <= 'z') {
			a = append(a, c)
		} else {
			b = append(b, c)
		}
	}

	j, k := len(a), len(b)
	for i := 0; i < len(t); i++ {
		if (t[i] >= 'A' && t[i] <= 'Z') || (t[i] >= 'a' && t[i] <= 'z') {
			j--
			t[i] = a[j]
		} else {
			k--
			t[i] = b[k]
		}
	}

	return string(t)
}
```

#### TypeScript

```ts
function reverseByType(s: string): string {
    const a: string[] = [];
    const b: string[] = [];
    const t = s.split('');

    for (const c of t) {
        if (/[a-zA-Z]/.test(c)) {
            a.push(c);
        } else {
            b.push(c);
        }
    }

    let j = a.length,
        k = b.length;
    for (let i = 0; i < t.length; i++) {
        if (/[a-zA-Z]/.test(t[i])) {
            t[i] = a[--j];
        } else {
            t[i] = b[--k];
        }
    }

    return t.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
