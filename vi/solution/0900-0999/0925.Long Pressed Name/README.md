---
comments: true
difficulty: Easy
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [925. Long Pressed Name](https://leetcode.com/problems/long-pressed-name)

[中文文档](/solution/0900-0999/0925.Long%20Pressed%20Name/README.md)

## Mô tả

<!-- description:start -->

<p>Người bạn của bạn đang gõ <code>name</code> trên bàn phím. Đôi khi, khi gõ ký tự <code>c</code>, phím có thể bị <em>nhấn giữ</em> khiến ký tự đó được nhập một hoặc nhiều lần.</p>

<p>Hãy kiểm tra các ký tự được nhập trong <code>typed</code>. Trả về <code>True</code> nếu chuỗi này có thể là tên bạn của bạn, trong đó một số ký tự (có thể không có ký tự nào) bị nhấn giữ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> name = &quot;alex&quot;, typed = &quot;aaleex&quot;
<strong>Output:</strong> true
<strong>Giải thích: </strong>&#39;a&#39; và &#39;e&#39; trong &#39;alex&#39; đã bị nhấn giữ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> name = &quot;saeed&quot;, typed = &quot;ssaaedd&quot;
<strong>Output:</strong> false
<strong>Giải thích: </strong>&#39;e&#39; phải được nhấn hai lần, nhưng trong kết quả typed, ký tự này không xuất hiện hai lần.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= name.length, typed.length &lt;= 1000</code></li>
	<li><code>name</code> và <code>typed</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{typed}$ là kết quả nhấn giữ của $\textit{name}$ nếu mỗi nhóm ký tự giống nhau chỉ có thể dài thêm, không thể ngắn đi hoặc đổi ký tự. Độ dài tối đa là $1000$, nên chỉ cần duyệt các nhóm theo thời gian tuyến tính.
>
> Hai con trỏ cùng duyệt hai chuỗi: ký tự hiện tại phải khớp, sau đó so sánh độ dài từng nhóm; nếu nhóm trong $\textit{name}$ dài hơn thì trả về false. Cuối cùng, cả hai con trỏ phải kết thúc đồng thời.

<!-- thinking:end -->

Ta dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến ký tự đầu tiên của `typed` và `name`, rồi bắt đầu duyệt. Nếu `typed[j]` khác `name[i]`, hai chuỗi không khớp nên trả về `False`. Nếu khớp, ta tìm vị trí tiếp theo sau nhóm ký tự giống nhau liên tiếp, lần lượt ký hiệu là $x$ và $y$. Nếu $x - i > y - j$, nghĩa là nhóm ký tự trong `typed` ngắn hơn nhóm tương ứng trong `name`, nên trả về `False`. Ngược lại, cập nhật $i$ và $j$ thành $x$ và $y$, tiếp tục duyệt cho đến khi đã duyệt hết `name` và `typed`, rồi trả về `True`.

Độ phức tạp thời gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là độ dài của `name` và `typed`. Độ phức tạp không gian là $O(1)`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isLongPressedName(self, name: str, typed: str) -> bool:
        m, n = len(name), len(typed)
        i = j = 0
        while i < m and j < n:
            if name[i] != typed[j]:
                return False
            x = i + 1
            while x < m and name[x] == name[i]:
                x += 1
            y = j + 1
            while y < n and typed[y] == typed[j]:
                y += 1
            if x - i > y - j:
                return False
            i, j = x, y
        return i == m and j == n
```

#### Java

```java
class Solution {
    public boolean isLongPressedName(String name, String typed) {
        int m = name.length(), n = typed.length();
        int i = 0, j = 0;
        while (i < m && j < n) {
            if (name.charAt(i) != typed.charAt(j)) {
                return false;
            }
            int x = i + 1;
            while (x < m && name.charAt(x) == name.charAt(i)) {
                ++x;
            }
            int y = j + 1;
            while (y < n && typed.charAt(y) == typed.charAt(j)) {
                ++y;
            }
            if (x - i > y - j) {
                return false;
            }
            i = x;
            j = y;
        }
        return i == m && j == n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isLongPressedName(string name, string typed) {
        int m = name.length(), n = typed.length();
        int i = 0, j = 0;
        while (i < m && j < n) {
            if (name[i] != typed[j]) {
                return false;
            }
            int x = i + 1;
            while (x < m && name[x] == name[i]) {
                ++x;
            }
            int y = j + 1;
            while (y < n && typed[y] == typed[j]) {
                ++y;
            }
            if (x - i > y - j) {
                return false;
            }
            i = x;
            j = y;
        }
        return i == m && j == n;
    }
};
```

#### Go

```go
func isLongPressedName(name string, typed string) bool {
	m, n := len(name), len(typed)
	i, j := 0, 0

	for i < m && j < n {
		if name[i] != typed[j] {
			return false
		}
		x, y := i+1, j+1

		for x < m && name[x] == name[i] {
			x++
		}

		for y < n && typed[y] == typed[j] {
			y++
		}

		if x-i > y-j {
			return false
		}

		i, j = x, y
	}

	return i == m && j == n
}
```

#### TypeScript

```ts
function isLongPressedName(name: string, typed: string): boolean {
    const [m, n] = [name.length, typed.length];
    let i = 0;
    let j = 0;
    while (i < m && j < n) {
        if (name[i] !== typed[j]) {
            return false;
        }
        let x = i + 1;
        while (x < m && name[x] === name[i]) {
            x++;
        }
        let y = j + 1;
        while (y < n && typed[y] === typed[j]) {
            y++;
        }
        if (x - i > y - j) {
            return false;
        }
        i = x;
        j = y;
    }
    return i === m && j === n;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_long_pressed_name(name: String, typed: String) -> bool {
        let (m, n) = (name.len(), typed.len());
        let (mut i, mut j) = (0, 0);
        let s: Vec<char> = name.chars().collect();
        let t: Vec<char> = typed.chars().collect();

        while i < m && j < n {
            if s[i] != t[j] {
                return false;
            }
            let mut x = i + 1;
            while x < m && s[x] == s[i] {
                x += 1;
            }
            let mut y = j + 1;
            while y < n && t[y] == t[j] {
                y += 1;
            }
            if x - i > y - j {
                return false;
            }
            i = x;
            j = y;
        }

        i == m && j == n
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
