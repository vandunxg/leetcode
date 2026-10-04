---
comments: true
difficulty: Hard
rating: 2415
source: Weekly Contest 343 Q4
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [2663. Lexicographically Smallest Beautiful String](https://leetcode.com/problems/lexicographically-smallest-beautiful-string)

[中文文档](/solution/2600-2699/2663.Lexicographically%20Smallest%20Beautiful%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi được gọi là <strong>đẹp</strong> nếu:</p>

<ul>
	<li>Chuỗi chỉ gồm <code>k</code> ký tự đầu tiên trong bảng chữ cái tiếng Anh viết thường.</li>
	<li>Chuỗi không chứa chuỗi con đối xứng nào có độ dài từ <code>2</code> trở lên.</li>
</ul>

<p>Cho một chuỗi đẹp <code>s</code> có độ dài <code>n</code> và một số nguyên dương <code>k</code>.</p>

<p>Trả về <em>chuỗi nhỏ nhất theo thứ tự từ điển, có độ dài </em><code>n</code><em>, lớn hơn </em><code>s</code><em> và là một chuỗi <strong>đẹp</strong></em>. Nếu không tồn tại chuỗi như vậy, trả về chuỗi rỗng.</p>

<p>Một chuỗi <code>a</code> lớn hơn chuỗi <code>b</code> theo thứ tự từ điển (với hai chuỗi có cùng độ dài) nếu tại vị trí đầu tiên mà <code>a</code> và <code>b</code> khác nhau, ký tự của <code>a</code> lớn hơn ký tự tương ứng của <code>b</code>.</p>

<ul>
	<li>Ví dụ, <code>&quot;abcd&quot;</code> lớn hơn <code>&quot;abcc&quot;</code> theo thứ tự từ điển vì vị trí đầu tiên chúng khác nhau là ký tự thứ tư, và <code>d</code> lớn hơn <code>c</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcz&quot;, k = 26
<strong>Đầu ra:</strong> &quot;abda&quot;
<strong>Giải thích:</strong> Chuỗi &quot;abda&quot; là chuỗi đẹp và lớn hơn chuỗi &quot;abcz&quot; theo thứ tự từ điển.
Có thể chứng minh rằng không tồn tại chuỗi nào lớn hơn chuỗi &quot;abcz&quot;, đẹp và nhỏ hơn chuỗi &quot;abda&quot; theo thứ tự từ điển.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;dc&quot;, k = 4
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> Có thể chứng minh rằng không tồn tại chuỗi nào lớn hơn chuỗi &quot;dc&quot; theo thứ tự từ điển mà vẫn là chuỗi đẹp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>4 &lt;= k &lt;= 26</code></li>
	<li><code>s</code> là một chuỗi đẹp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi đẹp không có palindrome có độ dài $\ge 2$, nên mỗi ký tự phải khác hai ký tự đứng trước nó. Ta cần tìm chuỗi đẹp tiếp theo lớn hơn chuỗi hiện tại. Việc thay đổi một vị trí bên trái sẽ buộc phải xử lý lại hậu tố; vì vậy, chuỗi kế tiếp nhỏ nhất theo thứ tự từ điển nên thay đổi vị trí càng về bên phải càng tốt.
>
> Duyệt từ phải sang trái để tìm vị trí đầu tiên có thể tăng lên một ký tự lớn hơn mà vẫn hợp lệ, sau đó điền hậu tố bằng các ký tự hợp lệ nhỏ nhất. Nếu không có vị trí nào có thể tăng, đáp án không tồn tại. Vì $k \le 26$, mỗi vị trí chỉ cần thử một số lượng hằng số ký tự, phù hợp với $n \le 10^5$.

<!-- thinking:end -->

Ta nhận thấy một chuỗi đối xứng có độ dài $2$ phải có hai ký tự kề nhau bằng nhau, còn chuỗi đối xứng có độ dài $3$ phải có ký tự đầu và cuối bằng nhau. Do đó, một chuỗi đẹp không chứa chuỗi con đối xứng có độ dài từ $2$ trở lên, nghĩa là mỗi ký tự phải khác hai ký tự liền trước nó.

Ta có thể tham lam duyệt ngược từ vị trí cuối cùng của chuỗi, tìm một vị trí $i$ sao cho ký tự tại vị trí $i$ có thể được thay bằng một ký tự lớn hơn một chút, đồng thời vẫn khác hai ký tự liền trước nó.

- Nếu tìm thấy vị trí $i$, ta thay $s[i]$ bằng $c$, rồi thay các ký tự từ $s[i+1]$ đến $s[n-1]$ bằng các ký tự đầu tiên trong bảng chữ cái gồm $k$ ký tự, theo thứ tự từ điển nhỏ nhất, sao cho không trùng với hai ký tự liền trước. Sau khi thay xong, ta thu được chuỗi đẹp nhỏ nhất theo thứ tự từ điển và lớn hơn $s$.
- Nếu không tìm thấy vị trí $i$, ta không thể tạo ra chuỗi đẹp lớn hơn $s$ theo thứ tự từ điển, nên trả về chuỗi rỗng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestBeautifulString(self, s: str, k: int) -> str:
        n = len(s)
        cs = list(s)
        for i in range(n - 1, -1, -1):
            p = ord(cs[i]) - ord('a') + 1
            for j in range(p, k):
                c = chr(ord('a') + j)
                if (i > 0 and cs[i - 1] == c) or (i > 1 and cs[i - 2] == c):
                    continue
                cs[i] = c
                for l in range(i + 1, n):
                    for m in range(k):
                        c = chr(ord('a') + m)
                        if (l > 0 and cs[l - 1] == c) or (l > 1 and cs[l - 2] == c):
                            continue
                        cs[l] = c
                        break
                return ''.join(cs)
        return ''
```

#### Java

```java
class Solution {
    public String smallestBeautifulString(String s, int k) {
        int n = s.length();
        char[] cs = s.toCharArray();
        for (int i = n - 1; i >= 0; --i) {
            int p = cs[i] - 'a' + 1;
            for (int j = p; j < k; ++j) {
                char c = (char) ('a' + j);
                if ((i > 0 && cs[i - 1] == c) || (i > 1 && cs[i - 2] == c)) {
                    continue;
                }
                cs[i] = c;
                for (int l = i + 1; l < n; ++l) {
                    for (int m = 0; m < k; ++m) {
                        c = (char) ('a' + m);
                        if ((l > 0 && cs[l - 1] == c) || (l > 1 && cs[l - 2] == c)) {
                            continue;
                        }
                        cs[l] = c;
                        break;
                    }
                }
                return String.valueOf(cs);
            }
        }
        return "";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string smallestBeautifulString(string s, int k) {
        int n = s.size();
        for (int i = n - 1; i >= 0; --i) {
            int p = s[i] - 'a' + 1;
            for (int j = p; j < k; ++j) {
                char c = (char) ('a' + j);
                if ((i > 0 && s[i - 1] == c) || (i > 1 && s[i - 2] == c)) {
                    continue;
                }
                s[i] = c;
                for (int l = i + 1; l < n; ++l) {
                    for (int m = 0; m < k; ++m) {
                        c = (char) ('a' + m);
                        if ((l > 0 && s[l - 1] == c) || (l > 1 && s[l - 2] == c)) {
                            continue;
                        }
                        s[l] = c;
                        break;
                    }
                }
                return s;
            }
        }
        return "";
    }
};
```

#### Go

```go
func smallestBeautifulString(s string, k int) string {
	cs := []byte(s)
	n := len(cs)
	for i := n - 1; i >= 0; i-- {
		p := int(cs[i] - 'a' + 1)
		for j := p; j < k; j++ {
			c := byte('a' + j)
			if (i > 0 && cs[i-1] == c) || (i > 1 && cs[i-2] == c) {
				continue
			}
			cs[i] = c
			for l := i + 1; l < n; l++ {
				for m := 0; m < k; m++ {
					c = byte('a' + m)
					if (l > 0 && cs[l-1] == c) || (l > 1 && cs[l-2] == c) {
						continue
					}
					cs[l] = c
					break
				}
			}
			return string(cs)
		}
	}
	return ""
}
```

#### TypeScript

```ts
function smallestBeautifulString(s: string, k: number): string {
    const cs: string[] = s.split('');
    const n = cs.length;
    for (let i = n - 1; i >= 0; --i) {
        const p = cs[i].charCodeAt(0) - 97 + 1;
        for (let j = p; j < k; ++j) {
            let c = String.fromCharCode(j + 97);
            if ((i > 0 && cs[i - 1] === c) || (i > 1 && cs[i - 2] === c)) {
                continue;
            }
            cs[i] = c;
            for (let l = i + 1; l < n; ++l) {
                for (let m = 0; m < k; ++m) {
                    c = String.fromCharCode(m + 97);
                    if ((l > 0 && cs[l - 1] === c) || (l > 1 && cs[l - 2] === c)) {
                        continue;
                    }
                    cs[l] = c;
                    break;
                }
            }
            return cs.join('');
        }
    }
    return '';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
