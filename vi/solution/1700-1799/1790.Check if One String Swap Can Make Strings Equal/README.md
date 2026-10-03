---
comments: true
difficulty: Easy
rating: 1300
source: Weekly Contest 232 Q1
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [1790. Check if One String Swap Can Make Strings Equal](https://leetcode.com/problems/check-if-one-string-swap-can-make-strings-equal)

[中文文档](/solution/1700-1799/1790.Check%20if%20One%20String%20Swap%20Can%20Make%20Strings%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s1</code> và <code>s2</code> có cùng độ dài. <strong>Hoán đổi chuỗi</strong> là thao tác chọn hai chỉ số trong một chuỗi (không nhất thiết khác nhau) rồi hoán đổi các ký tự ở hai chỉ số đó.</p>

<p>Trả về <code>true</code> <em>nếu có thể làm cho hai chuỗi bằng nhau bằng cách thực hiện <strong>không quá một lần hoán đổi</strong> trên <strong>chính xác một</strong> trong hai chuỗi. </em>Ngược lại, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s1 = &quot;bank&quot;, s2 = &quot;kanb&quot;
<strong>Output:</strong> true
<strong>Giải thích:</strong> Chẳng hạn, hoán đổi ký tự đầu tiên với ký tự cuối cùng của s2 để được &quot;bank&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s1 = &quot;attack&quot;, s2 = &quot;defend&quot;
<strong>Output:</strong> false
<strong>Giải thích:</strong> Không thể làm cho chúng bằng nhau chỉ bằng một lần hoán đổi chuỗi.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s1 = &quot;kelb&quot;, s2 = &quot;kelb&quot;
<strong>Output:</strong> true
<strong>Giải thích:</strong> Hai chuỗi đã bằng nhau nên không cần thực hiện thao tác hoán đổi.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s1.length, s2.length &lt;= 100</code></li>
	<li><code>s1.length == s2.length</code></li>
<li><code>s1</code> và <code>s2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Sau không quá một lần hoán đổi trong $s1$, hai chuỗi phải bằng $s2$. Hai chuỗi có thể khác nhau ở $0$ vị trí (đã bằng nhau) hoặc $2$ vị trí (chính xác là hai vị trí được hoán đổi).
>
> Ghi nhận nhiều nhất hai cặp ký tự khác nhau: thất bại nếu có hơn hai cặp, hoặc cặp thứ hai không đảo ngược cặp thứ nhất. Một điểm khác nhau không thể được sửa bằng một lần hoán đổi.

<!-- thinking:end -->

Ta dùng biến $cnt$ để ghi lại số ký tự khác nhau tại cùng vị trí trong hai chuỗi. Nếu hai chuỗi thỏa mãn yêu cầu, $cnt$ phải bằng $0$ hoặc $2$. Ta cũng dùng hai biến ký tự $c1$ và $c2$ để ghi lại các ký tự khác nhau tại cùng vị trí.

Khi duyệt đồng thời hai chuỗi, với hai ký tự $a$ và $b$ ở cùng vị trí, nếu $a \ne b$ thì tăng $cnt$ thêm $1$. Nếu lúc này $cnt$ lớn hơn $2$, hoặc $cnt$ bằng $2$ và $a \ne c2$ hoặc $b \ne c1$, ta trả về `false` ngay. Đừng quên ghi lại $c1$ và $c2$.

Sau khi duyệt xong, nếu $cnt \neq 1$ thì trả về `true`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def areAlmostEqual(self, s1: str, s2: str) -> bool:
        cnt = 0
        c1 = c2 = None
        for a, b in zip(s1, s2):
            if a != b:
                cnt += 1
                if cnt > 2 or (cnt == 2 and (a != c2 or b != c1)):
                    return False
                c1, c2 = a, b
        return cnt != 1
```

#### Java

```java
class Solution {
    public boolean areAlmostEqual(String s1, String s2) {
        int cnt = 0;
        char c1 = 0, c2 = 0;
        for (int i = 0; i < s1.length(); ++i) {
            char a = s1.charAt(i), b = s2.charAt(i);
            if (a != b) {
                if (++cnt > 2 || (cnt == 2 && (a != c2 || b != c1))) {
                    return false;
                }
                c1 = a;
                c2 = b;
            }
        }
        return cnt != 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool areAlmostEqual(string s1, string s2) {
        int cnt = 0;
        char c1 = 0, c2 = 0;
        for (int i = 0; i < s1.size(); ++i) {
            char a = s1[i], b = s2[i];
            if (a != b) {
                if (++cnt > 2 || (cnt == 2 && (a != c2 || b != c1))) {
                    return false;
                }
                c1 = a, c2 = b;
            }
        }
        return cnt != 1;
    }
};
```

#### Go

```go
func areAlmostEqual(s1 string, s2 string) bool {
	cnt := 0
	var c1, c2 byte
	for i := range s1 {
		a, b := s1[i], s2[i]
		if a != b {
			cnt++
			if cnt > 2 || (cnt == 2 && (a != c2 || b != c1)) {
				return false
			}
			c1, c2 = a, b
		}
	}
	return cnt != 1
}
```

#### TypeScript

```ts
function areAlmostEqual(s1: string, s2: string): boolean {
    let c1, c2;
    let cnt = 0;
    for (let i = 0; i < s1.length; ++i) {
        const a = s1.charAt(i);
        const b = s2.charAt(i);
        if (a != b) {
            if (++cnt > 2 || (cnt == 2 && (a != c2 || b != c1))) {
                return false;
            }
            c1 = a;
            c2 = b;
        }
    }
    return cnt != 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn are_almost_equal(s1: String, s2: String) -> bool {
        if s1 == s2 {
            return true;
        }
        let (s1, s2) = (s1.as_bytes(), s2.as_bytes());
        let mut idxs = vec![];
        for i in 0..s1.len() {
            if s1[i] != s2[i] {
                idxs.push(i);
            }
        }
        if idxs.len() != 2 {
            return false;
        }
        s1[idxs[0]] == s2[idxs[1]] && s2[idxs[0]] == s1[idxs[1]]
    }
}
```

#### C

```c
bool areAlmostEqual(char* s1, char* s2) {
    int n = strlen(s1);
    int i1 = -1;
    int i2 = -1;
    for (int i = 0; i < n; i++) {
        if (s1[i] != s2[i]) {
            if (i1 == -1) {
                i1 = i;
            } else if (i2 == -1) {
                i2 = i;
            } else {
                return 0;
            }
        }
    }
    if (i1 == -1 && i2 == -1) {
        return 1;
    }
    if (i1 == -1 || i2 == -1) {
        return 0;
    }
    return s1[i1] == s2[i2] && s1[i2] == s2[i1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
