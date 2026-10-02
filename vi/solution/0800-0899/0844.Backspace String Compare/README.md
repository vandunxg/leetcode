---
comments: true
difficulty: Easy
tags:
    - Stack
    - Two Pointers
    - String
    - Simulation
---

<!-- problem:start -->

# [844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare)

[中文文档](/solution/0800-0899/0844.Backspace%20String%20Compare/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>t</code>, trả về <code>true</code> <em>nếu chúng giống nhau khi được nhập vào hai trình soạn thảo văn bản trống</em>. Ký tự <code>&#39;#&#39;</code> biểu thị thao tác xóa lùi.</p>

<p>Lưu ý, nếu xóa lùi khi văn bản đang trống thì văn bản vẫn trống.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ab#c&quot;, t = &quot;ad#c&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Cả s và t đều trở thành &quot;ac&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ab##&quot;, t = &quot;c#d#&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Cả s và t đều trở thành &quot;&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;a#c&quot;, t = &quot;b&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> s trở thành &quot;c&quot;, còn t trở thành &quot;b&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code><span>1 &lt;= s.length, t.length &lt;= 200</span></code></li>
	<li><span><code>s</code> và <code>t</code> chỉ chứa chữ cái viết thường và ký tự <code>&#39;#&#39;</code>.</span></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Có thể giải bài toán với độ phức tạp thời gian <code>O(n)</code> và độ phức tạp không gian <code>O(1)</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần so sánh hai chuỗi sau khi xử lý thao tác xóa lùi. Tạo hẳn các chuỗi đã xử lý sẽ tốn thêm bộ nhớ tuyến tính; cách giải này duyệt trực tiếp chuỗi ban đầu.
>
> Duyệt từ phải sang trái, dùng bộ đếm số ký tự cần bỏ qua cho mỗi dấu `#` và các ký tự mà dấu đó xóa, sau đó so sánh ký tự kế tiếp còn lại. Nếu một chuỗi hết trước thì hai chuỗi khác nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def backspaceCompare(self, s: str, t: str) -> bool:
        i, j, skip1, skip2 = len(s) - 1, len(t) - 1, 0, 0
        while i >= 0 or j >= 0:
            while i >= 0:
                if s[i] == '#':
                    skip1 += 1
                    i -= 1
                elif skip1:
                    skip1 -= 1
                    i -= 1
                else:
                    break
            while j >= 0:
                if t[j] == '#':
                    skip2 += 1
                    j -= 1
                elif skip2:
                    skip2 -= 1
                    j -= 1
                else:
                    break
            if i >= 0 and j >= 0:
                if s[i] != t[j]:
                    return False
            elif i >= 0 or j >= 0:
                return False
            i, j = i - 1, j - 1
        return True
```

#### Java

```java
class Solution {
    public boolean backspaceCompare(String s, String t) {
        int i = s.length() - 1, j = t.length() - 1;
        int skip1 = 0, skip2 = 0;
        for (; i >= 0 || j >= 0; --i, --j) {
            while (i >= 0) {
                if (s.charAt(i) == '#') {
                    ++skip1;
                    --i;
                } else if (skip1 > 0) {
                    --skip1;
                    --i;
                } else {
                    break;
                }
            }
            while (j >= 0) {
                if (t.charAt(j) == '#') {
                    ++skip2;
                    --j;
                } else if (skip2 > 0) {
                    --skip2;
                    --j;
                } else {
                    break;
                }
            }
            if (i >= 0 && j >= 0) {
                if (s.charAt(i) != t.charAt(j)) {
                    return false;
                }
            } else if (i >= 0 || j >= 0) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        int i = s.size() - 1, j = t.size() - 1;
        int skip1 = 0, skip2 = 0;
        for (; i >= 0 || j >= 0; --i, --j) {
            while (i >= 0) {
                if (s[i] == '#') {
                    ++skip1;
                    --i;
                } else if (skip1) {
                    --skip1;
                    --i;
                } else
                    break;
            }
            while (j >= 0) {
                if (t[j] == '#') {
                    ++skip2;
                    --j;
                } else if (skip2) {
                    --skip2;
                    --j;
                } else
                    break;
            }
            if (i >= 0 && j >= 0) {
                if (s[i] != t[j]) return false;
            } else if (i >= 0 || j >= 0)
                return false;
        }
        return true;
    }
};
```

#### Go

```go
func backspaceCompare(s string, t string) bool {
	i, j := len(s)-1, len(t)-1
	skip1, skip2 := 0, 0
	for ; i >= 0 || j >= 0; i, j = i-1, j-1 {
		for i >= 0 {
			if s[i] == '#' {
				skip1++
				i--
			} else if skip1 > 0 {
				skip1--
				i--
			} else {
				break
			}
		}
		for j >= 0 {
			if t[j] == '#' {
				skip2++
				j--
			} else if skip2 > 0 {
				skip2--
				j--
			} else {
				break
			}
		}
		if i >= 0 && j >= 0 {
			if s[i] != t[j] {
				return false
			}
		} else if i >= 0 || j >= 0 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function backspaceCompare(s: string, t: string): boolean {
    let i = s.length - 1;
    let j = t.length - 1;
    while (i >= 0 || j >= 0) {
        let skip = 0;
        while (i >= 0) {
            if (s[i] === '#') {
                skip++;
            } else if (skip !== 0) {
                skip--;
            } else {
                break;
            }
            i--;
        }
        skip = 0;
        while (j >= 0) {
            if (t[j] === '#') {
                skip++;
            } else if (skip !== 0) {
                skip--;
            } else {
                break;
            }
            j--;
        }
        if (s[i] !== t[j]) {
            return false;
        }
        i--;
        j--;
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn backspace_compare(s: String, t: String) -> bool {
        let (s, t) = (s.as_bytes(), t.as_bytes());
        let (mut i, mut j) = (s.len(), t.len());
        while i != 0 || j != 0 {
            let mut skip = 0;
            while i != 0 {
                if s[i - 1] == b'#' {
                    skip += 1;
                } else if skip != 0 {
                    skip -= 1;
                } else {
                    break;
                }
                i -= 1;
            }
            skip = 0;
            while j != 0 {
                if t[j - 1] == b'#' {
                    skip += 1;
                } else if skip != 0 {
                    skip -= 1;
                } else {
                    break;
                }
                j -= 1;
            }
            if i == 0 && j == 0 {
                break;
            }
            if i == 0 || j == 0 {
                return false;
            }
            if s[i - 1] != t[j - 1] {
                return false;
            }
            i -= 1;
            j -= 1;
        }
        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
