---
comments: true
difficulty: Medium
rating: 1284
source: Weekly Contest 503 Q2
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [3941. Password Strength](https://leetcode.com/problems/password-strength)

[中文文档](/solution/3900-3999/3941.Password%20Strength/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>password</code>.</p>

<p><strong>Độ mạnh</strong> của mật khẩu được tính theo các quy tắc sau:</p>

<ul>
	<li>1 điểm cho mỗi chữ cái thường phân biệt (<code>&#39;a&#39;</code> đến <code>&#39;z&#39;</code>).</li>
	<li>2 điểm cho mỗi chữ cái hoa phân biệt (<code>&#39;A&#39;</code> đến <code>&#39;Z&#39;</code>).</li>
	<li>3 điểm cho mỗi chữ số phân biệt (<code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code>).</li>
	<li>5 điểm cho mỗi ký tự đặc biệt phân biệt thuộc tập <code>&quot;!@#$&quot;</code>.</li>
</ul>

<p>Mỗi ký tự đóng góp <strong>nhiều nhất</strong> một lần, ngay cả khi nó xuất hiện nhiều lần.</p>

<p>Trả về một số nguyên biểu thị độ mạnh của mật khẩu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">password = &quot;aA1!&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các ký tự phân biệt là <code>&#39;a&#39;</code>, <code>&#39;A&#39;</code>, <code>&#39;1&#39;</code> và <code>&#39;!&#39;</code>.</li>
	<li>Do đó, <code>strength = 1 + 2 + 3 + 5 = 11</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">password = &quot;bbB11#&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các ký tự phân biệt là <code>&#39;b&#39;</code>, <code>&#39;B&#39;</code>, <code>&#39;1&#39;</code> và <code>&#39;#&#39;</code>.</li>
	<li>Do đó, <code>strength = 1 + 2 + 3 + 5 = 11</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= password.length &lt;= 10<sup>5</sup></code></li>
	<li><code>password</code> gồm các chữ cái tiếng Anh thường và hoa, chữ số, cùng các ký tự đặc biệt thuộc <code>&quot;!@#$&quot;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Độ mạnh chỉ tính điểm cho mỗi ký tự phân biệt một lần, nên trước hết ta đưa chuỗi vào một set.
>
> Điểm số phụ thuộc vào nhóm ký tự: $1$ cho chữ thường, $2$ cho chữ hoa, $3$ cho chữ số và $5$ cho ký tự đặc biệt. $n\le 10^5$, nên một lần tạo set cùng một lần duyệt là đủ.

<!-- thinking:end -->

Ta lưu mỗi ký tự trong chuỗi đầu vào vào một hash set $\textit{st}$, nhờ đó có thể nhanh chóng bảo đảm mỗi ký tự phân biệt chỉ được tính một lần.

Sau đó, ta duyệt qua từng ký tự trong $\textit{st}$ và tính độ mạnh của mật khẩu theo các quy tắc:

- Nếu ký tự là chữ thường ('a' đến 'z'), cộng 1 điểm.
- Nếu ký tự là chữ hoa ('A' đến 'Z'), cộng 2 điểm.
- Nếu ký tự là chữ số ('0' đến '9'), cộng 3 điểm.
- Nếu ký tự là ký tự đặc biệt (thuộc tập "!@#$"), cộng 5 điểm.

Cuối cùng, trả về độ mạnh của mật khẩu đã tính được.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi đầu vào. Độ phức tạp bộ nhớ là $O(m)$, trong đó $m$ là số ký tự phân biệt trong chuỗi đầu vào.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def passwordStrength(self, password: str) -> int:
        st = set(password)
        ans = 0
        for ch in st:
            if ch.islower():
                ans += 1
            elif ch.isupper():
                ans += 2
            elif ch.isdigit():
                ans += 3
            else:
                ans += 5
        return ans
```

#### Java

```java
class Solution {
    public int passwordStrength(String password) {
        var st = password.chars().mapToObj(c -> (char) c).collect(Collectors.toSet());

        int ans = 0;

        for (char ch : st) {
            if (Character.isLowerCase(ch)) {
                ans += 1;
            } else if (Character.isUpperCase(ch)) {
                ans += 2;
            } else if (Character.isDigit(ch)) {
                ans += 3;
            } else {
                ans += 5;
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
    int passwordStrength(string password) {
        unordered_set<char> st(password.begin(), password.end());

        int ans = 0;

        for (char ch : st) {
            if (islower(ch)) {
                ans += 1;
            } else if (isupper(ch)) {
                ans += 2;
            } else if (isdigit(ch)) {
                ans += 3;
            } else {
                ans += 5;
            }
        }

        return ans;
    }
};
```

#### Go

```go
func passwordStrength(password string) (ans int) {
	st := map[rune]struct{}{}

	for _, ch := range password {
		st[ch] = struct{}{}
	}

	for ch := range st {
		switch {
		case unicode.IsLower(ch):
			ans += 1
		case unicode.IsUpper(ch):
			ans += 2
		case unicode.IsDigit(ch):
			ans += 3
		default:
			ans += 5
		}
	}

	return
}
```

#### TypeScript

```ts
function passwordStrength(password: string): number {
    const st = new Set(password);

    let ans = 0;

    for (const ch of st) {
        if (/[a-z]/u.test(ch)) {
            ans += 1;
        } else if (/[A-Z]/u.test(ch)) {
            ans += 2;
        } else if (/\d/u.test(ch)) {
            ans += 3;
        } else {
            ans += 5;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
