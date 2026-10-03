---
comments: true
difficulty: Medium
rating: 1501
source: Biweekly Contest 45 Q3
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [1750. Minimum Length of String After Deleting Similar Ends](https://leetcode.com/problems/minimum-length-of-string-after-deleting-similar-ends)

[中文文档](/solution/1700-1799/1750.Minimum%20Length%20of%20String%20After%20Deleting%20Similar%20Ends/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> chỉ gồm các ký tự <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> và <code>&#39;c&#39;</code>. Bạn cần áp dụng thuật toán sau lên chuỗi một số lần bất kỳ:</p>

<ol>
	<li>Chọn một tiền tố <strong>không rỗng</strong> của chuỗi <code>s</code> sao cho mọi ký tự trong tiền tố đều giống nhau.</li>
	<li>Chọn một hậu tố <strong>không rỗng</strong> của chuỗi <code>s</code> sao cho mọi ký tự trong hậu tố đều giống nhau.</li>
	<li>Tiền tố và hậu tố không được giao nhau tại bất kỳ chỉ số nào.</li>
	<li>Các ký tự trong tiền tố và hậu tố phải giống nhau.</li>
	<li>Xóa cả tiền tố và hậu tố.</li>
</ol>

<p>Trả về <em><strong>độ dài nhỏ nhất</strong> của </em><code>s</code> <em>sau khi thực hiện thao tác trên một số lần bất kỳ (có thể là không lần nào)</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ca&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>Không thể xóa ký tự nào nên chuỗi được giữ nguyên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;cabaabac&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Một dãy thao tác tối ưu là:
- Chọn tiền tố = &quot;c&quot; và hậu tố = &quot;c&quot; rồi xóa chúng, s = &quot;abaaba&quot;.
- Chọn tiền tố = &quot;a&quot; và hậu tố = &quot;a&quot; rồi xóa chúng, s = &quot;baab&quot;.
- Chọn tiền tố = &quot;b&quot; và hậu tố = &quot;b&quot; rồi xóa chúng, s = &quot;aa&quot;.
- Chọn tiền tố = &quot;a&quot; và hậu tố = &quot;a&quot; rồi xóa chúng, s = &quot;&quot;.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabccabba&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Một dãy thao tác tối ưu là:
- Chọn tiền tố = &quot;aa&quot; và hậu tố = &quot;a&quot; rồi xóa chúng, s = &quot;bccabb&quot;.
- Chọn tiền tố = &quot;b&quot; và hậu tố = &quot;bb&quot; rồi xóa chúng, s = &quot;cca&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các ký tự <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> và <code>&#39;c&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two pointers

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác xóa một tiền tố và hậu tố không rỗng có cùng ký tự. Sau đó, hai đầu mới có thể lại giống nhau. Hai con trỏ mô phỏng từng lần xóa.
>
> Khi hai đầu có cùng ký tự và chưa vượt qua nhau, bỏ qua toàn bộ đoạn liên tiếp ở mỗi bên rồi tiến vào trong. Phần còn lại có độ dài $\max(0,j-i+1)$.

<!-- thinking:end -->

Ta định nghĩa hai con trỏ $i$ và $j$ lần lượt trỏ đến đầu và cuối chuỗi $s$, sau đó di chuyển chúng vào giữa cho đến khi các ký tự tại $i$ và $j$ khác nhau. Khi đó, đáp án là $\max(0, j - i + 1)$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumLength(self, s: str) -> int:
        i, j = 0, len(s) - 1
        while i < j and s[i] == s[j]:
            while i + 1 < j and s[i] == s[i + 1]:
                i += 1
            while i < j - 1 and s[j - 1] == s[j]:
                j -= 1
            i, j = i + 1, j - 1
        return max(0, j - i + 1)
```

#### Java

```java
class Solution {
    public int minimumLength(String s) {
        int i = 0, j = s.length() - 1;
        while (i < j && s.charAt(i) == s.charAt(j)) {
            while (i + 1 < j && s.charAt(i) == s.charAt(i + 1)) {
                ++i;
            }
            while (i < j - 1 && s.charAt(j) == s.charAt(j - 1)) {
                --j;
            }
            ++i;
            --j;
        }
        return Math.max(0, j - i + 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumLength(string s) {
        int i = 0, j = s.size() - 1;
        while (i < j && s[i] == s[j]) {
            while (i + 1 < j && s[i] == s[i + 1]) {
                ++i;
            }
            while (i < j - 1 && s[j] == s[j - 1]) {
                --j;
            }
            ++i;
            --j;
        }
        return max(0, j - i + 1);
    }
};
```

#### Go

```go
func minimumLength(s string) int {
	i, j := 0, len(s)-1
	for i < j && s[i] == s[j] {
		for i+1 < j && s[i] == s[i+1] {
			i++
		}
		for i < j-1 && s[j] == s[j-1] {
			j--
		}
		i, j = i+1, j-1
	}
	return max(0, j-i+1)
}
```

#### TypeScript

```ts
function minimumLength(s: string): number {
    let i = 0;
    let j = s.length - 1;
    while (i < j && s[i] === s[j]) {
        while (i + 1 < j && s[i + 1] === s[i]) {
            ++i;
        }
        while (i < j - 1 && s[j - 1] === s[j]) {
            --j;
        }
        ++i;
        --j;
    }
    return Math.max(0, j - i + 1);
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_length(s: String) -> i32 {
        let s = s.as_bytes();
        let n = s.len();
        let mut start = 0;
        let mut end = n - 1;
        while start < end && s[start] == s[end] {
            while start + 1 < end && s[start] == s[start + 1] {
                start += 1;
            }
            while start < end - 1 && s[end] == s[end - 1] {
                end -= 1;
            }
            start += 1;
            end -= 1;
        }
        (0).max(end - start + 1) as i32
    }
}
```

#### C

```c
int minimumLength(char* s) {
    int n = strlen(s);
    int start = 0;
    int end = n - 1;
    while (start < end && s[start] == s[end]) {
        while (start + 1 < end && s[start] == s[start + 1]) {
            start++;
        }
        while (start < end - 1 && s[end] == s[end - 1]) {
            end--;
        }
        start++;
        end--;
    }
    if (start > end) {
        return 0;
    }
    return end - start + 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
