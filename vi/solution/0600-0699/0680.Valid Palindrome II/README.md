---
comments: true
difficulty: Easy
tags:
    - Greedy
    - Two Pointers
    - String
---

<!-- problem:start -->

# [680. Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii)

[中文文档](/solution/0600-0699/0680.Valid%20Palindrome%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, trả về <code>true</code> <em>nếu có thể biến </em><code>s</code><em> thành palindrome bằng cách xóa <strong>tối đa một</strong> ký tự</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aba&quot;
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abca&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có thể xóa ký tự &#39;c&#39;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abc&quot;
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Xóa tối đa một ký tự để tạo palindrome. Thử mọi cách xóa sẽ tốn thời gian bậc hai khi $n\le 10^5$.
>
> Hai con trỏ chỉ gặp chỗ không khớp nhiều nhất một lần: kiểm tra đoạn còn lại sau khi bỏ ký tự bên trái hoặc bên phải. Lượt kiểm tra thêm vẫn có độ phức tạp tuyến tính.

<!-- thinking:end -->

Ta dùng hai con trỏ lần lượt trỏ đến hai đầu chuỗi. Mỗi bước, kiểm tra hai ký tự mà chúng trỏ tới có giống nhau không. Nếu khác nhau, kiểm tra xem chuỗi có phải palindrome sau khi xóa ký tự ở vị trí con trỏ trái hoặc con trỏ phải hay không. Nếu hai ký tự giống nhau, dịch cả hai con trỏ vào giữa một vị trí cho đến khi chúng gặp nhau.

Nếu đến cuối quá trình duyệt mà không gặp hai ký tự khác nhau, bản thân chuỗi đã là palindrome và ta trả về `true`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validPalindrome(self, s: str) -> bool:
        def check(i, j):
            while i < j:
                if s[i] != s[j]:
                    return False
                i, j = i + 1, j - 1
            return True

        i, j = 0, len(s) - 1
        while i < j:
            if s[i] != s[j]:
                return check(i, j - 1) or check(i + 1, j)
            i, j = i + 1, j - 1
        return True
```

#### Java

```java
class Solution {
    private char[] s;

    public boolean validPalindrome(String S) {
        this.s = S.toCharArray();
        for (int i = 0, j = s.length - 1; i < j; ++i, --j) {
            if (s[i] != s[j]) {
                return check(i + 1, j) || check(i, j - 1);
            }
        }
        return true;
    }

    private boolean check(int i, int j) {
        for (; i < j; ++i, --j) {
            if (s[i] != s[j]) {
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
    bool validPalindrome(string s) {
        auto check = [&](int i, int j) {
            for (; i < j; ++i, --j) {
                if (s[i] != s[j]) {
                    return false;
                }
            }
            return true;
        };
        for (int i = 0, j = s.size() - 1; i < j; ++i, --j) {
            if (s[i] != s[j]) {
                return check(i + 1, j) || check(i, j - 1);
            }
        }
        return true;
    }
};
```

#### Go

```go
func validPalindrome(s string) bool {
	check := func(i, j int) bool {
		for ; i < j; i, j = i+1, j-1 {
			if s[i] != s[j] {
				return false
			}
		}
		return true
	}
	for i, j := 0, len(s)-1; i < j; i, j = i+1, j-1 {
		if s[i] != s[j] {
			return check(i+1, j) || check(i, j-1)
		}
	}
	return true
}
```

#### TypeScript

```ts
function validPalindrome(s: string): boolean {
    const check = (i: number, j: number): boolean => {
        for (; i < j; ++i, --j) {
            if (s[i] !== s[j]) {
                return false;
            }
        }
        return true;
    };
    for (let i = 0, j = s.length - 1; i < j; ++i, --j) {
        if (s[i] !== s[j]) {
            return check(i + 1, j) || check(i, j - 1);
        }
    }
    return true;
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var validPalindrome = function (s) {
    const check = function (i, j) {
        for (; i < j; ++i, --j) {
            if (s[i] !== s[j]) {
                return false;
            }
        }
        return true;
    };
    for (let i = 0, j = s.length - 1; i < j; ++i, --j) {
        if (s[i] !== s[j]) {
            return check(i + 1, j) || check(i, j - 1);
        }
    }
    return true;
};
```

#### C#

```cs
public class Solution {
    public bool ValidPalindrome(string s) {
        int i = 0, j = s.Length - 1;
        while (i < j && s[i] == s[j]) {
            i++;
            j--;
        }
        if (i >= j) {
            return true;
        }
        return check(s, i + 1, j) || check(s, i, j - 1);
    }

    private bool check(string s, int i, int j) {
        while (i < j) {
            if (s[i] != s[j]) {
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
