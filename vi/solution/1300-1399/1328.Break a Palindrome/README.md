---
comments: true
difficulty: Medium
rating: 1473
source: Biweekly Contest 18 Q2
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [1328. Break a Palindrome](https://leetcode.com/problems/break-a-palindrome)

[中文文档](/solution/1300-1399/1328.Break%20a%20Palindrome/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi palindrome <code>palindrome</code> gồm các chữ cái tiếng Anh viết thường. Hãy thay <strong>chính xác một</strong> ký tự bằng một chữ cái tiếng Anh viết thường bất kỳ sao cho chuỗi kết quả <strong>không</strong> còn là palindrome và là chuỗi <strong>nhỏ nhất theo thứ tự từ điển</strong> có thể.</p>

<p>Trả về <em>chuỗi kết quả. Nếu không thể thay một ký tự để chuỗi không còn là palindrome, hãy trả về <strong>chuỗi rỗng</strong>.</em></p>

<p>Chuỗi <code>a</code> nhỏ hơn chuỗi <code>b</code> theo thứ tự từ điển (khi hai chuỗi có cùng độ dài) nếu tại vị trí đầu tiên mà chúng khác nhau, ký tự của <code>a</code> nhỏ hơn ký tự tương ứng của <code>b</code>. Ví dụ, <code>&quot;abcc&quot;</code> nhỏ hơn <code>&quot;abcd&quot;</code> theo thứ tự từ điển vì chúng khác nhau lần đầu ở ký tự thứ tư, và <code>&#39;c&#39;</code> nhỏ hơn <code>&#39;d&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> palindrome = &quot;abccba&quot;
<strong>Đầu ra:</strong> &quot;aaccba&quot;
<strong>Giải thích:</strong> Có nhiều cách để biến &quot;abccba&quot; không còn là palindrome, chẳng hạn &quot;<u>z</u>bccba&quot;, &quot;a<u>a</u>ccba&quot; và &quot;ab<u>a</u>cba&quot;.
Trong số đó, &quot;aaccba&quot; là chuỗi nhỏ nhất theo thứ tự từ điển.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> palindrome = &quot;a&quot;
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> Không có cách nào thay một ký tự để &quot;a&quot; không còn là palindrome, nên trả về chuỗi rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= palindrome.length &lt;= 1000</code></li>
	<li><code>palindrome</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần thay đúng một ký tự để chuỗi không còn là palindrome và nhỏ nhất theo thứ tự từ điển; chuỗi độ dài $1$ thì không thể làm được. Thay ký tự khác `'a'` đầu tiên ở nửa đầu bằng `'a'` sẽ phá tính đối xứng sớm nhất và tạo chuỗi nhỏ nhất. Nếu cả nửa đầu chỉ gồm `'a'`, phải đổi ký tự cuối thành `'b'`, nếu không chuỗi vẫn là palindrome.

<!-- thinking:end -->

Trước tiên, ta kiểm tra xem độ dài chuỗi có bằng $1$ không. Nếu có, trả về chuỗi rỗng.

Nếu không, ta duyệt nửa đầu chuỗi từ trái sang phải, tìm ký tự đầu tiên khác `'a'` và đổi nó thành `'a'`. Nếu không có ký tự như vậy, ta đổi ký tự cuối thành `'b'`.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def breakPalindrome(self, palindrome: str) -> str:
        n = len(palindrome)
        if n == 1:
            return ""
        s = list(palindrome)
        i = 0
        while i < n // 2 and s[i] == "a":
            i += 1
        if i == n // 2:
            s[-1] = "b"
        else:
            s[i] = "a"
        return "".join(s)
```

#### Java

```java
class Solution {
    public String breakPalindrome(String palindrome) {
        int n = palindrome.length();
        if (n == 1) {
            return "";
        }
        char[] s = palindrome.toCharArray();
        int i = 0;
        while (i < n / 2 && s[i] == 'a') {
            ++i;
        }
        if (i == n / 2) {
            s[n - 1] = 'b';
        } else {
            s[i] = 'a';
        }
        return String.valueOf(s);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string breakPalindrome(string palindrome) {
        int n = palindrome.size();
        if (n == 1) {
            return "";
        }
        int i = 0;
        while (i < n / 2 && palindrome[i] == 'a') {
            ++i;
        }
        if (i == n / 2) {
            palindrome[n - 1] = 'b';
        } else {
            palindrome[i] = 'a';
        }
        return palindrome;
    }
};
```

#### Go

```go
func breakPalindrome(palindrome string) string {
	n := len(palindrome)
	if n == 1 {
		return ""
	}
	i := 0
	s := []byte(palindrome)
	for i < n/2 && s[i] == 'a' {
		i++
	}
	if i == n/2 {
		s[n-1] = 'b'
	} else {
		s[i] = 'a'
	}
	return string(s)
}
```

#### TypeScript

```ts
function breakPalindrome(palindrome: string): string {
    const n = palindrome.length;
    if (n === 1) {
        return '';
    }
    const s = palindrome.split('');
    let i = 0;
    while (i < n >> 1 && s[i] === 'a') {
        i++;
    }
    if (i == n >> 1) {
        s[n - 1] = 'b';
    } else {
        s[i] = 'a';
    }
    return s.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn break_palindrome(palindrome: String) -> String {
        let n = palindrome.len();
        if n == 1 {
            return "".to_string();
        }
        let mut s: Vec<char> = palindrome.chars().collect();
        let mut i = 0;

        while i < n / 2 && s[i] == 'a' {
            i += 1;
        }

        if i == n / 2 {
            s[n - 1] = 'b';
        } else {
            s[i] = 'a';
        }

        s.into_iter().collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
