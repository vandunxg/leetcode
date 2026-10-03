---
comments: true
difficulty: Easy
rating: 1357
source: Biweekly Contest 58 Q1
tags:
    - String
---

<!-- problem:start -->

# [1957. Delete Characters to Make Fancy String](https://leetcode.com/problems/delete-characters-to-make-fancy-string)

[中文文档](/solution/1900-1999/1957.Delete%20Characters%20to%20Make%20Fancy%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>chuỗi fancy</strong> là chuỗi không có <strong>ba</strong> <strong>ký tự liên tiếp</strong> nào giống nhau.</p>

<p>Cho một chuỗi <code>s</code>, hãy xóa số lượng ký tự <strong>ít nhất</strong> có thể khỏi <code>s</code> để biến nó thành một chuỗi <strong>fancy</strong>.</p>

<p>Trả về <em>chuỗi cuối cùng sau khi xóa</em>. Có thể chứng minh rằng đáp án luôn <strong>duy nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;le<u>e</u>etcode&quot;
<strong>Đầu ra:</strong> &quot;leetcode&quot;
<strong>Giải thích:</strong>
Xóa một &#39;e&#39; khỏi nhóm &#39;e&#39; đầu tiên để tạo thành &quot;leetcode&quot;.
Không có ba ký tự liên tiếp nào giống nhau, nên trả về &quot;leetcode&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;<u>a</u>aab<u>aa</u>aa&quot;
<strong>Đầu ra:</strong> &quot;aabaa&quot;
<strong>Giải thích:</strong>
Xóa một &#39;a&#39; khỏi nhóm &#39;a&#39; đầu tiên để tạo thành &quot;aabaaaa&quot;.
Xóa hai &#39;a&#39; khỏi nhóm &#39;a&#39; thứ hai để tạo thành &quot;aabaa&quot;.
Không có ba ký tự liên tiếp nào giống nhau, nên trả về &quot;aabaa&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aab&quot;
<strong>Đầu ra:</strong> &quot;aab&quot;
<strong>Giải thích:</strong> Không có ba ký tự liên tiếp nào giống nhau, nên trả về &quot;aab&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi fancy không có ba chữ cái giống nhau liên tiếp. Để xóa ít nhất có thể, ta giữ lại một ký tự khi việc đó vẫn hợp lệ.
>
> Thêm $s[i]$ vào kết quả trừ khi nó giống với hai ký tự được giữ lại ngay trước đó. Như vậy, ta chỉ xóa những ký tự bắt buộc phải xóa và thu được chuỗi fancy dài nhất.

<!-- thinking:end -->

Ta có thể duyệt qua chuỗi $s$ và dùng một mảng $\textit{ans}$ để lưu đáp án hiện tại. Với mỗi ký tự $\textit{s[i]}$, nếu $i < 2$ hoặc $s[i]$ khác $s[i - 1]$, hoặc $s[i]$ khác $s[i - 2]$, ta thêm $s[i]$ vào $\textit{ans}$.

Cuối cùng, nối các ký tự trong $\textit{ans}$ để nhận được đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Không tính phần bộ nhớ dùng cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeFancyString(self, s: str) -> str:
        ans = []
        for i, c in enumerate(s):
            if i < 2 or c != s[i - 1] or c != s[i - 2]:
                ans.append(c)
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String makeFancyString(String s) {
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (i < 2 || c != s.charAt(i - 1) || c != s.charAt(i - 2)) {
                ans.append(c);
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string makeFancyString(string s) {
        string ans = "";
        for (int i = 0; i < s.length(); ++i) {
            char c = s[i];
            if (i < 2 || c != s[i - 1] || c != s[i - 2]) {
                ans += c;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func makeFancyString(s string) string {
	ans := []byte{}
	for i, ch := range s {
		if c := byte(ch); i < 2 || c != s[i-1] || c != s[i-2] {
			ans = append(ans, c)
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function makeFancyString(s: string): string {
    const ans: string[] = [];
    for (let i = 0; i < s.length; ++i) {
        if (s[i] !== s[i - 1] || s[i] !== s[i - 2]) {
            ans.push(s[i]);
        }
    }
    return ans.join('');
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {string}
 */
var makeFancyString = function (s) {
    const ans = [];
    for (let i = 0; i < s.length; ++i) {
        if (s[i] !== s[i - 1] || s[i] !== s[i - 2]) {
            ans.push(s[i]);
        }
    }
    return ans.join('');
};
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @return String
     */
    function makeFancyString($s) {
        $ans = '';
        for ($i = 0; $i < strlen($s); $i++) {
            $c = $s[$i];
            if ($i < 2 || $c !== $s[$i - 1] || $c !== $s[$i - 2]) {
                $ans .= $c;
            }
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
