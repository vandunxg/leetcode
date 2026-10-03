---
comments: true
difficulty: Easy
rating: 1241
source: Biweekly Contest 80 Q1
tags:
    - String
---

<!-- problem:start -->

# [2299. Strong Password Checker II](https://leetcode.com/problems/strong-password-checker-ii)

[中文文档](/solution/2200-2299/2299.Strong%20Password%20Checker%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Một mật khẩu được xem là <strong>mạnh</strong> nếu thỏa mãn tất cả các tiêu chí sau:</p>

<ul>
	<li>Có ít nhất <code>8</code> ký tự.</li>
	<li>Chứa ít nhất <strong>một chữ cái thường</strong>.</li>
	<li>Chứa ít nhất <strong>một chữ cái hoa</strong>.</li>
	<li>Chứa ít nhất <strong>một chữ số</strong>.</li>
	<li>Chứa ít nhất <strong>một ký tự đặc biệt</strong>. Các ký tự đặc biệt là những ký tự trong chuỗi sau: <code>&quot;!@#$%^&amp;*()-+&quot;</code>.</li>
	<li><strong>Không</strong> chứa <code>2</code> ký tự giống nhau ở các vị trí liền kề (ví dụ: <code>&quot;aab&quot;</code> vi phạm điều kiện này, còn <code>&quot;aba&quot;</code> thì không).</li>
</ul>

<p>Cho một chuỗi <code>password</code>, hãy trả về <code>true</code><em> nếu đó là một <strong>mật khẩu mạnh</strong></em>. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> password = &quot;IloveLe3tcode!&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Mật khẩu đáp ứng tất cả các yêu cầu. Do đó, ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> password = &quot;Me+You--IsMyDream&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Mật khẩu không chứa chữ số và còn chứa 2 ký tự giống nhau ở các vị trí liền kề. Do đó, ta trả về false.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> password = &quot;1aB!&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Mật khẩu không đáp ứng yêu cầu về độ dài. Do đó, ta trả về false.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= password.length &lt;= 100</code></li>
	<li><code>password</code> chỉ gồm các chữ cái, chữ số và ký tự đặc biệt: <code>&quot;!@#$%^&amp;*()-+&quot;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng + thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Một mật khẩu cần có độ dài ít nhất $8$, đủ cả bốn nhóm ký tự và không có hai ký tự giống nhau liền kề. Độ dài tối đa là $100$, nên ta có thể duyệt một lần để kiểm tra tất cả. Một mask gồm bốn bit dùng để ghi nhận những nhóm ký tự đã xuất hiện.
>
> Nếu chuỗi quá ngắn hoặc có một cặp ký tự liền kề giống nhau thì từ chối; với mỗi ký tự, ta bật bit tương ứng với nhóm của ký tự đó. Nếu mask cuối cùng bằng $15$ thì cả bốn nhóm ký tự đều xuất hiện.

<!-- thinking:end -->

Theo mô tả đề bài, ta có thể mô phỏng quá trình kiểm tra xem password có đáp ứng các yêu cầu hay không.

Đầu tiên, ta kiểm tra xem độ dài của password có nhỏ hơn $8$ hay không. Nếu có, ta trả về $\textit{false}$.

Tiếp theo, ta dùng một mask $\textit{mask}$ để ghi nhận password có chứa chữ cái thường, chữ cái hoa, chữ số và ký tự đặc biệt hay không. Ta duyệt qua password, với mỗi ký tự, trước tiên kiểm tra xem nó có giống ký tự trước đó hay không. Nếu có, ta trả về $\textit{false}$. Sau đó, ta cập nhật mask $\textit{mask}$ dựa trên loại ký tự. Cuối cùng, ta kiểm tra xem mask $\textit{mask}$ có bằng $15$ hay không. Nếu có, ta trả về $\textit{true}$; ngược lại, ta trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài của password.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def strongPasswordCheckerII(self, password: str) -> bool:
        if len(password) < 8:
            return False
        mask = 0
        for i, c in enumerate(password):
            if i and c == password[i - 1]:
                return False
            if c.islower():
                mask |= 1
            elif c.isupper():
                mask |= 2
            elif c.isdigit():
                mask |= 4
            else:
                mask |= 8
        return mask == 15
```

#### Java

```java
class Solution {
    public boolean strongPasswordCheckerII(String password) {
        if (password.length() < 8) {
            return false;
        }
        int mask = 0;
        for (int i = 0; i < password.length(); ++i) {
            char c = password.charAt(i);
            if (i > 0 && c == password.charAt(i - 1)) {
                return false;
            }
            if (Character.isLowerCase(c)) {
                mask |= 1;
            } else if (Character.isUpperCase(c)) {
                mask |= 2;
            } else if (Character.isDigit(c)) {
                mask |= 4;
            } else {
                mask |= 8;
            }
        }
        return mask == 15;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool strongPasswordCheckerII(string password) {
        if (password.size() < 8) {
            return false;
        }
        int mask = 0;
        for (int i = 0; i < password.size(); ++i) {
            char c = password[i];
            if (i && c == password[i - 1]) {
                return false;
            }
            if (c >= 'a' && c <= 'z') {
                mask |= 1;
            } else if (c >= 'A' && c <= 'Z') {
                mask |= 2;
            } else if (c >= '0' && c <= '9') {
                mask |= 4;
            } else {
                mask |= 8;
            }
        }
        return mask == 15;
    }
};
```

#### Go

```go
func strongPasswordCheckerII(password string) bool {
	if len(password) < 8 {
		return false
	}
	mask := 0
	for i, c := range password {
		if i > 0 && byte(c) == password[i-1] {
			return false
		}
		if unicode.IsLower(c) {
			mask |= 1
		} else if unicode.IsUpper(c) {
			mask |= 2
		} else if unicode.IsDigit(c) {
			mask |= 4
		} else {
			mask |= 8
		}
	}
	return mask == 15
}
```

#### TypeScript

```ts
function strongPasswordCheckerII(password: string): boolean {
    if (password.length < 8) {
        return false;
    }
    let mask = 0;
    for (let i = 0; i < password.length; ++i) {
        const c = password[i];
        if (i && c == password[i - 1]) {
            return false;
        }
        if (c >= 'a' && c <= 'z') {
            mask |= 1;
        } else if (c >= 'A' && c <= 'Z') {
            mask |= 2;
        } else if (c >= '0' && c <= '9') {
            mask |= 4;
        } else {
            mask |= 8;
        }
    }
    return mask == 15;
}
```

#### Rust

```rust
impl Solution {
    pub fn strong_password_checker_ii(password: String) -> bool {
        let s = password.as_bytes();
        let n = password.len();
        if n < 8 {
            return false;
        }
        let mut mask = 0;
        let mut prev = b' ';
        for &c in s.iter() {
            if c == prev {
                return false;
            }
            mask |= if c.is_ascii_uppercase() {
                0b1000
            } else if c.is_ascii_lowercase() {
                0b100
            } else if c.is_ascii_digit() {
                0b10
            } else {
                0b1
            };
            prev = c;
        }
        mask == 0b1111
    }
}
```

#### C

```c
bool strongPasswordCheckerII(char* password) {
    int n = strlen(password);
    if (n < 8) {
        return false;
    }
    int mask = 0;
    char prev = ' ';
    for (int i = 0; i < n; i++) {
        if (prev == password[i]) {
            return false;
        }
        if (islower(password[i])) {
            mask |= 0b1000;
        } else if (isupper(password[i])) {
            mask |= 0b100;
        } else if (isdigit(password[i])) {
            mask |= 0b10;
        } else {
            mask |= 0b1;
        }
        prev = password[i];
    }
    return mask == 0b1111;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
