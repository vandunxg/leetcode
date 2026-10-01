---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [01.09. String Rotation](https://leetcode.cn/problems/string-rotation-lcci)

[中文文档](/lcci/01.09.String%20Rotation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s1</code> và <code>s2</code>, hãy viết code để kiểm tra xem <code>s2</code> có phải là phép xoay của <code>s1</code> hay không (ví dụ, &quot;waterbottle&quot; là phép xoay của &quot;erbottlewat&quot;). Bạn có thể chỉ gọi một lần phương thức kiểm tra xem một từ có phải là chuỗi con của một từ khác hay không?</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong>Đầu vào: </strong>s1 = &quot;waterbottle&quot;, s2 = &quot;erbottlewat&quot;

<strong>Đầu ra: </strong>True

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong>Đầu vào: </strong>s1 = &quot;aa&quot;, &quot;aba&quot;

<strong>Đầu ra: </strong>False

</pre>

<p>&nbsp;</p>

<p><strong>Lưu ý:</strong></p>

<ol>
	<li><code><font face="monospace">0 &lt;= s1.length, s1.length &lt;=&nbsp;</font>100000</code></li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đối sánh chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Một phép xoay đưa một tiền tố ra cuối chuỗi. Thử mọi vị trí cắt sẽ cần $n$ phép so sánh có độ dài $n$, tuy chấp nhận được nhưng dư thừa.
>
> Hai chuỗi có độ dài khác nhau thì không thể là phép xoay của nhau. Khi độ dài bằng nhau, $s_1+s_1$ chứa mọi phép xoay của $s_1$, nên vấn đề trở thành kiểm tra xem $s_2$ có phải là chuỗi con của phép nối đó hay không.
>
> Code trước tiên so sánh độ dài, sau đó dùng `s2 in s1 * 2`, chính là phép kiểm tra này.

<!-- thinking:end -->

Trước hết, nếu độ dài của hai chuỗi $s1$ và $s2$ không bằng nhau thì chắc chắn chúng không phải là chuỗi xoay của nhau.

Tiếp theo, nếu độ dài của hai chuỗi $s1$ và $s2$ bằng nhau, việc nối hai $s1$ sẽ tạo ra chuỗi $s1 + s1$, chắc chắn chứa mọi trường hợp xoay của $s1$. Khi đó, ta chỉ cần kiểm tra xem $s2$ có phải là chuỗi con của $s1 + s1$ hay không.

```bash
# True
s1 = "aba"
s2 = "baa"
s1 + s1 = "abaaba"
            ^^^

# False
s1 = "aba"
s2 = "bab"
s1 + s1 = "abaaba"
```

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi $s1$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isFlipedString(self, s1: str, s2: str) -> bool:
        return len(s1) == len(s2) and s2 in s1 * 2
```

#### Java

```java
class Solution {
    public boolean isFlipedString(String s1, String s2) {
        return s1.length() == s2.length() && (s1 + s1).contains(s2);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isFlipedString(string s1, string s2) {
        return s1.size() == s2.size() && (s1 + s1).find(s2) != string::npos;
    }
};
```

#### Go

```go
func isFlipedString(s1 string, s2 string) bool {
	return len(s1) == len(s2) && strings.Contains(s1+s1, s2)
}
```

#### TypeScript

```ts
function isFlipedString(s1: string, s2: string): boolean {
    return s1.length === s2.length && (s2 + s2).indexOf(s1) !== -1;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_fliped_string(s1: String, s2: String) -> bool {
        s1.len() == s2.len() && (s2.clone() + &s2).contains(&s1)
    }
}
```

#### Swift

```swift
class Solution {
    func isFlippedString(_ s1: String, _ s2: String) -> Bool {
        return (s1.isEmpty && s2.isEmpty) || (s1.count == s2.count && (s1 + s1).contains(s2))
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
