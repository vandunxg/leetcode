---
comments: true
difficulty: Easy
rating: 1193
source: Weekly Contest 199 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [1528. Shuffle String](https://leetcode.com/problems/shuffle-string)

[中文文档](/solution/1500-1599/1528.Shuffle%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và mảng số nguyên <code>indices</code> có <strong>cùng độ dài</strong>. Chuỗi <code>s</code> được xáo trộn sao cho ký tự ở vị trí <code>i<sup>th</sup></code> được chuyển đến <code>indices[i]</code> trong chuỗi sau khi xáo trộn.</p>

<p>Trả về <em>chuỗi sau khi xáo trộn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1528.Shuffle%20String/images/q1.jpg" style="width: 321px; height: 243px;" />
<pre>
<strong>Input:</strong> s = &quot;codeleet&quot;, <code>indices</code> = [4,5,6,7,0,2,1,3]
<strong>Output:</strong> &quot;leetcode&quot;
<strong>Giải thích:</strong> Như minh họa, &quot;codeleet&quot; trở thành &quot;leetcode&quot; sau khi xáo trộn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abc&quot;, <code>indices</code> = [0,1,2]
<strong>Output:</strong> &quot;abc&quot;
<strong>Giải thích:</strong> Sau khi xáo trộn, mỗi ký tự vẫn ở vị trí ban đầu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>s.length == indices.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>0 &lt;= indices[i] &lt; n</code></li>
	<li>Mọi giá trị của <code>indices</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ký tự $s[i]$ thuộc về chỉ số $indices[i]$. Các phép đổi chỗ tại chỗ phải lần theo các chu kỳ hoán vị, phức tạp không cần thiết; $n$ đủ nhỏ để dùng thêm một mảng.
>
> Tạo kết quả có cùng độ dài, ghi mỗi ký tự một lần vào $indices[i]$, rồi nối lại. Mỗi vị trí được gán đúng một lần, không phụ thuộc thứ tự duyệt.

<!-- thinking:end -->

Ta tạo mảng ký tự hoặc chuỗi $\textit{ans}$ có cùng độ dài với chuỗi đầu vào, rồi duyệt chuỗi $\textit{s}$ và đặt mỗi ký tự $\textit{s}[i]$ vào vị trí $\textit{indices}[i]$ trong $\textit{ans}$. Cuối cùng, ta nối mảng ký tự hoặc chuỗi $\textit{ans}$ để tạo kết quả và trả về.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def restoreString(self, s: str, indices: List[int]) -> str:
        ans = [None] * len(s)
        for c, j in zip(s, indices):
            ans[j] = c
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String restoreString(String s, int[] indices) {
        int n = s.length();
        char[] ans = new char[n];
        for (int i = 0; i < n; ++i) {
            ans[indices[i]] = s.charAt(i);
        }
        return new String(ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string restoreString(string s, vector<int>& indices) {
        int n = s.size();
        string ans(n, 0);
        for (int i = 0; i < n; ++i) {
            ans[indices[i]] = s[i];
        }
        return ans;
    }
};
```

#### Go

```go
func restoreString(s string, indices []int) string {
	ans := make([]rune, len(s))
	for i, c := range s {
		ans[indices[i]] = c
	}
	return string(ans)
}
```

#### TypeScript

```ts
function restoreString(s: string, indices: number[]): string {
    const ans: string[] = [];
    for (let i = 0; i < s.length; i++) {
        ans[indices[i]] = s[i];
    }
    return ans.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn restore_string(s: String, indices: Vec<i32>) -> String {
        let n = s.len();
        let mut ans = vec![' '; n];
        let chars: Vec<char> = s.chars().collect();
        for i in 0..n {
            ans[indices[i] as usize] = chars[i];
        }
        ans.iter().collect()
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @param {number[]} indices
 * @return {string}
 */
var restoreString = function (s, indices) {
    const ans = [];
    for (let i = 0; i < s.length; i++) {
        ans[indices[i]] = s[i];
    }
    return ans.join('');
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
