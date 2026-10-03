---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - Math
    - String
    - Counting
    - Prefix Sum
---

<!-- problem:start -->

# [2083. Substrings That Begin and End With the Same Letter 🔒](https://leetcode.com/problems/substrings-that-begin-and-end-with-the-same-letter)

[中文文档](/solution/2000-2099/2083.Substrings%20That%20Begin%20and%20End%20With%20the%20Same%20Letter/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <strong>được đánh chỉ số từ 0</strong> <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường. Hãy trả về <em>số lượng <strong>chuỗi con</strong> trong </em><code>s</code> <em>bắt đầu và kết thúc bằng <strong>cùng một</strong> ký tự.</em></p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp không rỗng trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcba&quot;
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong>
Các chuỗi con có độ dài 1 bắt đầu và kết thúc bằng cùng một chữ cái là: &quot;a&quot;, &quot;b&quot;, &quot;c&quot;, &quot;b&quot; và &quot;a&quot;.
Chuỗi con có độ dài 3 bắt đầu và kết thúc bằng cùng một chữ cái là: &quot;bcb&quot;.
Chuỗi con có độ dài 5 bắt đầu và kết thúc bằng cùng một chữ cái là: &quot;abcba&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abacad&quot;
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong>
Các chuỗi con có độ dài 1 bắt đầu và kết thúc bằng cùng một chữ cái là: &quot;a&quot;, &quot;b&quot;, &quot;a&quot;, &quot;c&quot;, &quot;a&quot; và &quot;d&quot;.
Các chuỗi con có độ dài 3 bắt đầu và kết thúc bằng cùng một chữ cái là: &quot;aba&quot; và &quot;aca&quot;.
Chuỗi con có độ dài 5 bắt đầu và kết thúc bằng cùng một chữ cái là: &quot;abaca&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;a&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Chuỗi con có độ dài 1 bắt đầu và kết thúc bằng cùng một chữ cái là: &quot;a&quot;.
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

### Lời giải 1: Mảng hoặc Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Các chuỗi con có hai đầu cùng là một chữ cái. Với $n \le 10^5$, việc liệt kê các cặp có độ phức tạp bậc hai. Số chuỗi con kết thúc tại một ký tự $c$ bằng số ký tự $c$ đã gặp trước đó, tính cả ký tự hiện tại.
>
> Tăng bộ đếm của $c$ rồi cộng tần số mới vào đáp án.

<!-- thinking:end -->

Ta có thể dùng một hash table hoặc một mảng $\textit{cnt}$ có độ dài $26$ để ghi nhận số lần xuất hiện của mỗi ký tự.

Duyệt qua chuỗi $\textit{s}$. Với mỗi ký tự $\textit{c}$, tăng giá trị của $\textit{cnt}[c]$ lên $1$, sau đó cộng giá trị của $\textit{cnt}[c]$ vào đáp án.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $\textit{s}$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự. Ở đây, chuỗi chỉ gồm các chữ cái tiếng Anh viết thường, nên $|\Sigma|=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSubstrings(self, s: str) -> int:
        cnt = Counter()
        ans = 0
        for c in s:
            cnt[c] += 1
            ans += cnt[c]
        return ans
```

#### Java

```java
class Solution {
    public long numberOfSubstrings(String s) {
        int[] cnt = new int[26];
        long ans = 0;
        for (char c : s.toCharArray()) {
            ans += ++cnt[c - 'a'];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long numberOfSubstrings(string s) {
        int cnt[26]{};
        long long ans = 0;
        for (char& c : s) {
            ans += ++cnt[c - 'a'];
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSubstrings(s string) (ans int64) {
	cnt := [26]int{}
	for _, c := range s {
		c -= 'a'
		cnt[c]++
		ans += int64(cnt[c])
	}
	return ans
}
```

#### TypeScript

```ts
function numberOfSubstrings(s: string): number {
    const cnt: Record<string, number> = {};
    let ans = 0;
    for (const c of s) {
        cnt[c] = (cnt[c] || 0) + 1;
        ans += cnt[c];
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_substrings(s: String) -> i64 {
        let mut cnt = [0; 26];
        let mut ans = 0_i64;
        for c in s.chars() {
            let idx = (c as u8 - b'a') as usize;
            cnt[idx] += 1;
            ans += cnt[idx];
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var numberOfSubstrings = function (s) {
    const cnt = {};
    let ans = 0;
    for (const c of s) {
        cnt[c] = (cnt[c] || 0) + 1;
        ans += cnt[c];
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
