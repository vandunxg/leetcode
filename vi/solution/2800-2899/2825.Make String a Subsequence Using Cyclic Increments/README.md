---
comments: true
difficulty: Medium
rating: 1414
source: Biweekly Contest 111 Q2
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [2825. Make String a Subsequence Using Cyclic Increments](https://leetcode.com/problems/make-string-a-subsequence-using-cyclic-increments)

[中文文档](/solution/2800-2899/2825.Make%20String%20a%20Subsequence%20Using%20Cyclic%20Increments/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>str1</code> và <code>str2</code> được đánh chỉ số từ <strong>0</strong>.</p>

<p>Trong một thao tác, bạn chọn một <strong>tập</strong> các chỉ số trong <code>str1</code>, rồi với mỗi chỉ số <code>i</code> trong tập đó, tăng <code>str1[i]</code> lên ký tự tiếp theo theo <strong>chu kỳ</strong>. Cụ thể, <code>&#39;a&#39;</code> trở thành <code>&#39;b&#39;</code>, <code>&#39;b&#39;</code> trở thành <code>&#39;c&#39;</code>, và cứ tiếp tục như vậy; <code>&#39;z&#39;</code> trở thành <code>&#39;a&#39;</code>.</p>

<p>Trả về <code>true</code> <em>nếu có thể biến </em><code>str2</code> <em>thành một subsequence của </em><code>str1</code> <em>bằng cách thực hiện thao tác <strong>nhiều nhất một lần</strong></em>, <em>và </em><code>false</code> <em>trong trường hợp ngược lại.</em></p>

<p><strong>Lưu ý:</strong> Subsequence của một chuỗi là một chuỗi mới được tạo từ chuỗi ban đầu bằng cách xóa một số ký tự (có thể không xóa ký tự nào) mà không làm thay đổi thứ tự tương đối của các ký tự còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> str1 = &quot;abc&quot;, str2 = &quot;ad&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Chọn chỉ số 2 trong str1.
Tăng str1[2] để trở thành &#39;d&#39;.
Do đó, str1 trở thành &quot;abd&quot; và str2 hiện là một subsequence. Vì vậy, trả về true.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> str1 = &quot;zc&quot;, str2 = &quot;ad&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Chọn các chỉ số 0 và 1 trong str1.
Tăng str1[0] để trở thành &#39;a&#39;.
Tăng str1[1] để trở thành &#39;d&#39;.
Do đó, str1 trở thành &quot;ad&quot; và str2 hiện là một subsequence. Vì vậy, trả về true.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> str1 = &quot;ab&quot;, str2 = &quot;d&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Trong ví dụ này, có thể chứng minh rằng không thể biến str2 thành một subsequence của str1 bằng cách sử dụng thao tác nhiều nhất một lần.
Do đó, trả về false.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= str1.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= str2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>str1</code> và <code>str2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> $str2$ phải là một subsequence của $str1$, trong đó mỗi ký tự được khớp có thể được tăng theo chu kỳ nhiều nhất một lần. Duyệt qua $str1$ và tăng vị trí trong $str2$ khi ký tự hiện tại hoặc ký tự kế tiếp của nó khớp với chữ cái tiếp theo cần tìm.

<!-- thinking:end -->

Bài toán này thực chất yêu cầu xác định xem một chuỗi $s$ có phải là subsequence của một chuỗi khác $t$ hay không. Tuy nhiên, các ký tự không nhất thiết phải khớp chính xác. Nếu hai ký tự giống nhau, hoặc một ký tự là ký tự tiếp theo của ký tự kia, thì chúng có thể khớp.

Độ phức tạp thời gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là độ dài của các chuỗi $str1$ và $str2$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canMakeSubsequence(self, str1: str, str2: str) -> bool:
        i = 0
        for c in str1:
            d = "a" if c == "z" else chr(ord(c) + 1)
            if i < len(str2) and str2[i] in (c, d):
                i += 1
        return i == len(str2)
```

#### Java

```java
class Solution {
    public boolean canMakeSubsequence(String str1, String str2) {
        int i = 0, n = str2.length();
        for (char c : str1.toCharArray()) {
            char d = c == 'z' ? 'a' : (char) (c + 1);
            if (i < n && (str2.charAt(i) == c || str2.charAt(i) == d)) {
                ++i;
            }
        }
        return i == n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canMakeSubsequence(string str1, string str2) {
        int i = 0, n = str2.size();
        for (char c : str1) {
            char d = c == 'z' ? 'a' : c + 1;
            if (i < n && (str2[i] == c || str2[i] == d)) {
                ++i;
            }
        }
        return i == n;
    }
};
```

#### Go

```go
func canMakeSubsequence(str1 string, str2 string) bool {
	i, n := 0, len(str2)
	for _, c := range str1 {
		d := byte('a')
		if c != 'z' {
			d = byte(c + 1)
		}
		if i < n && (str2[i] == byte(c) || str2[i] == d) {
			i++
		}
	}
	return i == n
}
```

#### TypeScript

```ts
function canMakeSubsequence(str1: string, str2: string): boolean {
    let i = 0;
    const n = str2.length;
    for (const c of str1) {
        const d = c === 'z' ? 'a' : String.fromCharCode(c.charCodeAt(0) + 1);
        if (i < n && (str2[i] === c || str2[i] === d)) {
            ++i;
        }
    }
    return i === n;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
