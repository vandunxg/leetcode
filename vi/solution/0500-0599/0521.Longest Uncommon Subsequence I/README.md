---
comments: true
difficulty: Easy
tags:
    - String
---

<!-- problem:start -->

# [521. Longest Uncommon Subsequence I](https://leetcode.com/problems/longest-uncommon-subsequence-i)

[中文文档](/solution/0500-0599/0521.Longest%20Uncommon%20Subsequence%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>a</code> và <code>b</code>, hãy trả về <em>độ dài của <strong>subsequence không chung dài nhất</strong> giữa </em><code>a</code> <em>và</em> <code>b</code>. <em>Nếu không tồn tại subsequence không chung như vậy, hãy trả về</em> <code>-1</code><em>.</em></p>

<p><strong>Subsequence không chung</strong> của hai chuỗi là một chuỗi <strong>là <span data-keyword="subsequence-string">subsequence</span> của đúng một trong hai chuỗi đó</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = &quot;aba&quot;, b = &quot;cdc&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Một subsequence không chung dài nhất là &quot;aba&quot; vì &quot;aba&quot; là subsequence của &quot;aba&quot; nhưng không phải subsequence của &quot;cdc&quot;.
Lưu ý, &quot;cdc&quot; cũng là một subsequence không chung dài nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = &quot;aaa&quot;, b = &quot;bbb&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>&nbsp;Các subsequence không chung dài nhất là &quot;aaa&quot; và &quot;bbb&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = &quot;aaa&quot;, b = &quot;aaa&quot;
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>&nbsp;Mọi subsequence của chuỗi a cũng là subsequence của chuỗi b. Tương tự, mọi subsequence của chuỗi b cũng là subsequence của chuỗi a. Vì vậy, đáp án là <code>-1</code>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= a.length, b.length &lt;= 100</code></li>
	<li><code>a</code> và <code>b</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhận xét nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Subsequence được tạo bằng cách xóa ký tự. Nếu hai chuỗi bằng nhau, mỗi chuỗi đều là subsequence của chuỗi còn lại, nên không tồn tại subsequence không chung.
>
> Nếu hai chuỗi khác nhau, chuỗi dài hơn không thể là subsequence của chuỗi ngắn hơn, nên độ dài của nó chính là đáp án. Chỉ cần so sánh hai chuỗi và độ dài; không cần liệt kê các subsequence.

<!-- thinking:end -->

Nếu hai chuỗi `a` và `b` bằng nhau thì không có subsequence không chung nào, hãy trả về `-1`; nếu không, trả về độ dài của chuỗi dài hơn.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi dài hơn trong `a` và `b`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLUSlength(self, a: str, b: str) -> int:
        return -1 if a == b else max(len(a), len(b))
```

#### Java

```java
class Solution {
    public int findLUSlength(String a, String b) {
        return a.equals(b) ? -1 : Math.max(a.length(), b.length());
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findLUSlength(string a, string b) {
        return a == b ? -1 : max(a.size(), b.size());
    }
};
```

#### Go

```go
func findLUSlength(a string, b string) int {
	if a == b {
		return -1
	}
	if len(a) > len(b) {
		return len(a)
	}
	return len(b)
}
```

#### TypeScript

```ts
function findLUSlength(a: string, b: string): number {
    return a === b ? -1 : Math.max(a.length, b.length);
}
```

#### Rust

```rust
impl Solution {
    pub fn find_lu_slength(a: String, b: String) -> i32 {
        if a == b {
            return -1;
        }
        a.len().max(b.len()) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
