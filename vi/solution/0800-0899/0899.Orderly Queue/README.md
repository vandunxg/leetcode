---
comments: true
difficulty: Hard
tags:
    - Math
    - String
    - Sorting
    - Smallest Representation
---

<!-- problem:start -->

# [899. Orderly Queue](https://leetcode.com/problems/orderly-queue)

[中文文档](/solution/0800-0899/0899.Orderly%20Queue/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và số nguyên <code>k</code>. Bạn có thể chọn một trong <code>k</code> ký tự đầu tiên của <code>s</code> rồi chuyển ký tự đó xuống cuối chuỗi.</p>

<p>Trả về <em>chuỗi nhỏ nhất theo thứ tự từ điển có thể thu được sau khi thực hiện thao tác trên bất kỳ số lần nào</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;cba&quot;, k = 1
<strong>Đầu ra:</strong> &quot;acb&quot;
<strong>Giải thích:</strong> 
Ở lượt đầu tiên, ta chuyển ký tự thứ 1 &#39;c&#39; xuống cuối, được chuỗi &quot;bac&quot;.
Ở lượt thứ hai, ta chuyển ký tự thứ 1 &#39;b&#39; xuống cuối, thu được kết quả &quot;acb&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;baaca&quot;, k = 3
<strong>Đầu ra:</strong> &quot;aaabc&quot;
<strong>Giải thích:</strong> 
Ở lượt đầu tiên, ta chuyển ký tự thứ 1 &#39;b&#39; xuống cuối, được chuỗi &quot;aacab&quot;.
Ở lượt thứ hai, ta chuyển ký tự thứ 3 &#39;c&#39; xuống cuối, thu được kết quả &quot;aaabc&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Xét từng trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Ta chuyển một trong $k$ ký tự đầu tiên xuống cuối và cần tìm chuỗi nhỏ nhất theo thứ tự từ điển. Khi $k=1$, thao tác chỉ tạo phép xoay chuỗi, nên ta thử mọi phép xoay.
>
> Khi $k\ge 2$, ta có thể hoán đổi hai ký tự liền kề, nên có thể tạo ra mọi hoán vị; đáp án là chuỗi đã sắp xếp.

<!-- thinking:end -->

Nếu $k = 1$, mỗi lần ta chỉ có thể chuyển ký tự đầu tiên xuống cuối chuỗi, tạo ra $|s|$ trạng thái khác nhau. Ta trả về chuỗi nhỏ nhất theo thứ tự từ điển.

Nếu $k > 1$, xét chuỗi có dạng $abc[xy]def$. Ta lần lượt chuyển $a$, $b$ và $c$ xuống cuối, thu được $[xy]defabc$. Sau đó, chuyển $y$ rồi $x$ xuống cuối, thu được $defabc[yx]$. Cuối cùng, chuyển $d$, $e$ và $f$ xuống cuối, thu được $abc[yx]def$. Như vậy, ta đã hoán đổi vị trí của $x$ và $y$.

Vì vậy, khi $k > 1$, ta có thể hoán đổi hai ký tự bất kỳ liền kề trong chuỗi, từ đó sắp xếp chuỗi theo thứ tự tăng dần.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def orderlyQueue(self, s: str, k: int) -> str:
        if k == 1:
            ans = s
            for _ in range(len(s) - 1):
                s = s[1:] + s[0]
                ans = min(ans, s)
            return ans
        return "".join(sorted(s))
```

#### Java

```java
class Solution {
    public String orderlyQueue(String s, int k) {
        if (k == 1) {
            String ans = s;
            StringBuilder sb = new StringBuilder(s);
            for (int i = 0; i < s.length() - 1; ++i) {
                sb.append(sb.charAt(0)).deleteCharAt(0);
                if (sb.toString().compareTo(ans) < 0) {
                    ans = sb.toString();
                }
            }
            return ans;
        }
        char[] cs = s.toCharArray();
        Arrays.sort(cs);
        return String.valueOf(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string orderlyQueue(string s, int k) {
        if (k == 1) {
            string ans = s;
            for (int i = 0; i < s.size() - 1; ++i) {
                s = s.substr(1) + s[0];
                if (s < ans) ans = s;
            }
            return ans;
        }
        sort(s.begin(), s.end());
        return s;
    }
};
```

#### Go

```go
func orderlyQueue(s string, k int) string {
	if k == 1 {
		ans := s
		for i := 0; i < len(s)-1; i++ {
			s = s[1:] + s[:1]
			if s < ans {
				ans = s
			}
		}
		return ans
	}
	t := []byte(s)
	sort.Slice(t, func(i, j int) bool { return t[i] < t[j] })
	return string(t)
}
```

#### TypeScript

```ts
function orderlyQueue(s: string, k: number): string {
    if (k > 1) {
        return [...s].sort().join('');
    }
    const n = s.length;
    let min = s;
    for (let i = 1; i < n; i++) {
        const t = s.slice(i) + s.slice(0, i);
        if (t < min) {
            min = t;
        }
    }
    return min;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
