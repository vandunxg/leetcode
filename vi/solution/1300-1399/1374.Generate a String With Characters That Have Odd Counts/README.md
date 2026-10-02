---
comments: true
difficulty: Easy
rating: 1164
source: Weekly Contest 179 Q1
tags:
    - String
---

<!-- problem:start -->

# [1374. Generate a String With Characters That Have Odd Counts](https://leetcode.com/problems/generate-a-string-with-characters-that-have-odd-counts)

[中文文档](/solution/1300-1399/1374.Generate%20a%20String%20With%20Characters%20That%20Have%20Odd%20Counts/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy <em>trả về một chuỗi gồm <code>n</code> ký tự sao cho mỗi ký tự trong chuỗi xuất hiện <strong>số lần lẻ</strong></em>.</p>

<p>Chuỗi trả về chỉ được chứa chữ cái tiếng Anh viết thường. Nếu có nhiều chuỗi hợp lệ, có thể trả về <strong>bất kỳ</strong> chuỗi nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> &quot;pppz&quot;
<strong>Giải thích:</strong> &quot;pppz&quot; là chuỗi hợp lệ vì ký tự &#39;p&#39; xuất hiện ba lần và ký tự &#39;z&#39; xuất hiện một lần. Ngoài ra còn nhiều chuỗi hợp lệ khác, chẳng hạn &quot;ohhh&quot; và &quot;love&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> &quot;xy&quot;
<strong>Giải thích:</strong> &quot;xy&quot; là chuỗi hợp lệ vì các ký tự &#39;x&#39; và &#39;y&#39; đều xuất hiện một lần. Ngoài ra còn nhiều chuỗi hợp lệ khác, chẳng hạn &quot;ag&quot; và &quot;ur&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 7
<strong>Đầu ra:</strong> &quot;holasss&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dựng chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Dựng chuỗi độ dài $n$ sao cho mỗi chữ cái được dùng xuất hiện số lần lẻ. Nếu $n$ lẻ, dùng $n$ ký tự `'a'`. Nếu $n$ chẵn, dùng $n-1$ ký tự `'a'` và một ký tự `'b'`, khi đó cả hai số lần xuất hiện đều lẻ.

<!-- thinking:end -->

Nếu $n$ lẻ, ta có thể tạo trực tiếp chuỗi gồm $n$ ký tự `'a'`.

Nếu $n$ chẵn, ta có thể tạo chuỗi gồm $n-1$ ký tự `'a'` và một ký tự `'b'`.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def generateTheString(self, n: int) -> str:
        return 'a' * n if n & 1 else 'a' * (n - 1) + 'b'
```

#### Java

```java
class Solution {
    public String generateTheString(int n) {
        return (n % 2 == 1) ? "a".repeat(n) : "a".repeat(n - 1) + "b";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string generateTheString(int n) {
        string ans(n, 'a');
        if (n % 2 == 0) {
            ans[0] = 'b';
        }
        return ans;
    }
};
```

#### Go

```go
func generateTheString(n int) string {
	ans := strings.Repeat("a", n-1)
	if n%2 == 0 {
		ans += "b"
	} else {
		ans += "a"
	}
	return ans
}
```

#### TypeScript

```ts
function generateTheString(n: number): string {
    const ans = Array(n).fill('a');
    if (n % 2 === 0) {
        ans[0] = 'b';
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
