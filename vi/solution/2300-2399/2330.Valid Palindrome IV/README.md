---
comments: true
difficulty: Medium
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [2330. Valid Palindrome IV 🔒](https://leetcode.com/problems/valid-palindrome-iv)

[中文文档](/solution/2300-2399/2330.Valid%20Palindrome%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> được đánh chỉ số từ <strong>0</strong>, chỉ gồm các chữ cái tiếng Anh viết thường. Trong một thao tác, bạn có thể thay đổi <strong>bất kỳ</strong> ký tự nào của <code>s</code> thành một ký tự <strong>khác</strong> bất kỳ.</p>

<p>Trả về <code>true</code><em> nếu có thể biến </em><code>s</code><em> thành một palindrome sau khi thực hiện <strong>chính xác</strong> một hoặc hai thao tác, ngược lại trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcdba&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Một cách biến s thành palindrome bằng 1 thao tác là:
- Thay s[2] thành &#39;d&#39;. Khi đó, s = &quot;abddba&quot;.
Có thể thực hiện một thao tác để biến s thành palindrome, nên trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aa&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Một cách biến s thành palindrome bằng 2 thao tác là:
- Thay s[0] thành &#39;b&#39;. Khi đó, s = &quot;ba&quot;.
- Thay s[1] thành &#39;b&#39;. Khi đó, s = &quot;bb&quot;.
Có thể thực hiện hai thao tác để biến s thành palindrome, nên trả về true.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcdef&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể biến s thành palindrome bằng một hoặc hai thao tác, nên trả về false.
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

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể thay đổi nhiều nhất hai ký tự để biến chuỗi thành palindrome. $n \le 10^5$, nên không thể thử từng vị trí cần sửa. Mỗi thao tác chỉ sửa được nhiều nhất một cặp đối xứng.
>
> Dùng hai con trỏ để đếm các cặp có $s[i] \ne s[j]$. Hai thao tác có thể sửa nhiều nhất hai cặp không khớp.

<!-- thinking:end -->

Ta có thể dùng hai con trỏ $i$ và $j$, lần lượt trỏ đến đầu và cuối chuỗi, sau đó di chuyển về phía trung tâm, đồng thời đếm số cặp ký tự khác nhau. Nếu số cặp khác nhau lớn hơn $2$, trả về $\textit{false}$; ngược lại, trả về $\textit{true}$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makePalindrome(self, s: str) -> bool:
        i, j = 0, len(s) - 1
        cnt = 0
        while i < j:
            cnt += s[i] != s[j]
            i, j = i + 1, j - 1
        return cnt <= 2
```

#### Java

```java
class Solution {
    public boolean makePalindrome(String s) {
        int cnt = 0;
        int i = 0, j = s.length() - 1;
        for (; i < j; ++i, --j) {
            if (s.charAt(i) != s.charAt(j)) {
                ++cnt;
            }
        }
        return cnt <= 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool makePalindrome(string s) {
        int cnt = 0;
        int i = 0, j = s.size() - 1;
        for (; i < j; ++i, --j) {
            cnt += s[i] != s[j];
        }
        return cnt <= 2;
    }
};
```

#### Go

```go
func makePalindrome(s string) bool {
	cnt := 0
	i, j := 0, len(s)-1
	for ; i < j; i, j = i+1, j-1 {
		if s[i] != s[j] {
			cnt++
		}
	}
	return cnt <= 2
}
```

#### TypeScript

```ts
function makePalindrome(s: string): boolean {
    let cnt = 0;
    let i = 0;
    let j = s.length - 1;
    for (; i < j; ++i, --j) {
        if (s[i] != s[j]) {
            ++cnt;
        }
    }
    return cnt <= 2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
