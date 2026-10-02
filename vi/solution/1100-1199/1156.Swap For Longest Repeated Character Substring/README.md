---
comments: true
difficulty: Medium
rating: 1787
source: Weekly Contest 149 Q3
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [1156. Swap For Longest Repeated Character Substring](https://leetcode.com/problems/swap-for-longest-repeated-character-substring)

[中文文档](/solution/1100-1199/1156.Swap%20For%20Longest%20Repeated%20Character%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>text</code>. Bạn có thể hoán đổi hai ký tự bất kỳ trong <code>text</code>.</p>

<p>Trả về <em>độ dài của chuỗi con dài nhất gồm các ký tự lặp lại</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;ababa&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể đổi ký tự &#39;b&#39; đầu tiên với ký tự &#39;a&#39; cuối cùng, hoặc đổi ký tự &#39;b&#39; cuối cùng với ký tự &#39;a&#39; đầu tiên. Khi đó, chuỗi con dài nhất gồm các ký tự lặp lại là &quot;aaa&quot;, có độ dài 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;aaabaaa&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Đổi &#39;b&#39; với ký tự &#39;a&#39; cuối cùng (hoặc đầu tiên), ta được chuỗi con dài nhất gồm các ký tự lặp lại là &quot;aaaaaa&quot;, có độ dài 6.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;aaaaa&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Không cần hoán đổi; chuỗi con dài nhất gồm các ký tự lặp lại là &quot;aaaaa&quot;, có độ dài 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= text.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>text</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Với một lần hoán đổi, đoạn ký tự giống nhau dài nhất có thể là một đoạn được nối dài bằng một ký tự giống nó ở xa, hoặc hai đoạn ký tự giống nhau được ngăn cách bởi đúng một ký tự khác. Dùng hai con trỏ để lấy độ dài đoạn đầu $l$ và đoạn $r$ sau khi bỏ qua một ký tự; độ dài ứng viên là $\min(l+r+1,\textit{global count of that character})$, nhằm bảo đảm ta không tạo thêm ký tự ngoài số lượng vốn có.

<!-- thinking:end -->

Trước tiên, dùng hash table hoặc mảng $cnt$ để đếm số lần xuất hiện của từng ký tự trong chuỗi $text$.

Tiếp theo, đặt con trỏ $i$ ban đầu bằng $0$. Ở mỗi lượt, đặt con trỏ $j$ bằng $i$ rồi liên tục dịch $j$ sang phải cho đến khi ký tự tại $j$ khác ký tự tại $i$. Khi đó, ta có chuỗi con $text[i..j-1]$ dài $l = j - i$, trong đó mọi ký tự đều giống nhau.

Sau đó, bỏ qua ký tự tại con trỏ $j$, rồi tiếp tục dịch con trỏ $k$ sang phải cho đến khi ký tự tại $k$ khác ký tự tại $i$. Khi đó, ta có chuỗi con $text[j+1..k-1]$ dài $r = k - j - 1$, trong đó mọi ký tự đều giống nhau. Vì vậy, độ dài lớn nhất của chuỗi con chỉ gồm một loại ký tự mà ta có thể tạo được với tối đa một lần hoán đổi là $\min(l + r + 1, cnt[text[i]])$. Tiếp theo, chuyển $i$ đến $j$ và tìm chuỗi con kế tiếp. Lấy độ dài lớn nhất trong số các chuỗi con thỏa điều kiện.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(C)$, trong đó $n$ là độ dài chuỗi và $C$ là kích thước bảng chữ cái. Trong bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxRepOpt1(self, text: str) -> int:
        cnt = Counter(text)
        n = len(text)
        ans = i = 0
        while i < n:
            j = i
            while j < n and text[j] == text[i]:
                j += 1
            l = j - i
            k = j + 1
            while k < n and text[k] == text[i]:
                k += 1
            r = k - j - 1
            ans = max(ans, min(l + r + 1, cnt[text[i]]))
            i = j
        return ans
```

#### Java

```java
class Solution {
    public int maxRepOpt1(String text) {
        int[] cnt = new int[26];
        int n = text.length();
        for (int i = 0; i < n; ++i) {
            ++cnt[text.charAt(i) - 'a'];
        }
        int ans = 0, i = 0;
        while (i < n) {
            int j = i;
            while (j < n && text.charAt(j) == text.charAt(i)) {
                ++j;
            }
            int l = j - i;
            int k = j + 1;
            while (k < n && text.charAt(k) == text.charAt(i)) {
                ++k;
            }
            int r = k - j - 1;
            ans = Math.max(ans, Math.min(l + r + 1, cnt[text.charAt(i) - 'a']));
            i = j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxRepOpt1(string text) {
        int cnt[26] = {0};
        for (char& c : text) {
            ++cnt[c - 'a'];
        }
        int n = text.size();
        int ans = 0, i = 0;
        while (i < n) {
            int j = i;
            while (j < n && text[j] == text[i]) {
                ++j;
            }
            int l = j - i;
            int k = j + 1;
            while (k < n && text[k] == text[i]) {
                ++k;
            }
            int r = k - j - 1;
            ans = max(ans, min(l + r + 1, cnt[text[i] - 'a']));
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
func maxRepOpt1(text string) (ans int) {
	cnt := [26]int{}
	for _, c := range text {
		cnt[c-'a']++
	}
	n := len(text)
	for i, j := 0, 0; i < n; i = j {
		j = i
		for j < n && text[j] == text[i] {
			j++
		}
		l := j - i
		k := j + 1
		for k < n && text[k] == text[i] {
			k++
		}
		r := k - j - 1
		ans = max(ans, min(l+r+1, cnt[text[i]-'a']))
	}
	return
}
```

#### TypeScript

```ts
function maxRepOpt1(text: string): number {
    const idx = (c: string) => c.charCodeAt(0) - 'a'.charCodeAt(0);
    const cnt: number[] = new Array(26).fill(0);
    for (const c of text) {
        cnt[idx(c)]++;
    }
    let ans = 0;
    let i = 0;
    const n = text.length;
    while (i < n) {
        let j = i;
        while (j < n && text[j] === text[i]) {
            ++j;
        }
        const l = j - i;
        let k = j + 1;
        while (k < n && text[k] === text[i]) {
            ++k;
        }
        const r = k - j - 1;
        ans = Math.max(ans, Math.min(cnt[idx(text[i])], l + r + 1));
        i = j;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
