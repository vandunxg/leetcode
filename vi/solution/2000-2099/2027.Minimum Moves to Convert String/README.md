---
comments: true
difficulty: Easy
rating: 1346
source: Weekly Contest 261 Q1
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [2027. Minimum Moves to Convert String](https://leetcode.com/problems/minimum-moves-to-convert-string)

[中文文档](/solution/2000-2099/2027.Minimum%20Moves%20to%20Convert%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> gồm <code>n</code> ký tự, mỗi ký tự là <code>&#39;X&#39;</code> hoặc <code>&#39;O&#39;</code>.</p>

<p>Một <strong>thao tác</strong> được định nghĩa là chọn <strong>ba</strong> <strong>ký tự liên tiếp</strong> của <code>s</code> và chuyển chúng thành <code>&#39;O&#39;</code>. Lưu ý rằng nếu áp dụng thao tác lên ký tự <code>&#39;O&#39;</code>, ký tự đó sẽ giữ <strong>nguyên</strong>.</p>

<p>Hãy trả về <em><strong>số thao tác ít nhất</strong> cần thực hiện để tất cả ký tự của </em><code>s</code><em> được chuyển thành </em><code>&#39;O&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;XXX&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> <u>XXX</u> -&gt; OOO
Ta chọn cả 3 ký tự và chuyển chúng trong một thao tác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;XXOX&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> <u>XXO</u>X -&gt; O<u>OOX</u> -&gt; OOOO
Ta chọn 3 ký tự đầu tiên trong thao tác đầu tiên và chuyển chúng thành <code>&#39;O&#39;</code>.
Sau đó, ta chọn 3 ký tự cuối cùng và chuyển chúng để chuỗi cuối cùng chỉ chứa các ký tự <code>&#39;O&#39;</code>.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;OOOO&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Trong <code>s</code> không có ký tự <code>&#39;X&#39;s</code> nào cần chuyển đổi.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s[i]</code> là <code>&#39;X&#39;</code> hoặc <code>&#39;O&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác bao phủ ba ký tự liên tiếp. Với $n \le 1000$, chỉ cần duyệt chuỗi một lần. Để giảm số thao tác, ta luôn bao phủ `X` chưa được xử lý ở bên trái cùng với hai vị trí tiếp theo.
>
> Khi gặp `X`, ta tăng đáp án và bỏ qua ba chỉ số; khi gặp `O`, ta chỉ tăng một chỉ số. Các đoạn được chọn không giao nhau và đây là cách tối ưu.

<!-- thinking:end -->

Duyệt chuỗi $s$. Mỗi khi gặp `'X'`, tăng con trỏ $i$ thêm ba bước và tăng đáp án thêm $1$; nếu không, tăng con trỏ $i$ thêm một bước.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumMoves(self, s: str) -> int:
        ans = i = 0
        while i < len(s):
            if s[i] == "X":
                ans += 1
                i += 3
            else:
                i += 1
        return ans
```

#### Java

```java
class Solution {
    public int minimumMoves(String s) {
        int ans = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == 'X') {
                ++ans;
                i += 2;
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
    int minimumMoves(string s) {
        int ans = 0;
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == 'X') {
                ++ans;
                i += 2;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumMoves(s string) (ans int) {
	for i := 0; i < len(s); i++ {
		if s[i] == 'X' {
			ans++
			i += 2
		}
	}
	return
}
```

#### TypeScript

```ts
function minimumMoves(s: string): number {
    const n = s.length;
    let ans = 0;
    let i = 0;
    while (i < n) {
        if (s[i] === 'X') {
            ans++;
            i += 3;
        } else {
            i++;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_moves(s: String) -> i32 {
        let s = s.as_bytes();
        let n = s.len();
        let mut ans = 0;
        let mut i = 0;
        while i < n {
            if s[i] == b'X' {
                ans += 1;
                i += 3;
            } else {
                i += 1;
            }
        }
        ans
    }
}
```

#### C

```c
int minimumMoves(char* s) {
    int n = strlen(s);
    int ans = 0;
    int i = 0;
    while (i < n) {
        if (s[i] == 'X') {
            ans++;
            i += 3;
        } else {
            i++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
