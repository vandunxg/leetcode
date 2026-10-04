---
comments: true
difficulty: Hard
rating: 2605
source: Weekly Contest 439 Q4
tags:
    - Greedy
    - String
    - String Matching
---

<!-- problem:start -->

# [3474. Lexicographically Smallest Generated String](https://leetcode.com/problems/lexicographically-smallest-generated-string)

[中文文档](/solution/3400-3499/3474.Lexicographically%20Smallest%20Generated%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <code>str1</code> và <code>str2</code>, có độ dài lần lượt là <code>n</code> và <code>m</code>.</p>

<p>Một chuỗi <code>word</code> có độ dài <code>n + m - 1</code> được gọi là <strong>được tạo ra</strong> bởi <code>str1</code> và <code>str2</code> nếu thỏa mãn các điều kiện sau với <strong>mỗi</strong> chỉ số <code>0 &lt;= i &lt;= n - 1</code>:</p>

<ul>
	<li>Nếu <code>str1[i] == &#39;T&#39;</code>, <strong><span data-keyword="substring-nonempty">chuỗi con</span></strong> của <code>word</code> có kích thước <code>m</code> bắt đầu tại chỉ số <code>i</code> phải <strong>bằng</strong> <code>str2</code>, tức là <code>word[i..(i + m - 1)] == str2</code>.</li>
	<li>Nếu <code>str1[i] == &#39;F&#39;</code>, <strong><span data-keyword="substring-nonempty">chuỗi con</span></strong> của <code>word</code> có kích thước <code>m</code> bắt đầu tại chỉ số <code>i</code> phải <strong>khác</strong> <code>str2</code>, tức là <code>word[i..(i + m - 1)] != str2</code>.</li>
</ul>

<p>Hãy trả về chuỗi khả dĩ <strong><span data-keyword="lexicographically-smaller-string">nhỏ nhất theo thứ tự từ điển</span></strong> có thể được <strong>tạo ra</strong> bởi <code>str1</code> và <code>str2</code>. Nếu không thể tạo ra chuỗi nào, hãy trả về chuỗi rỗng <code>&quot;&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">str1 = &quot;TFTF&quot;, str2 = &quot;ab&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;ababa&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<h4>Bảng dưới đây biểu diễn chuỗi <code>&quot;ababa&quot;</code></h4>

<table>
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Chỉ số</th>
			<th style="border: 1px solid black;">T/F</th>
			<th style="border: 1px solid black;">Chuỗi con có độ dài <code>m</code></th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;"><code>&#39;T&#39;</code></td>
			<td style="border: 1px solid black;">&quot;ab&quot;</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>&#39;F&#39;</code></td>
			<td style="border: 1px solid black;">&quot;ba&quot;</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>&#39;T&#39;</code></td>
			<td style="border: 1px solid black;">&quot;ab&quot;</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;"><code>&#39;F&#39;</code></td>
			<td style="border: 1px solid black;">&quot;ba&quot;</td>
		</tr>
	</tbody>
</table>

<p>Các chuỗi <code>&quot;ababa&quot;</code> và <code>&quot;ababb&quot;</code> có thể được tạo ra bởi <code>str1</code> và <code>str2</code>.</p>

<p>Trả về <code>&quot;ababa&quot;</code> vì đây là chuỗi nhỏ hơn theo thứ tự từ điển.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">str1 = &quot;TFTF&quot;, str2 = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể tạo ra chuỗi nào thỏa mãn các điều kiện.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">str1 = &quot;F&quot;, str2 = &quot;d&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;a&quot;</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == str1.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= m == str2.length &lt;= 500</code></li>
	<li><code>str1</code> chỉ gồm <code>&#39;T&#39;</code> hoặc <code>&#39;F&#39;</code>.</li>
	<li><code>str2</code> chỉ gồm các ký tự tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> $s[i]=\texttt{T}$ buộc $ans[i..i+m)=t$; $\texttt{F}$ cấm sự bằng nhau. Chuỗi có độ dài $n+m-1$ và cần nhỏ nhất theo thứ tự từ điển, vì vậy ta bắt đầu với toàn bộ ký tự $\texttt{a}$.
>
> Các ràng buộc T có thể xung đột và phải được ghi trước, đồng thời đánh dấu $\textit{fixed}$. Một ràng buộc F vẫn cho kết quả bằng $t$ thì phải thay đổi một vị trí chưa cố định.
>
> Ta thay đổi vị trí chưa cố định ngoài cùng bên phải thành $\texttt{b}$ để giữ các ký tự $\texttt{a}$ ở phía trước. Nếu không còn vị trí chưa cố định nào, bài toán không có lời giải.

<!-- thinking:end -->

Đặt $str1$ là $s$ và $str2$ là $t$.

Ta có thể sử dụng chuỗi $ans$ có độ dài $n + m - 1$ để lưu chuỗi được tạo ra, trong đó mỗi ký tự của $ans$ ban đầu được đặt là $'a'$. Ta cũng cần một mảng boolean $fixed$ có độ dài $n + m - 1$ để ghi lại những vị trí trong $ans$ đã được cố định.

Đầu tiên, ta duyệt qua chuỗi $s$. Với mỗi chỉ số $i$, nếu $s[i]$ là 'T', ta cần đặt chuỗi con của $ans$ bắt đầu tại chỉ số $i$ và có độ dài $m$ thành $t$. Trong quá trình này, nếu phát hiện một vị trí đã được cố định nhưng ký tự tại đó không khớp với ký tự tương ứng trong $t$, điều đó có nghĩa là không thể tạo ra chuỗi hợp lệ, nên ta lập tức trả về chuỗi rỗng.

Tiếp theo, ta lại duyệt qua $s$. Với mỗi chỉ số $i$, nếu $s[i]$ là 'F', ta cần kiểm tra xem chuỗi con của $ans$ bắt đầu tại chỉ số $i$ và có độ dài $m$ có bằng $t$ hay không. Nếu bằng, ta cần tìm một vị trí trong chuỗi con này và đổi ký tự tại đó thành 'b' (vì 'b' lớn hơn 'a' theo thứ tự từ điển), để đảm bảo chuỗi con này không bằng $t$. Nếu không tìm được vị trí như vậy, điều đó có nghĩa là không thể tạo ra chuỗi hợp lệ, nên ta lập tức trả về chuỗi rỗng.

Cuối cùng, ta nối các ký tự trong $ans$ thành một chuỗi và trả về chuỗi đó.

Độ phức tạp thời gian là $O(n \times m)$, còn độ phức tạp không gian là $O(n + m)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def generateString(self, s: str, t: str) -> str:
        n, m = len(s), len(t)
        ans = ["a"] * (n + m - 1)
        fixed = [False] * (n + m - 1)

        for i, b in enumerate(s):
            if b != "T":
                continue
            for j, c in enumerate(t):
                k = i + j
                if fixed[k] and ans[k] != c:
                    return ""
                ans[k] = c
                fixed[k] = True

        for i, b in enumerate(s):
            if b != "F":
                continue
            if "".join(ans[i : i + m]) != t:
                continue
            for j in range(i + m - 1, i - 1, -1):
                if not fixed[j]:
                    ans[j] = "b"
                    break
            else:
                return ""

        return "".join(ans)
```

#### Java

```java
class Solution {
    public String generateString(String s, String t) {
        int n = s.length(), m = t.length();
        char[] ans = new char[n + m - 1];
        boolean[] fixed = new boolean[n + m - 1];

        Arrays.fill(ans, 'a');

        for (int i = 0; i < n; i++) {
            if (s.charAt(i) != 'T') {
                continue;
            }
            for (int j = 0; j < m; j++) {
                int k = i + j;
                if (fixed[k] && ans[k] != t.charAt(j)) {
                    return "";
                }
                ans[k] = t.charAt(j);
                fixed[k] = true;
            }
        }

        for (int i = 0; i < n; i++) {
            if (s.charAt(i) != 'F') {
                continue;
            }

            boolean same = true;
            for (int j = 0; j < m; j++) {
                if (ans[i + j] != t.charAt(j)) {
                    same = false;
                    break;
                }
            }
            if (!same) {
                continue;
            }

            boolean ok = false;
            for (int j = i + m - 1; j >= i; j--) {
                if (!fixed[j]) {
                    ans[j] = 'b';
                    ok = true;
                    break;
                }
            }
            if (!ok) {
                return "";
            }
        }

        return new String(ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string generateString(string s, string t) {
        int n = s.size(), m = t.size();
        string ans(n + m - 1, 'a');
        vector<bool> fixed(n + m - 1, false);

        for (int i = 0; i < n; i++) {
            if (s[i] != 'T') continue;
            for (int j = 0; j < m; j++) {
                int k = i + j;
                if (fixed[k] && ans[k] != t[j]) return "";
                ans[k] = t[j];
                fixed[k] = true;
            }
        }

        for (int i = 0; i < n; i++) {
            if (s[i] != 'F') continue;

            bool same = true;
            for (int j = 0; j < m; j++) {
                if (ans[i + j] != t[j]) {
                    same = false;
                    break;
                }
            }
            if (!same) continue;

            bool ok = false;
            for (int j = i + m - 1; j >= i; j--) {
                if (!fixed[j]) {
                    ans[j] = 'b';
                    ok = true;
                    break;
                }
            }
            if (!ok) return "";
        }

        return ans;
    }
};
```

#### Go

```go
func generateString(s string, t string) string {
	n, m := len(s), len(t)
	ans := make([]byte, n+m-1)
	fixed := make([]bool, n+m-1)

	for i := range ans {
		ans[i] = 'a'
	}

	for i, b := range s {
		if b != 'T' {
			continue
		}
		for j, c := range t {
			k := i + j
			if fixed[k] && ans[k] != byte(c) {
				return ""
			}
			ans[k] = byte(c)
			fixed[k] = true
		}
	}

	for i, b := range s {
		if b != 'F' {
			continue
		}

		same := true
		for j := 0; j < m; j++ {
			if ans[i+j] != t[j] {
				same = false
				break
			}
		}
		if !same {
			continue
		}

		ok := false
		for j := i + m - 1; j >= i; j-- {
			if !fixed[j] {
				ans[j] = 'b'
				ok = true
				break
			}
		}
		if !ok {
			return ""
		}
	}

	return string(ans)
}
```

#### TypeScript

```ts
function generateString(s: string, t: string): string {
    const n = s.length,
        m = t.length;
    const ans: string[] = new Array(n + m - 1).fill('a');
    const fixed: boolean[] = new Array(n + m - 1).fill(false);

    for (let i = 0; i < n; i++) {
        if (s[i] !== 'T') continue;
        for (let j = 0; j < m; j++) {
            const k = i + j;
            if (fixed[k] && ans[k] !== t[j]) return '';
            ans[k] = t[j];
            fixed[k] = true;
        }
    }

    for (let i = 0; i < n; i++) {
        if (s[i] !== 'F') continue;

        let same = true;
        for (let j = 0; j < m; j++) {
            if (ans[i + j] !== t[j]) {
                same = false;
                break;
            }
        }
        if (!same) continue;

        let ok = false;
        for (let j = i + m - 1; j >= i; j--) {
            if (!fixed[j]) {
                ans[j] = 'b';
                ok = true;
                break;
            }
        }
        if (!ok) return '';
    }

    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
