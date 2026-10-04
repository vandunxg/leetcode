---
comments: true
difficulty: Easy
rating: 1303
source: Weekly Contest 346 Q2
tags:
    - Greedy
    - Two Pointers
    - String
---

<!-- problem:start -->

# [2697. Lexicographically Smallest Palindrome](https://leetcode.com/problems/lexicographically-smallest-palindrome)

[中文文档](/solution/2600-2699/2697.Lexicographically%20Smallest%20Palindrome/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code node="[object Object]">s</code> gồm các <strong>chữ cái tiếng Anh viết thường</strong>, và được phép thực hiện các thao tác trên chuỗi. Trong một thao tác, bạn có thể <strong>thay thế</strong> một ký tự trong <code node="[object Object]">s</code> bằng một chữ cái tiếng Anh viết thường khác.</p>

<p>Mục tiêu của bạn là biến <code node="[object Object]">s</code> thành một <strong>palindrome</strong> với <strong>số thao tác</strong> <strong>ít nhất</strong> có thể. Nếu có <strong>nhiều palindrome</strong> có thể được <meta charset="utf-8" />tạo ra với <strong>số</strong> thao tác <strong>ít nhất</strong>, <meta charset="utf-8" />hãy tạo palindrome <strong>nhỏ nhất theo thứ tự từ điển</strong>.</p>

<p>Chuỗi <code>a</code> nhỏ hơn chuỗi <code>b</code> theo thứ tự từ điển (với cùng độ dài) nếu tại vị trí đầu tiên mà <code>a</code> và <code>b</code> khác nhau, chuỗi <code>a</code> có một chữ cái đứng trước chữ tương ứng trong <code>b</code> theo thứ tự alphabet.</p>

<p>Trả về <em>chuỗi palindrome thu được.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;egcfe&quot;
<strong>Output:</strong> &quot;efcfe&quot;
<strong>Giải thích:</strong> Số thao tác ít nhất để biến &quot;egcfe&quot; thành palindrome là 1, và chuỗi palindrome nhỏ nhất theo thứ tự từ điển có thể nhận được bằng cách sửa một ký tự là &quot;efcfe&quot;, bằng cách thay đổi &#39;g&#39;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abcd&quot;
<strong>Output:</strong> &quot;abba&quot;
<strong>Giải thích:</strong> Số thao tác ít nhất để biến &quot;abcd&quot; thành palindrome là 2, và chuỗi palindrome nhỏ nhất theo thứ tự từ điển có thể nhận được bằng cách sửa hai ký tự là &quot;abba&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;seven&quot;
<strong>Output:</strong> &quot;neven&quot;
<strong>Giải thích:</strong> Số thao tác ít nhất để biến &quot;seven&quot; thành palindrome là 1, và chuỗi palindrome nhỏ nhất theo thứ tự từ điển có thể nhận được bằng cách sửa một ký tự là &quot;neven&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code>&nbsp;chỉ gồm các chữ cái tiếng Anh viết thường<b>.</b></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ cần giảm các chữ cái, và kết quả phải là một palindrome. Hai đầu đối xứng nên cùng trở thành ký tự nhỏ hơn trong hai ký tự. $n \le 10^5$ cho phép duyệt một lần bằng hai con trỏ; ta không cần tìm xem nên thay đổi những chỉ số nào.

<!-- thinking:end -->

Ta dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến đầu và cuối chuỗi, ban đầu $i = 0$, $j = n - 1$.

Tiếp theo, mỗi lần ta tham lam sửa $s[i]$ và $s[j]$ thành giá trị nhỏ hơn của chúng để chúng bằng nhau. Sau đó ta tăng $i$ lên một bước và giảm $j$ xuống một bước, rồi tiếp tục quá trình này cho đến khi $i \ge j$. Lúc này, ta đã thu được chuỗi palindrome nhỏ nhất.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeSmallestPalindrome(self, s: str) -> str:
        cs = list(s)
        i, j = 0, len(s) - 1
        while i < j:
            cs[i] = cs[j] = min(cs[i], cs[j])
            i, j = i + 1, j - 1
        return "".join(cs)
```

#### Java

```java
class Solution {
    public String makeSmallestPalindrome(String s) {
        char[] cs = s.toCharArray();
        for (int i = 0, j = cs.length - 1; i < j; ++i, --j) {
            cs[i] = cs[j] = (char) Math.min(cs[i], cs[j]);
        }
        return new String(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string makeSmallestPalindrome(string s) {
        for (int i = 0, j = s.size() - 1; i < j; ++i, --j) {
            s[i] = s[j] = min(s[i], s[j]);
        }
        return s;
    }
};
```

#### Go

```go
func makeSmallestPalindrome(s string) string {
	cs := []byte(s)
	for i, j := 0, len(s)-1; i < j; i, j = i+1, j-1 {
		cs[i] = min(cs[i], cs[j])
		cs[j] = cs[i]
	}
	return string(cs)
}
```

#### TypeScript

```ts
function makeSmallestPalindrome(s: string): string {
    const cs = s.split('');
    for (let i = 0, j = s.length - 1; i < j; ++i, --j) {
        cs[i] = cs[j] = s[i] < s[j] ? s[i] : s[j];
    }
    return cs.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn make_smallest_palindrome(s: String) -> String {
        let mut cs: Vec<char> = s.chars().collect();
        let n = cs.len();
        for i in 0..n / 2 {
            let j = n - 1 - i;
            cs[i] = std::cmp::min(cs[i], cs[j]);
            cs[j] = cs[i];
        }
        cs.into_iter().collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
