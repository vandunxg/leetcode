---
comments: true
difficulty: Easy
rating: 1175
source: Weekly Contest 382 Q1
tags:
    - String
---

<!-- problem:start -->

# [3019. Number of Changing Keys](https://leetcode.com/problems/number-of-changing-keys)

[中文文档](/solution/3000-3099/3019.Number%20of%20Changing%20Keys/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một chuỗi <strong>0-indexed </strong><code>s</code> do người dùng nhập. Việc đổi phím được định nghĩa là sử dụng một phím khác với phím được sử dụng gần nhất. Ví dụ, <code>s = &quot;ab&quot;</code> có một lần đổi phím, trong khi <code>s = &quot;bBBb&quot;</code> không có lần nào.</p>

<p>Hãy trả về <em>số lần người dùng phải đổi phím.</em></p>

<p><strong>Lưu ý: </strong>Các phím bổ trợ như <code>shift</code> hoặc <code>caps lock</code> không được tính khi đổi phím, nghĩa là nếu người dùng nhập chữ <code>&#39;a&#39;</code> rồi nhập chữ <code>&#39;A&#39;</code> thì sẽ không được xem là đổi phím.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aAbBcC&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Từ s[0] = &#39;a&#39; đến s[1] = &#39;A&#39;, không có lần đổi phím nào vì caps lock hoặc shift không được tính.
Từ s[1] = &#39;A&#39; đến s[2] = &#39;b&#39;, có một lần đổi phím.
Từ s[2] = &#39;b&#39; đến s[3] = &#39;B&#39;, không có lần đổi phím nào vì caps lock hoặc shift không được tính.
Từ s[3] = &#39;B&#39; đến s[4] = &#39;c&#39;, có một lần đổi phím.
Từ s[4] = &#39;c&#39; đến s[5] = &#39;C&#39;, không có lần đổi phím nào vì caps lock hoặc shift không được tính.

</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;AaAaAaaA&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có lần đổi phím nào vì chỉ nhấn các chữ cái &#39;a&#39; và &#39;A&#39;, không cần đổi phím.<!-- notionvc: 8849fe75-f31e-41dc-a2e0-b7d33d8427d2 -->
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết hoa và viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lượt

<!-- thinking:start -->

> **Tư duy**
>
> Bỏ qua chữ hoa/chữ thường và $n \le 100$. Có một lần đổi phím khi hai ký tự liền kề khác nhau sau khi chuyển về chữ thường.
>
> Chuyển chuỗi về chữ thường một lần rồi đếm các cặp ký tự liền kề không bằng nhau; không cần mô phỏng bàn phím.

<!-- thinking:end -->

Ta có thể duyệt chuỗi, mỗi lần kiểm tra dạng viết thường của ký tự hiện tại có giống dạng viết thường của ký tự trước đó hay không. Nếu chúng khác nhau, nghĩa là đã đổi phím, vì vậy ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countKeyChanges(self, s: str) -> int:
        return sum(a != b for a, b in pairwise(s.lower()))
```

#### Java

```java
class Solution {
    public int countKeyChanges(String s) {
        int ans = 0;
        for (int i = 1; i < s.length(); ++i) {
            if (Character.toLowerCase(s.charAt(i)) != Character.toLowerCase(s.charAt(i - 1))) {
                ++ans;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countKeyChanges(string s) {
        transform(s.begin(), s.end(), s.begin(), ::tolower);
        int ans = 0;
        for (int i = 1; i < s.size(); ++i) {
            ans += s[i] != s[i - 1];
        }
        return ans;
    }
};
```

#### Go

```go
func countKeyChanges(s string) (ans int) {
	s = strings.ToLower(s)
	for i, c := range s[1:] {
		if byte(c) != s[i] {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countKeyChanges(s: string): number {
    s = s.toLowerCase();
    let ans = 0;
    for (let i = 1; i < s.length; ++i) {
        if (s[i] !== s[i - 1]) {
            ++ans;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_key_changes(s: String) -> i32 {
        let s = s.to_lowercase();
        let bytes = s.as_bytes();
        let mut ans = 0;
        for i in 1..bytes.len() {
            if bytes[i] != bytes[i - 1] {
                ans += 1;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
