---
comments: true
difficulty: Easy
tags:
    - String
---

<!-- problem:start -->

# [482. License Key Formatting](https://leetcode.com/problems/license-key-formatting)

[中文文档](/solution/0400-0499/0482.License%20Key%20Formatting/README.md)

## Mô tả

<!-- description:start -->

<p>Cho license key được biểu diễn bằng chuỗi <code>s</code>, chỉ gồm chữ và số cùng dấu gạch ngang. Chuỗi được chia thành <code>n + 1</code> nhóm bởi <code>n</code> dấu gạch ngang. Ngoài ra, cho số nguyên <code>k</code>.</p>

<p>Hãy định dạng lại chuỗi <code>s</code> sao cho mỗi nhóm có đúng <code>k</code> ký tự, ngoại trừ nhóm đầu tiên có thể ngắn hơn <code>k</code> nhưng phải có ít nhất một ký tự. Giữa hai nhóm phải có dấu gạch ngang, đồng thời cần chuyển tất cả chữ thường thành chữ hoa.</p>

<p>Trả về <em>license key sau khi định dạng lại</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;5F3Z-2e-9-w&quot;, k = 4
<strong>Đầu ra:</strong> &quot;5F3Z-2E9W&quot;
<strong>Giải thích:</strong> Chuỗi s được chia thành hai nhóm, mỗi nhóm có 4 ký tự.
Lưu ý rằng hai dấu gạch ngang thừa không cần thiết và có thể được bỏ đi.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;2-5g-3-J&quot;, k = 2
<strong>Đầu ra:</strong> &quot;2-5G-3J&quot;
<strong>Giải thích:</strong> Chuỗi s được chia thành ba nhóm, mỗi nhóm có 2 ký tự, ngoại trừ nhóm đầu tiên có thể ngắn hơn như đã nêu ở trên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm chữ cái tiếng Anh, chữ số và dấu gạch ngang <code>&#39;-&#39;</code>.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi bỏ các dấu gạch ngang cũ, ta nhóm mỗi $k$ ký tự từ phải sang trái; nhóm đầu tiên có thể ngắn hơn, và các chữ cái phải được viết hoa. Có thể duyệt từ phải sang trái rồi đảo chuỗi, nhưng dù sao ta cũng cần biết độ dài nhóm đầu tiên.
>
> Số ký tự chữ và số chia lấy dư cho $k$ chính là độ dài nhóm đầu tiên (nếu dư $0$ thì độ dài là $k$). Duyệt từ trái sang phải, ghi các ký tự viết hoa, chèn dấu gạch ngang khi bộ đếm về $0$, rồi bỏ dấu gạch ngang ở cuối nếu có.
>
> Biết trước độ dài nhóm đầu tiên giúp ta duyệt từ trái sang phải và giữ được nhóm đầu ngắn hơn mà không cần đảo chuỗi lần nữa.

<!-- thinking:end -->

Trước tiên, ta đếm số ký tự trong chuỗi $s$ sau khi bỏ các dấu gạch ngang, rồi lấy phần dư khi chia cho $k$ để xác định số ký tự của nhóm đầu tiên. Nếu phần dư bằng $0$, nhóm đầu có $k$ ký tự; nếu không, số ký tự của nhóm đầu bằng phần dư đó.

Tiếp theo, ta duyệt chuỗi $s$. Nếu ký tự hiện tại là dấu gạch ngang thì bỏ qua; nếu không, chuyển ký tự thành chữ hoa rồi thêm vào chuỗi kết quả. Đồng thời, ta duy trì bộ đếm $cnt$ biểu diễn số ký tự còn lại trong nhóm hiện tại. Khi $cnt$ giảm về $0$, ta đặt lại $cnt$ thành $k$; nếu ký tự hiện tại không phải ký tự cuối cùng thì thêm dấu gạch ngang vào chuỗi kết quả.

Cuối cùng, ta xóa dấu gạch ngang ở cuối chuỗi kết quả rồi trả về chuỗi này.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Nếu không tính phần bộ nhớ dành cho chuỗi kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def licenseKeyFormatting(self, s: str, k: int) -> str:
        n = len(s)
        cnt = (n - s.count("-")) % k or k
        ans = []
        for i, c in enumerate(s):
            if c == "-":
                continue
            ans.append(c.upper())
            cnt -= 1
            if cnt == 0:
                cnt = k
                if i != n - 1:
                    ans.append("-")
        return "".join(ans).rstrip("-")
```

#### Java

```java
class Solution {
    public String licenseKeyFormatting(String s, int k) {
        int n = s.length();
        int cnt = (int) (n - s.chars().filter(ch -> ch == '-').count()) % k;
        if (cnt == 0) {
            cnt = k;
        }
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < n; i++) {
            char c = s.charAt(i);
            if (c == '-') {
                continue;
            }
            ans.append(Character.toUpperCase(c));
            --cnt;
            if (cnt == 0) {
                cnt = k;
                if (i != n - 1) {
                    ans.append('-');
                }
            }
        }
        if (ans.length() > 0 && ans.charAt(ans.length() - 1) == '-') {
            ans.deleteCharAt(ans.length() - 1);
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string licenseKeyFormatting(string s, int k) {
        int n = s.length();
        int cnt = (n - count(s.begin(), s.end(), '-')) % k;
        if (cnt == 0) {
            cnt = k;
        }
        string ans;
        for (int i = 0; i < n; ++i) {
            char c = s[i];
            if (c == '-') {
                continue;
            }
            ans += toupper(c);
            if (--cnt == 0) {
                cnt = k;
                if (i != n - 1) {
                    ans += '-';
                }
            }
        }
        if (!ans.empty() && ans.back() == '-') {
            ans.pop_back();
        }
        return ans;
    }
};
```

#### Go

```go
func licenseKeyFormatting(s string, k int) string {
	n := len(s)
	cnt := (n - strings.Count(s, "-")) % k
	if cnt == 0 {
		cnt = k
	}

	var ans strings.Builder
	for i := 0; i < n; i++ {
		c := s[i]
		if c == '-' {
			continue
		}
		if cnt == 0 {
			cnt = k
			ans.WriteByte('-')
		}
		ans.WriteRune(unicode.ToUpper(rune(c)))
		cnt--
	}

	return ans.String()
}
```

#### TypeScript

```ts
function licenseKeyFormatting(s: string, k: number): string {
    const n = s.length;
    let cnt = (n - (s.match(/-/g) || []).length) % k || k;
    const ans: string[] = [];
    for (let i = 0; i < n; i++) {
        const c = s[i];
        if (c === '-') {
            continue;
        }
        ans.push(c.toUpperCase());
        if (--cnt === 0) {
            cnt = k;
            if (i !== n - 1) {
                ans.push('-');
            }
        }
    }
    while (ans.at(-1) === '-') {
        ans.pop();
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
