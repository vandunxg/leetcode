---
comments: true
difficulty: Easy
tags:
    - String
---

<!-- problem:start -->

# [3571. Find the Shortest Superstring II 🔒](https://leetcode.com/problems/find-the-shortest-superstring-ii)

[中文文档](/solution/3500-3599/3571.Find%20the%20Shortest%20Superstring%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <strong>hai</strong> chuỗi, <code>s1</code> và <code>s2</code>. Hãy trả về chuỗi <strong>ngắn nhất</strong> <em>có thể</em> chứa cả <code>s1</code> và <code>s2</code> dưới dạng chuỗi con. Nếu có nhiều đáp án hợp lệ, hãy trả về <em>bất kỳ </em>đáp án nào.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s1 = &quot;aba&quot;, s2 = &quot;bab&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;abab&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>&quot;abab&quot;</code> là chuỗi ngắn nhất chứa cả <code>&quot;aba&quot;</code> và <code>&quot;bab&quot;</code> dưới dạng chuỗi con.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s1 = &quot;aa&quot;, s2 = &quot;aaa&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;aaa&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>&quot;aa&quot;</code> đã nằm trong <code>&quot;aaa&quot;</code>, nên chuỗi siêu ngắn nhất là <code>&quot;aaa&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li data-end="23" data-start="2"><code>1 &lt;= s1.length &lt;= 100</code></li>
	<li data-end="47" data-start="26"><code>1 &lt;= s2.length &lt;= 100</code></li>
	<li data-end="102" data-is-last-node="" data-start="50"><code>s1</code> và <code>s2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê các phần chồng lấp

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi siêu ngắn nhất là chuỗi dài hơn nếu nó đã chứa chuỗi ngắn hơn, hoặc là phép nối có phần đầu của một chuỗi chồng lên phần cuối của chuỗi kia. Hai chuỗi ngắn, nên ta có thể thử mọi độ dài phần chồng lấp.
>
> Giả sử $s_1$ ngắn hơn. Nếu nó xuất hiện trong $s_2$, ta trả về $s_2$. Nếu không, ta kiểm tra xem phần cuối của $s_1$ có trùng với phần đầu của $s_2$, hoặc phần đầu của $s_1$ có trùng với phần cuối của $s_2$ không, rồi nối chuỗi ngay khi tìm thấy trường hợp đầu tiên; nếu không có phần nào trùng, trả về $s_1+s_2$.

<!-- thinking:end -->

Ta có thể xây dựng chuỗi ngắn nhất chứa cả `s1` và `s2` dưới dạng chuỗi con bằng cách liệt kê các phần chồng lấp của hai chuỗi.

Mục tiêu là tạo ra chuỗi ngắn nhất chứa cả `s1` và `s2` dưới dạng chuỗi con. Vì chuỗi con phải là một dãy liên tiếp, ta thử chồng **phần cuối** của chuỗi này lên **phần đầu** của chuỗi kia, từ đó giảm tổng độ dài khi nối.

Cụ thể, có một số trường hợp:

1. **Chứa**: Nếu `s1` là chuỗi con của `s2`, thì bản thân `s2` đã thỏa mãn điều kiện, nên chỉ cần trả về `s2`; trường hợp ngược lại cũng tương tự.
2. **Nối s1 trước s2**: Liệt kê xem phần cuối của `s1` có trùng với phần đầu của `s2` không, rồi nối hai chuỗi sau khi tìm được phần chồng lấp dài nhất.
3. **Nối s2 trước s1**: Liệt kê xem phần đầu của `s1` có trùng với phần cuối của `s2` không, rồi nối hai chuỗi sau khi tìm được phần chồng lấp dài nhất.
4. **Không chồng lấp**: Nếu phần cuối/phần đầu của hai chuỗi không chồng lấp, chỉ cần trả về `s1 + s2`.

Ta thử cả hai thứ tự nối và trả về chuỗi ngắn hơn (nếu hai chuỗi có cùng độ dài thì trả về chuỗi nào cũng được).

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài lớn hơn của `s1` và `s2`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestSuperstring(self, s1: str, s2: str) -> str:
        m, n = len(s1), len(s2)
        if m > n:
            return self.shortestSuperstring(s2, s1)
        if s1 in s2:
            return s2
        for i in range(m):
            if s2.startswith(s1[i:]):
                return s1[:i] + s2
            if s2.endswith(s1[: m - i]):
                return s2 + s1[m - i :]
        return s1 + s2
```

#### Java

```java
class Solution {
    public String shortestSuperstring(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        if (m > n) {
            return shortestSuperstring(s2, s1);
        }
        if (s2.contains(s1)) {
            return s2;
        }
        for (int i = 0; i < m; i++) {
            if (s2.startsWith(s1.substring(i))) {
                return s1.substring(0, i) + s2;
            }
            if (s2.endsWith(s1.substring(0, m - i))) {
                return s2 + s1.substring(m - i);
            }
        }
        return s1 + s2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string shortestSuperstring(string s1, string s2) {
        int m = s1.size(), n = s2.size();
        if (m > n) {
            return shortestSuperstring(s2, s1);
        }
        if (s2.find(s1) != string::npos) {
            return s2;
        }
        for (int i = 0; i < m; ++i) {
            if (s2.find(s1.substr(i)) == 0) {
                return s1.substr(0, i) + s2;
            }
            if (s2.rfind(s1.substr(0, m - i)) == s2.size() - (m - i)) {
                return s2 + s1.substr(m - i);
            }
        }
        return s1 + s2;
    }
};
```

#### Go

```go
func shortestSuperstring(s1 string, s2 string) string {
	m, n := len(s1), len(s2)

	if m > n {
		return shortestSuperstring(s2, s1)
	}

	if strings.Contains(s2, s1) {
		return s2
	}

	for i := 0; i < m; i++ {
		if strings.HasPrefix(s2, s1[i:]) {
			return s1[:i] + s2
		}
		if strings.HasSuffix(s2, s1[:m-i]) {
			return s2 + s1[m-i:]
		}
	}

	return s1 + s2
}
```

#### TypeScript

```ts
function shortestSuperstring(s1: string, s2: string): string {
    const m = s1.length,
        n = s2.length;

    if (m > n) {
        return shortestSuperstring(s2, s1);
    }

    if (s2.includes(s1)) {
        return s2;
    }

    for (let i = 0; i < m; i++) {
        if (s2.startsWith(s1.slice(i))) {
            return s1.slice(0, i) + s2;
        }
        if (s2.endsWith(s1.slice(0, m - i))) {
            return s2 + s1.slice(m - i);
        }
    }

    return s1 + s2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
