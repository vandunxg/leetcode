---
comments: true
difficulty: Hard
tags:
    - Greedy
    - String
    - Counting Sort
    - Sorting
---

<!-- problem:start -->

# [3088. Make String Anti-palindrome 🔒](https://leetcode.com/problems/make-string-anti-palindrome)

[中文文档](/solution/3000-3099/3088.Make%20String%20Anti-palindrome/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi <code>s</code> có độ dài <strong>chẵn</strong> <code>n</code> được gọi là <strong>chuỗi phản đối xứng</strong> nếu với mọi chỉ số <code>0 &lt;= i &lt; n</code>, <code>s[i] != s[n - i - 1]</code>.</p>

<p>Cho một chuỗi <code>s</code>, nhiệm vụ của bạn là biến <code>s</code> thành một <strong>chuỗi phản đối xứng</strong> bằng cách thực hiện <strong>bất kỳ</strong> số lượng thao tác nào (kể cả 0).</p>

<p>Trong một thao tác, bạn có thể chọn hai ký tự trong <code>s</code> và hoán đổi chúng.</p>

<p>Trả về <em>chuỗi kết quả. Nếu có nhiều chuỗi thỏa mãn điều kiện, hãy trả về chuỗi <span data-keyword="lexicographically-smaller-string">nhỏ nhất theo thứ tự từ điển</span>. Nếu không thể biến đổi thành chuỗi phản đối xứng, trả về </em><code>&quot;-1&quot;</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abca&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;aabc&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>&quot;aabc&quot;</code> là một chuỗi phản đối xứng vì <code>s[0] != s[3]</code> và <code>s[1] != s[2]</code>. Ngoài ra, đây là một cách sắp xếp lại của <code>&quot;abca&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;aabb&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>&quot;aabb&quot;</code> là một chuỗi phản đối xứng vì <code>s[0] != s[3]</code> và <code>s[1] != s[2]</code>. Ngoài ra, đây là một cách sắp xếp lại của <code>&quot;abba&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;cccd&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;-1&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bạn có thể thấy rằng dù sắp xếp lại các ký tự của <code>&quot;cccd&quot;</code> thế nào, vẫn có <code>s[0] == s[3]</code> hoặc <code>s[1] == s[2]</code>. Vì vậy, không thể tạo thành một chuỗi phản đối xứng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s.length % 2 == 0</code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi phản đối xứng có $s[i] \ne s[n-1-i]$, và ta muốn tìm chuỗi nhỏ nhất theo thứ tự từ điển có thể đạt được. $n$ là số chẵn và không vượt quá $10^5$.
>
> Sắp xếp sẽ tạo ra dãy nhỏ nhất. Nếu hai ký tự ở giữa đã khác nhau, mọi cặp đối xứng cũng khác nhau; nếu chúng bằng nhau, một đoạn ký tự ở nửa sau sẽ xung đột với nửa đầu và cần được hoán đổi với một chữ cái khác ở phía sau.
>
> Sau khi sắp xếp, khi $s[m]=s[m-1]$, ta tìm chữ cái khác biệt tiếp theo ở nửa sau và hoán đổi nó vào các vị trí giữa đang xung đột; nếu đã dùng hết chữ cái đó thì không thể thực hiện được.

<!-- thinking:end -->

Bài toán yêu cầu biến chuỗi $s$ thành chuỗi không đối xứng nhỏ nhất theo thứ tự từ điển. Trước hết, ta có thể sắp xếp chuỗi $s$.

Tiếp theo, ta chỉ cần kiểm tra xem hai ký tự ở giữa $s[m]$ và $s[m-1]$ có bằng nhau hay không. Nếu chúng bằng nhau, ta tìm ký tự đầu tiên $s[i]$ ở nửa sau khác với $s[m]$, dùng con trỏ $j$ trỏ đến $m$, rồi hoán đổi $s[i]$ và $s[j]$. Nếu không tìm thấy ký tự $s[i]$ như vậy, điều đó có nghĩa là không thể biến đổi chuỗi $s$ thành chuỗi không đối xứng, nên trả về `"1"`. Nếu tìm thấy, thực hiện thao tác hoán đổi, tăng $i$ và $j$, kiểm tra xem $s[j]$ và $s[n-j-1]$ có bằng nhau hay không; nếu bằng nhau, tiếp tục hoán đổi cho đến khi $i$ vượt quá độ dài chuỗi.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeAntiPalindrome(self, s: str) -> str:
        cs = sorted(s)
        n = len(cs)
        m = n // 2
        if cs[m] == cs[m - 1]:
            i = m
            while i < n and cs[i] == cs[i - 1]:
                i += 1
            j = m
            while j < n and cs[j] == cs[n - j - 1]:
                if i >= n:
                    return "-1"
                cs[i], cs[j] = cs[j], cs[i]
                i, j = i + 1, j + 1
        return "".join(cs)
```

#### Java

```java
class Solution {
    public String makeAntiPalindrome(String s) {
        char[] cs = s.toCharArray();
        Arrays.sort(cs);
        int n = cs.length;
        int m = n / 2;
        if (cs[m] == cs[m - 1]) {
            int i = m;
            while (i < n && cs[i] == cs[i - 1]) {
                ++i;
            }
            for (int j = m; j < n && cs[j] == cs[n - j - 1]; ++i, ++j) {
                if (i >= n) {
                    return "-1";
                }
                char t = cs[i];
                cs[i] = cs[j];
                cs[j] = t;
            }
        }
        return new String(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string makeAntiPalindrome(string s) {
        sort(s.begin(), s.end());
        int n = s.length();
        int m = n / 2;
        if (s[m] == s[m - 1]) {
            int i = m;
            while (i < n && s[i] == s[i - 1]) {
                ++i;
            }
            for (int j = m; j < n && s[j] == s[n - j - 1]; ++i, ++j) {
                if (i >= n) {
                    return "-1";
                }
                swap(s[i], s[j]);
            }
        }
        return s;
    }
};
```

#### Go

```go
func makeAntiPalindrome(s string) string {
	cs := []byte(s)
	sort.Slice(cs, func(i, j int) bool { return cs[i] < cs[j] })
	n := len(cs)
	m := n / 2
	if cs[m] == cs[m-1] {
		i := m
		for i < n && cs[i] == cs[i-1] {
			i++
		}
		for j := m; j < n && cs[j] == cs[n-j-1]; i, j = i+1, j+1 {
			if i >= n {
				return "-1"
			}
			cs[i], cs[j] = cs[j], cs[i]
		}
	}
	return string(cs)
}
```

#### TypeScript

```ts
function makeAntiPalindrome(s: string): string {
    const cs: string[] = s.split('').sort();
    const n: number = cs.length;
    const m = n >> 1;
    if (cs[m] === cs[m - 1]) {
        let i = m;
        for (; i < n && cs[i] === cs[i - 1]; i++);
        for (let j = m; j < n && cs[j] === cs[n - j - 1]; ++i, ++j) {
            if (i >= n) {
                return '-1';
            }
            [cs[j], cs[i]] = [cs[i], cs[j]];
        }
    }
    return cs.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
