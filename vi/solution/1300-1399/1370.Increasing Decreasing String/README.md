---
comments: true
difficulty: Easy
rating: 1369
source: Biweekly Contest 21 Q1
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [1370. Increasing Decreasing String](https://leetcode.com/problems/increasing-decreasing-string)

[中文文档](/solution/1300-1399/1370.Increasing%20Decreasing%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho chuỗi <code>s</code>. Hãy sắp xếp lại chuỗi theo thuật toán sau:</p>

<ol>
	<li>Xóa ký tự <strong>nhỏ nhất</strong> trong <code>s</code> và <strong>thêm</strong> ký tự đó vào cuối kết quả.</li>
	<li>Xóa ký tự <strong>nhỏ nhất</strong> trong <code>s</code> lớn hơn ký tự vừa thêm cuối cùng, rồi <strong>thêm</strong> ký tự đó vào cuối kết quả.</li>
	<li>Lặp lại bước 2 cho đến khi không thể xóa thêm ký tự nào.</li>
	<li>Xóa ký tự <strong>lớn nhất</strong> trong <code>s</code> và <strong>thêm</strong> ký tự đó vào cuối kết quả.</li>
	<li>Xóa ký tự <strong>lớn nhất</strong> trong <code>s</code> nhỏ hơn ký tự vừa thêm cuối cùng, rồi <strong>thêm</strong> ký tự đó vào cuối kết quả.</li>
	<li>Lặp lại bước 5 cho đến khi không thể xóa thêm ký tự nào.</li>
	<li>Lặp lại các bước từ 1 đến 6 cho đến khi xóa hết ký tự trong <code>s</code>.</li>
</ol>

<p>Nếu ký tự nhỏ nhất hoặc lớn nhất xuất hiện nhiều lần, bạn có thể chọn bất kỳ lần xuất hiện nào để thêm vào kết quả.</p>

<p>Hãy trả về chuỗi thu được sau khi sắp xếp lại <code>s</code> theo thuật toán này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaaabbbbcccc&quot;
<strong>Đầu ra:</strong> &quot;abccbaabccba&quot;
<strong>Giải thích:</strong> Sau các bước 1, 2 và 3 của vòng lặp thứ nhất, kết quả = &quot;abc&quot;
Sau các bước 4, 5 và 6 của vòng lặp thứ nhất, kết quả = &quot;abccba&quot;
Vòng lặp thứ nhất hoàn tất. Lúc này s = &quot;aabbcc&quot; và ta quay lại bước 1.
Sau các bước 1, 2 và 3 của vòng lặp thứ hai, kết quả = &quot;abccbaabc&quot;
Sau các bước 4, 5 và 6 của vòng lặp thứ hai, kết quả = &quot;abccbaabccba&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;rat&quot;
<strong>Đầu ra:</strong> &quot;art&quot;
<strong>Giải thích:</strong> Từ &quot;rat&quot; trở thành &quot;art&quot; sau khi được sắp xếp lại theo thuật toán trên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 500</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Lần lượt lấy từng ký tự còn lại theo thứ tự từ $a$ đến $z$, rồi từ $z$ đến $a$. Vì bảng chữ cái có $26$ ký tự, ta đếm số lần xuất hiện trước, sau đó duyệt dãy $a\ldots z$ nối với $z\ldots a$, thêm một ký tự nếu số đếm của nó còn lớn hơn 0, cho đến khi đã thêm đủ $n$ ký tự.

<!-- thinking:end -->

Trước tiên, ta dùng hash table hoặc mảng $cnt$ có độ dài $26$ để đếm số lần xuất hiện của từng ký tự trong chuỗi $s$.

Sau đó, ta duyệt các chữ cái theo thứ tự $[a,...,z]$. Với chữ cái hiện tại $c$, nếu $cnt[c] > 0$, ta thêm $c$ vào cuối chuỗi kết quả và giảm $cnt[c]$ đi một. Lặp lại thao tác này cho đến khi $cnt[c] = 0$. Tiếp theo, duyệt các chữ cái theo thứ tự ngược lại $[z,...,a]$ và thực hiện tương tự. Nếu độ dài chuỗi kết quả bằng độ dài $s$, nghĩa là ta đã hoàn tất việc ghép chuỗi.

Độ phức tạp thời gian là $O(n \times |\Sigma|)$ và độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài chuỗi $s$ và $\Sigma$ là tập ký tự. Trong bài này, tập ký tự gồm toàn bộ chữ cái viết thường, nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortString(self, s: str) -> str:
        cnt = Counter(s)
        cs = ascii_lowercase + ascii_lowercase[::-1]
        ans = []
        while len(ans) < len(s):
            for c in cs:
                if cnt[c]:
                    ans.append(c)
                    cnt[c] -= 1
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String sortString(String s) {
        int[] cnt = new int[26];
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            cnt[s.charAt(i) - 'a']++;
        }
        StringBuilder sb = new StringBuilder();
        while (sb.length() < n) {
            for (int i = 0; i < 26; ++i) {
                if (cnt[i] > 0) {
                    sb.append((char) ('a' + i));
                    --cnt[i];
                }
            }
            for (int i = 25; i >= 0; --i) {
                if (cnt[i] > 0) {
                    sb.append((char) ('a' + i));
                    --cnt[i];
                }
            }
        }
        return sb.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string sortString(string s) {
        int cnt[26]{};
        for (char& c : s) {
            ++cnt[c - 'a'];
        }
        string ans;
        while (ans.size() < s.size()) {
            for (int i = 0; i < 26; ++i) {
                if (cnt[i]) {
                    ans += i + 'a';
                    --cnt[i];
                }
            }
            for (int i = 25; i >= 0; --i) {
                if (cnt[i]) {
                    ans += i + 'a';
                    --cnt[i];
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sortString(s string) string {
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	n := len(s)
	ans := make([]byte, 0, n)
	for len(ans) < n {
		for i := 0; i < 26; i++ {
			if cnt[i] > 0 {
				ans = append(ans, byte(i)+'a')
				cnt[i]--
			}
		}
		for i := 25; i >= 0; i-- {
			if cnt[i] > 0 {
				ans = append(ans, byte(i)+'a')
				cnt[i]--
			}
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function sortString(s: string): string {
    const cnt: number[] = Array(26).fill(0);
    for (const c of s) {
        ++cnt[c.charCodeAt(0) - 'a'.charCodeAt(0)];
    }
    const ans: string[] = [];
    while (ans.length < s.length) {
        for (let i = 0; i < 26; ++i) {
            if (cnt[i]) {
                ans.push(String.fromCharCode(i + 'a'.charCodeAt(0)));
                --cnt[i];
            }
        }
        for (let i = 25; i >= 0; --i) {
            if (cnt[i]) {
                ans.push(String.fromCharCode(i + 'a'.charCodeAt(0)));
                --cnt[i];
            }
        }
    }
    return ans.join('');
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {string}
 */
var sortString = function (s) {
    const cnt = Array(26).fill(0);
    for (const c of s) {
        ++cnt[c.charCodeAt(0) - 'a'.charCodeAt(0)];
    }
    const ans = [];
    while (ans.length < s.length) {
        for (let i = 0; i < 26; ++i) {
            if (cnt[i]) {
                ans.push(String.fromCharCode(i + 'a'.charCodeAt(0)));
                --cnt[i];
            }
        }
        for (let i = 25; i >= 0; --i) {
            if (cnt[i]) {
                ans.push(String.fromCharCode(i + 'a'.charCodeAt(0)));
                --cnt[i];
            }
        }
    }
    return ans.join('');
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
