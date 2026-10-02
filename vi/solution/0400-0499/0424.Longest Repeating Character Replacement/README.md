---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement)

[中文文档](/solution/0400-0499/0424.Longest%20Repeating%20Character%20Replacement/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và số nguyên <code>k</code>. Bạn có thể chọn bất kỳ ký tự nào trong chuỗi và đổi thành một chữ cái tiếng Anh viết hoa khác. Bạn được thực hiện thao tác này nhiều nhất <code>k</code> lần.</p>

<p>Hãy trả về <em>độ dài của chuỗi con dài nhất gồm cùng một chữ cái mà bạn có thể tạo được sau khi thực hiện các thao tác trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ABAB&quot;, k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Thay hai ký tự &#39;A&#39; bằng hai ký tự &#39;B&#39;, hoặc ngược lại.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;AABABBA&quot;, k = 1
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Thay ký tự &#39;A&#39; ở giữa bằng &#39;B&#39; để tạo thành &quot;AABBBBA&quot;.
Chuỗi con &quot;BBBB&quot; có độ dài lớn nhất gồm các chữ cái lặp lại, bằng 4.
Cũng có thể có những cách khác để đạt được kết quả này.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết hoa.</li>
	<li><code>0 &lt;= k &lt;= s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Có thể biến một window thành chuỗi chỉ gồm một ký tự bằng nhiều nhất $k$ lần thay đổi khi và chỉ khi độ dài window trừ số lần xuất hiện của ký tự phổ biến nhất không vượt quá $k$. Thử mọi cặp đầu mút sẽ tốn $O(n^2)$.
>
> Mở rộng đầu phải và theo dõi tần suất lớn nhất $\textit{mx}$. Khi $r-l+1-\textit{mx}>k$, dịch đầu trái thêm một vị trí. Window hợp lệ dài nhất có độ dài $n-l$.
>
> Không cần giảm $\textit{mx}$ khi dịch đầu trái: ta chỉ cần tìm độ dài lớn nhất, và việc ước lượng $\textit{mx}$ cao hơn thực tế chỉ khiến điều kiện kiểm tra chặt hơn.

<!-- thinking:end -->

Dùng hash table `cnt` để đếm số lần xuất hiện của từng ký tự trong chuỗi, cùng hai con trỏ `l` và `r` để duy trì sliding window sao cho độ dài window trừ số lần xuất hiện của ký tự phổ biến nhất không vượt quá $k$.

Duyệt chuỗi, mỗi lần cập nhật đầu phải `r` của window, số lần xuất hiện của các ký tự trong window và tần suất lớn nhất `mx` trong số đó. Khi độ dài window trừ `mx` lớn hơn $k$, thu hẹp đầu trái `l` và cập nhật lại số lần xuất hiện trong window cho đến khi độ dài window trừ `mx` không còn lớn hơn $k$.

Cuối cùng, đáp án là $n - l$, trong đó $n$ là độ dài chuỗi.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài chuỗi còn $|\Sigma|$ là kích thước của bảng ký tự. Trong bài này, bảng ký tự gồm các chữ cái tiếng Anh viết hoa nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        cnt = Counter()
        l = mx = 0
        for r, c in enumerate(s):
            cnt[c] += 1
            mx = max(mx, cnt[c])
            if r - l + 1 - mx > k:
                cnt[s[l]] -= 1
                l += 1
        return len(s) - l
```

#### Java

```java
class Solution {
    public int characterReplacement(String s, int k) {
        int[] cnt = new int[26];
        int l = 0, mx = 0;
        int n = s.length();
        for (int r = 0; r < n; ++r) {
            mx = Math.max(mx, ++cnt[s.charAt(r) - 'A']);
            if (r - l + 1 - mx > k) {
                --cnt[s.charAt(l++) - 'A'];
            }
        }
        return n - l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int characterReplacement(string s, int k) {
        int cnt[26]{};
        int l = 0, mx = 0;
        int n = s.length();
        for (int r = 0; r < n; ++r) {
            mx = max(mx, ++cnt[s[r] - 'A']);
            if (r - l + 1 - mx > k) {
                --cnt[s[l++] - 'A'];
            }
        }
        return n - l;
    }
};
```

#### Go

```go
func characterReplacement(s string, k int) int {
	cnt := [26]int{}
	l, mx := 0, 0
	for r, c := range s {
		cnt[c-'A']++
		mx = max(mx, cnt[c-'A'])
		if r-l+1-mx > k {
			cnt[s[l]-'A']--
			l++
		}
	}
	return len(s) - l
}
```

#### TypeScript

```ts
function characterReplacement(s: string, k: number): number {
    const idx = (c: string) => c.charCodeAt(0) - 65;
    const cnt: number[] = Array(26).fill(0);
    const n = s.length;
    let [l, mx] = [0, 0];
    for (let r = 0; r < n; ++r) {
        mx = Math.max(mx, ++cnt[idx(s[r])]);
        if (r - l + 1 - mx > k) {
            --cnt[idx(s[l++])];
        }
    }
    return n - l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
