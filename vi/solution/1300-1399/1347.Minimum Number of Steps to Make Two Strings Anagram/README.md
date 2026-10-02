---
comments: true
difficulty: Medium
rating: 1330
source: Weekly Contest 175 Q2
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [1347. Minimum Number of Steps to Make Two Strings Anagram](https://leetcode.com/problems/minimum-number-of-steps-to-make-two-strings-anagram)

[中文文档](/solution/1300-1399/1347.Minimum%20Number%20of%20Steps%20to%20Make%20Two%20Strings%20Anagram/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <code>s</code> và <code>t</code> có cùng độ dài. Trong một bước, bạn có thể chọn <strong>bất kỳ ký tự nào</strong> trong <code>t</code> và thay bằng <strong>một ký tự khác</strong>.</p>

<p>Trả về <em>số bước tối thiểu</em> để biến <code>t</code> thành một anagram của <code>s</code>.</p>

<p><strong>Anagram</strong> của một chuỗi là chuỗi chứa cùng các ký tự, nhưng theo thứ tự khác (hoặc giữ nguyên thứ tự).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bab&quot;, t = &quot;aba&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Thay chữ &#39;a&#39; đầu tiên trong t bằng b, ta được t = &quot;bba&quot;, là anagram của s.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leetcode&quot;, t = &quot;practice&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Thay các ký tự &#39;p&#39;, &#39;r&#39;, &#39;a&#39;, &#39;i&#39; và &#39;c&#39; trong t bằng các ký tự phù hợp để biến t thành anagram của s.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;anagram&quot;, t = &quot;mangaar&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> &quot;anagram&quot; và &quot;mangaar&quot; là hai anagram. 
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>s.length == t.length</code></li>
	<li><code>s</code> và <code>t</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Biến chuỗi $t$ có cùng độ dài thành anagram của $s$ bằng cách thay ký tự. Số lần thay bằng số ký tự dư trong $t$ so với $s$. Đếm ký tự trong $s$, rồi duyệt $t$: nếu bộ đếm giảm xuống dưới 0, nghĩa là ký tự đó dư trong $t$ và cần thay một lần.

<!-- thinking:end -->

Ta có thể dùng hash table hoặc mảng $\textit{cnt}$ để đếm số lần xuất hiện của mỗi ký tự trong chuỗi $\textit{s}$. Sau đó, duyệt chuỗi $\textit{t}$. Với mỗi ký tự, giảm bộ đếm tương ứng trong $\textit{cnt}$ đi một. Nếu giá trị sau khi giảm nhỏ hơn $0$, nghĩa là ký tự này xuất hiện trong $\textit{t}$ nhiều hơn trong $\textit{s}$. Khi đó, ta cần thay ký tự này và tăng đáp án thêm một.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(m + n)$ và độ phức tạp không gian là $O(|\Sigma|)$, trong đó $m$ và $n$ lần lượt là độ dài của chuỗi $\textit{s}$ và $\textit{t}$, còn $|\Sigma|$ là kích thước bảng ký tự. Trong bài này, bảng ký tự gồm các chữ cái viết thường nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSteps(self, s: str, t: str) -> int:
        cnt = Counter(s)
        ans = 0
        for c in t:
            cnt[c] -= 1
            ans += cnt[c] < 0
        return ans
```

#### Java

```java
class Solution {
    public int minSteps(String s, String t) {
        int[] cnt = new int[26];
        for (char c : s.toCharArray()) {
            cnt[c - 'a']++;
        }
        int ans = 0;
        for (char c : t.toCharArray()) {
            if (--cnt[c - 'a'] < 0) {
                ans++;
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
    int minSteps(string s, string t) {
        int cnt[26]{};
        for (char c : s) {
            ++cnt[c - 'a'];
        }
        int ans = 0;
        for (char c : t) {
            if (--cnt[c - 'a'] < 0) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minSteps(s string, t string) (ans int) {
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	for _, c := range t {
		cnt[c-'a']--
		if cnt[c-'a'] < 0 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function minSteps(s: string, t: string): number {
    const cnt: number[] = Array(26).fill(0);
    for (const c of s) {
        ++cnt[c.charCodeAt(0) - 97];
    }
    let ans = 0;
    for (const c of t) {
        if (--cnt[c.charCodeAt(0) - 97] < 0) {
            ++ans;
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @param {string} t
 * @return {number}
 */
var minSteps = function (s, t) {
    const cnt = Array(26).fill(0);
    for (const c of s) {
        ++cnt[c.charCodeAt(0) - 97];
    }
    let ans = 0;
    for (const c of t) {
        if (--cnt[c.charCodeAt(0) - 97] < 0) {
            ++ans;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
