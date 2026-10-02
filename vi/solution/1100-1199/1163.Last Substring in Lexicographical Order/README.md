---
comments: true
difficulty: Hard
rating: 1864
source: Weekly Contest 150 Q4
tags:
    - Two Pointers
    - String
    - Lyndon Factorization
---

<!-- problem:start -->

# [1163. Last Substring in Lexicographical Order](https://leetcode.com/problems/last-substring-in-lexicographical-order)

[中文文档](/solution/1100-1199/1163.Last%20Substring%20in%20Lexicographical%20Order/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy trả về <em>chuỗi con đứng cuối cùng của</em> <code>s</code> <em>theo thứ tự từ điển</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abab&quot;
<strong>Đầu ra:</strong> &quot;bab&quot;
<strong>Giải thích:</strong> Các chuỗi con là [&quot;a&quot;, &quot;ab&quot;, &quot;aba&quot;, &quot;abab&quot;, &quot;b&quot;, &quot;ba&quot;, &quot;bab&quot;]. Chuỗi con lớn nhất theo thứ tự từ điển là &quot;bab&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leetcode&quot;
<strong>Đầu ra:</strong> &quot;tcode&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 4 * 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two pointers

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi con đứng cuối theo thứ tự từ điển chính là một suffix; so sánh mọi suffix sẽ có độ phức tạp bậc hai. Hai pointer $i$ và $j$ lần lượt đánh dấu suffix tốt nhất hiện tại và một ứng viên, còn $k$ dùng để so sánh ký tự: nếu bằng nhau thì tăng $k$; nếu ứng viên tốt hơn thì đưa $i$ vượt qua đoạn vừa so sánh; nếu kém hơn thì đưa $j$ tiến lên. Các vị trí bắt đầu bị bỏ qua không thể là đáp án, nên có thể tìm suffix tốt nhất trong thời gian tuyến tính.

<!-- thinking:end -->

Ta nhận thấy nếu một chuỗi con bắt đầu tại vị trí $i$, thì chuỗi con lớn nhất theo thứ tự từ điển bắt đầu từ đó phải là $s[i,..n-1]$, tức suffix dài nhất bắt đầu tại vị trí $i$. Vì vậy, ta chỉ cần tìm suffix lớn nhất.

Ta dùng hai pointer $i$ và $j$. Pointer $i$ trỏ đến vị trí bắt đầu của chuỗi con lớn nhất theo thứ tự từ điển hiện tại, còn pointer $j$ trỏ đến vị trí bắt đầu của chuỗi con đang xét. Ngoài ra, biến $k$ lưu vị trí hiện tại đang được so sánh. Ban đầu, $i = 0$, $j=1$, $k=0$.

Mỗi lần, ta so sánh $s[i+k]$ và $s[j+k]$:

Nếu $s[i + k] = s[j + k]$, nghĩa là $s[i,..i+k]$ và $s[j,..j+k]$ giống nhau. Ta tăng $k$ thêm $1$ rồi tiếp tục so sánh $s[i+k]$ và $s[j+k]$;

Nếu $s[i + k] \lt s[j + k]$, thứ tự từ điển của $s[j,..j+k]$ lớn hơn. Khi đó, ta cập nhật $i = i + k + 1$ và đặt lại $k$ về $0$. Nếu lúc này $i \geq j$, ta cập nhật pointer $j$ thành $i + 1$, tức $j = i + 1$. Ta bỏ qua mọi suffix bắt đầu tại các vị trí từ $i$ đến $i+k$, vì chúng có thứ tự từ điển nhỏ hơn các suffix bắt đầu từ $j$ đến $j+k$.

Tương tự, nếu $s[i + k] \gt s[j + k]$, thứ tự từ điển của $s[i,..,i+k]$ lớn hơn. Khi đó, ta cập nhật $j = j + k + 1$ và đặt lại $k$ về $0$. Ta bỏ qua mọi suffix bắt đầu tại các vị trí từ $j$ đến $j+k$, vì chúng có thứ tự từ điển nhỏ hơn các suffix bắt đầu từ $i$ đến $i+k$.

Cuối cùng, ta trả về suffix bắt đầu tại $i$, tức $s[i,..,n-1]$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lastSubstring(self, s: str) -> str:
        i, j, k = 0, 1, 0
        while j + k < len(s):
            if s[i + k] == s[j + k]:
                k += 1
            elif s[i + k] < s[j + k]:
                i += k + 1
                k = 0
                if i >= j:
                    j = i + 1
            else:
                j += k + 1
                k = 0
        return s[i:]
```

#### Java

```java
class Solution {
    public String lastSubstring(String s) {
        int n = s.length();
        int i = 0;
        for (int j = 1, k = 0; j + k < n;) {
            int d = s.charAt(i + k) - s.charAt(j + k);
            if (d == 0) {
                ++k;
            } else if (d < 0) {
                i += k + 1;
                k = 0;
                if (i >= j) {
                    j = i + 1;
                }
            } else {
                j += k + 1;
                k = 0;
            }
        }
        return s.substring(i);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string lastSubstring(string s) {
        int n = s.size();
        int i = 0;
        for (int j = 1, k = 0; j + k < n;) {
            if (s[i + k] == s[j + k]) {
                ++k;
            } else if (s[i + k] < s[j + k]) {
                i += k + 1;
                k = 0;
                if (i >= j) {
                    j = i + 1;
                }
            } else {
                j += k + 1;
                k = 0;
            }
        }
        return s.substr(i);
    }
};
```

#### Go

```go
func lastSubstring(s string) string {
	i, n := 0, len(s)
	for j, k := 1, 0; j+k < n; {
		if s[i+k] == s[j+k] {
			k++
		} else if s[i+k] < s[j+k] {
			i += k + 1
			k = 0
			if i >= j {
				j = i + 1
			}
		} else {
			j += k + 1
			k = 0
		}
	}
	return s[i:]
}
```

#### TypeScript

```ts
function lastSubstring(s: string): string {
    const n = s.length;
    let i = 0;
    for (let j = 1, k = 0; j + k < n;) {
        if (s[i + k] === s[j + k]) {
            ++k;
        } else if (s[i + k] < s[j + k]) {
            i += k + 1;
            k = 0;
            if (i >= j) {
                j = i + 1;
            }
        } else {
            j += k + 1;
            k = 0;
        }
    }
    return s.slice(i);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
