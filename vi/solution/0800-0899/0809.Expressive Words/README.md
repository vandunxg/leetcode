---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
    - String
---

<!-- problem:start -->

# [809. Expressive Words](https://leetcode.com/problems/expressive-words)

[中文文档](/solution/0800-0899/0809.Expressive%20Words/README.md)

## Mô tả

<!-- description:start -->

<p>Đôi khi người ta lặp lại chữ cái để thể hiện cảm xúc mạnh hơn. Ví dụ:</p>

<ul>
	<li><code>&quot;hello&quot; -&gt; &quot;heeellooo&quot;</code></li>
	<li><code>&quot;hi&quot; -&gt; &quot;hiiii&quot;</code></li>
</ul>

<p>Trong những chuỗi như <code>&quot;heeellooo&quot;</code>, có các nhóm chữ cái giống nhau đứng liền nhau: <code>&quot;h&quot;</code>, <code>&quot;eee&quot;</code>, <code>&quot;ll&quot;</code>, <code>&quot;ooo&quot;</code>.</p>

<p>Cho chuỗi <code>s</code> và mảng các chuỗi truy vấn <code>words</code>. Một từ truy vấn được gọi là <strong>có thể kéo dài</strong> nếu có thể biến nó thành <code>s</code> bằng cách áp dụng phép mở rộng sau một số lần bất kỳ: chọn một nhóm gồm các ký tự <code>c</code> rồi thêm một số ký tự <code>c</code> vào nhóm sao cho kích thước nhóm đạt <strong>ít nhất ba</strong>.</p>

<ul>
	<li>Ví dụ, bắt đầu với <code>&quot;hello&quot;</code>, ta có thể mở rộng nhóm <code>&quot;o&quot;</code> để được <code>&quot;hellooo&quot;</code>, nhưng không thể tạo ra <code>&quot;helloo&quot;</code> vì nhóm <code>&quot;oo&quot;</code> có ít hơn ba ký tự. Ta cũng có thể mở rộng tiếp như <code>&quot;ll&quot; -&gt; &quot;lllll&quot;</code> để được <code>&quot;helllllooo&quot;</code>. Nếu <code>s = &quot;helllllooo&quot;</code>, từ truy vấn <code>&quot;hello&quot;</code> là <strong>có thể kéo dài</strong> nhờ hai phép mở rộng: <code>query = &quot;hello&quot; -&gt; &quot;hellooo&quot; -&gt; &quot;helllllooo&quot; = s</code>.</li>
</ul>

<p>Hãy trả về <em>số lượng chuỗi truy vấn <strong>có thể kéo dài</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;heeellooo&quot;, words = [&quot;hello&quot;, &quot;hi&quot;, &quot;helo&quot;]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> 
Ta có thể mở rộng &quot;e&quot; và &quot;o&quot; trong từ &quot;hello&quot; để được &quot;heeellooo&quot;.
Không thể mở rộng &quot;helo&quot; để được &quot;heeellooo&quot; vì nhóm &quot;ll&quot; không có ít nhất 3 ký tự.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;zzzzzyyyyy&quot;, words = [&quot;zzyy&quot;,&quot;zy&quot;,&quot;zyy&quot;]
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 100</code></li>
	<li><code>s</code> và <code>words[i]</code> chỉ gồm các chữ cái viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm nhóm ký tự khi duyệt + hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Phép kéo dài chỉ tăng độ dài một nhóm ký tự, và nhóm sau khi kéo dài phải có độ dài ít nhất $3$; các chữ cái không thể thay đổi. Cả $s$ và từ truy vấn đều ngắn nên chỉ cần duyệt mỗi từ một lần bằng hai con trỏ.
>
> Ghép các nhóm ký tự liền nhau: các chữ cái phải giống nhau; nhóm tương ứng trong chuỗi đích không được ngắn hơn nhóm trong từ truy vấn, và nếu nhóm trong chuỗi đích ngắn hơn $3$ thì độ dài hai nhóm phải bằng nhau. Cả hai chuỗi phải được duyệt hết cùng lúc.

<!-- thinking:end -->

Ta duyệt mảng $\textit{words}$ và kiểm tra từng từ $t$ xem có thể mở rộng thành $s$ hay không. Nếu có thể, tăng đáp án thêm một.

Vì vậy, mấu chốt là xác định từ $t$ có thể được mở rộng thành $s$ hay không. Ta dùng hàm $\textit{check}(s, t)$ để kiểm tra, với cách triển khai như sau:

Trước tiên, so sánh độ dài của $s$ và $t$. Nếu $t$ dài hơn $s$, trả về ngay $\textit{false}$. Nếu không, dùng hai con trỏ $i$ và $j$ lần lượt trỏ vào $s$ và $t$, ban đầu đều bằng $0$.

Nếu ký tự tại $i$ và $j$ khác nhau thì không thể mở rộng $t$ thành $s$, nên trả về $\textit{false}$. Nếu giống nhau, kiểm tra số lần xuất hiện liên tiếp của ký tự đó tại hai vị trí, lần lượt gọi là $c_1$ và $c_2$. Nếu $c_1 \lt c_2$, hoặc $c_1 \lt 3$ và $c_1 \neq c_2$, thì không thể mở rộng $t$ thành $s$ và ta trả về $\textit{false}$. Nếu không, lần lượt dịch $i$ và $j$ sang phải $c_1$ và $c_2$ vị trí rồi tiếp tục kiểm tra.

Nếu cả $i$ và $j$ đều đến cuối chuỗi, thì có thể mở rộng $t$ thành $s$ và ta trả về $\textit{true}$. Nếu không, trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n \times m + \sum_{i=0}^{m-1} w_i)$, trong đó $n$ và $m$ lần lượt là độ dài của chuỗi $s$ và mảng $\textit{words}$, còn $w_i$ là độ dài của từ thứ $i$ trong mảng $\textit{words}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def expressiveWords(self, s: str, words: List[str]) -> int:
        def check(s, t):
            m, n = len(s), len(t)
            if n > m:
                return False
            i = j = 0
            while i < m and j < n:
                if s[i] != t[j]:
                    return False
                k = i
                while k < m and s[k] == s[i]:
                    k += 1
                c1 = k - i
                i, k = k, j
                while k < n and t[k] == t[j]:
                    k += 1
                c2 = k - j
                j = k
                if c1 < c2 or (c1 < 3 and c1 != c2):
                    return False
            return i == m and j == n

        return sum(check(s, t) for t in words)
```

#### Java

```java
class Solution {
    public int expressiveWords(String s, String[] words) {
        int ans = 0;
        for (String t : words) {
            if (check(s, t)) {
                ++ans;
            }
        }
        return ans;
    }

    private boolean check(String s, String t) {
        int m = s.length(), n = t.length();
        if (n > m) {
            return false;
        }
        int i = 0, j = 0;
        while (i < m && j < n) {
            if (s.charAt(i) != t.charAt(j)) {
                return false;
            }
            int k = i;
            while (k < m && s.charAt(k) == s.charAt(i)) {
                ++k;
            }
            int c1 = k - i;
            i = k;
            k = j;
            while (k < n && t.charAt(k) == t.charAt(j)) {
                ++k;
            }
            int c2 = k - j;
            j = k;
            if (c1 < c2 || (c1 < 3 && c1 != c2)) {
                return false;
            }
        }
        return i == m && j == n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int expressiveWords(string s, vector<string>& words) {
        auto check = [](string& s, string& t) -> int {
            int m = s.size(), n = t.size();
            if (n > m) return 0;
            int i = 0, j = 0;
            while (i < m && j < n) {
                if (s[i] != t[j]) return 0;
                int k = i;
                while (k < m && s[k] == s[i]) ++k;
                int c1 = k - i;
                i = k, k = j;
                while (k < n && t[k] == t[j]) ++k;
                int c2 = k - j;
                j = k;
                if (c1 < c2 || (c1 < 3 && c1 != c2)) return 0;
            }
            return i == m && j == n;
        };

        int ans = 0;
        for (string& t : words) ans += check(s, t);
        return ans;
    }
};
```

#### Go

```go
func expressiveWords(s string, words []string) (ans int) {
	check := func(s, t string) bool {
		m, n := len(s), len(t)
		if n > m {
			return false
		}
		i, j := 0, 0
		for i < m && j < n {
			if s[i] != t[j] {
				return false
			}
			k := i
			for k < m && s[k] == s[i] {
				k++
			}
			c1 := k - i
			i, k = k, j
			for k < n && t[k] == t[j] {
				k++
			}
			c2 := k - j
			j = k
			if c1 < c2 || (c1 != c2 && c1 < 3) {
				return false
			}
		}
		return i == m && j == n
	}
	for _, t := range words {
		if check(s, t) {
			ans++
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
