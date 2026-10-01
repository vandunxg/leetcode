---
comments: true
difficulty: Easy
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [344. Reverse String](https://leetcode.com/problems/reverse-string)

[中文文档](/solution/0300-0399/0344.Reverse%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Viết hàm đảo ngược chuỗi. Chuỗi đầu vào được cho dưới dạng mảng ký tự <code>s</code>.</p>

<p>Bạn phải thực hiện việc này bằng cách sửa trực tiếp mảng đầu vào <a href="https://en.wikipedia.org/wiki/In-place_algorithm" target="_blank">in-place</a> và chỉ dùng thêm bộ nhớ <code>O(1)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> s = ["h","e","l","l","o"]
<strong>Đầu ra:</strong> ["o","l","l","e","h"]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> s = ["H","a","n","n","a","h"]
<strong>Đầu ra:</strong> ["h","a","n","n","a","H"]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <a href="https://en.wikipedia.org/wiki/ASCII#Printable_characters" target="_blank">ký tự ASCII có thể in</a>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Đảo ngược mảng ký tự ngay tại chỗ. Dùng một buffer thứ hai sẽ tốn $O(n)$ bộ nhớ; chỉ cần hoán đổi hai đầu là đủ.
>
> Hai pointer $i,j$ bắt đầu ở hai đầu mảng, hoán đổi phần tử rồi tiến dần vào giữa cho đến khi gặp nhau. Chỉ cần một lượt duyệt và bộ nhớ phụ hằng số.

<!-- thinking:end -->

Ta dùng hai pointer $i$ và $j$, ban đầu lần lượt trỏ đến đầu và cuối mảng. Mỗi lượt, ta hoán đổi hai phần tử tại $i$ và $j$, sau đó tăng $i$ và giảm $j$ cho đến khi chúng gặp nhau.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        i, j = 0, len(s) - 1
        while i < j:
            s[i], s[j] = s[j], s[i]
            i, j = i + 1, j - 1
```

#### Java

```java
class Solution {
    public void reverseString(char[] s) {
        for (int i = 0, j = s.length - 1; i < j; ++i, --j) {
            char t = s[i];
            s[i] = s[j];
            s[j] = t;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        for (int i = 0, j = s.size() - 1; i < j;) {
            swap(s[i++], s[j--]);
        }
    }
};
```

#### Go

```go
func reverseString(s []byte) {
	for i, j := 0, len(s)-1; i < j; i, j = i+1, j-1 {
		s[i], s[j] = s[j], s[i]
	}
}
```

#### TypeScript

```ts
/**
 Do not return anything, modify s in-place instead.
 */
function reverseString(s: string[]): void {
    for (let i = 0, j = s.length - 1; i < j; ++i, --j) {
        [s[i], s[j]] = [s[j], s[i]];
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn reverse_string(s: &mut Vec<char>) {
        let mut i = 0;
        let mut j = s.len() - 1;
        while i < j {
            s.swap(i, j);
            i += 1;
            j -= 1;
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {character[]} s
 * @return {void} Do not return anything, modify s in-place instead.
 */
var reverseString = function (s) {
    for (let i = 0, j = s.length - 1; i < j; ++i, --j) {
        [s[i], s[j]] = [s[j], s[i]];
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
