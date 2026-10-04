---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [2743. Count Substrings Without Repeating Character 🔒](https://leetcode.com/problems/count-substrings-without-repeating-character)

[中文文档](/solution/2700-2799/2743.Count%20Substrings%20Without%20Repeating%20Character/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường. Ta gọi một substring là <b>đặc biệt</b> nếu nó không chứa ký tự nào đã xuất hiện ít nhất hai lần (nói cách khác, nó không chứa ký tự lặp lại). Nhiệm vụ của bạn là đếm số substring <b>đặc biệt</b>. Ví dụ, trong chuỗi <code>&quot;pop&quot;</code>, substring <code>&quot;po&quot;</code> là một substring <strong>đặc biệt</strong>, nhưng <code>&quot;pop&quot;</code> không <strong>đặc biệt</strong> (vì <code>&#39;p&#39;</code> đã xuất hiện hai lần).</p>

<p>Trả về <em>số lượng substring <b>đặc biệt</b>.</em></p>

<p><strong>Substring</strong> là một dãy ký tự liên tiếp trong một chuỗi. Ví dụ, <code>&quot;abc&quot;</code> là một substring của <code>&quot;abcd&quot;</code>, nhưng <code>&quot;acd&quot;</code> thì không.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Vì mỗi ký tự chỉ xuất hiện một lần, mọi substring đều là substring đặc biệt. Có 4 substring độ dài một, 3 substring độ dài hai, 2 substring độ dài ba và 1 substring độ dài bốn. Vậy tổng cộng có 4 + 3 + 2 + 1 = 10 substring đặc biệt.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ooo&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Mọi substring có độ dài ít nhất hai đều chứa ký tự lặp lại. Vì vậy, ta chỉ cần đếm các substring có độ dài một, tổng cộng là 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abab&quot;
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Các substring đặc biệt như sau (sắp xếp theo vị trí bắt đầu):
Các substring đặc biệt có độ dài 1: &quot;a&quot;, &quot;b&quot;, &quot;a&quot;, &quot;b&quot;
Các substring đặc biệt có độ dài 2: &quot;ab&quot;, &quot;ba&quot;, &quot;ab&quot;
Có thể thấy không có substring đặc biệt nào có độ dài ít nhất ba. Do đó, đáp án là 4 + 3 = 7.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các substring mà mọi ký tự đều khác nhau. Kiểm tra mọi cặp đầu mút có độ phức tạp bậc hai và quá chậm với $n\le 10^5$.
>
> Sau khi thêm ký tự ở đầu phải, di chuyển đầu trái cho đến khi số lần xuất hiện của ký tự đó bằng $1$. Mọi substring kết thúc ở đầu phải và nằm trong cửa sổ đều hợp lệ, nên ta cộng độ dài của cửa sổ vào đáp án.

<!-- thinking:end -->

Ta dùng hai con trỏ $j$ và $i$ để biểu diễn biên trái và phải của substring hiện tại, cùng một mảng $cnt$ có độ dài $26$ để đếm số lần xuất hiện của mỗi ký tự trong substring hiện tại. Ta duyệt chuỗi từ trái sang phải. Mỗi khi duyệt đến vị trí $i$, ta tăng số lần xuất hiện của $s[i]$, rồi kiểm tra xem $s[i]$ có xuất hiện ít nhất hai lần hay không. Nếu có, ta cần giảm số lần xuất hiện của $s[j]$ và di chuyển $j$ sang phải một bước, cho đến khi số lần xuất hiện của $s[i]$ không vượt quá một lần. Nhờ đó, ta có được substring đặc biệt dài nhất kết thúc tại $s[i]$, có độ dài là $i - j + 1$, nên số substring đặc biệt kết thúc tại $s[i]$ cũng là $i - j + 1$. Cuối cùng, ta cộng số substring đặc biệt kết thúc tại mỗi vị trí để nhận được đáp án.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(C)$. Trong đó, $n$ là độ dài chuỗi $s$, còn $C$ là kích thước của bộ ký tự. Trong bài toán này, bộ ký tự gồm các chữ cái tiếng Anh viết thường, nên $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSpecialSubstrings(self, s: str) -> int:
        cnt = Counter()
        ans = j = 0
        for i, c in enumerate(s):
            cnt[c] += 1
            while cnt[c] > 1:
                cnt[s[j]] -= 1
                j += 1
            ans += i - j + 1
        return ans
```

#### Java

```java
class Solution {
    public int numberOfSpecialSubstrings(String s) {
        int n = s.length();
        int ans = 0;
        int[] cnt = new int[26];
        for (int i = 0, j = 0; i < n; ++i) {
            int k = s.charAt(i) - 'a';
            ++cnt[k];
            while (cnt[k] > 1) {
                --cnt[s.charAt(j++) - 'a'];
            }
            ans += i - j + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfSpecialSubstrings(string s) {
        int n = s.size();
        int cnt[26]{};
        int ans = 0;
        for (int i = 0, j = 0; i < n; ++i) {
            int k = s[i] - 'a';
            ++cnt[k];
            while (cnt[k] > 1) {
                --cnt[s[j++] - 'a'];
            }
            ans += i - j + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSpecialSubstrings(s string) (ans int) {
	j := 0
	cnt := [26]int{}
	for i, c := range s {
		k := c - 'a'
		cnt[k]++
		for cnt[k] > 1 {
			cnt[s[j]-'a']--
			j++
		}
		ans += i - j + 1
	}
	return
}
```

#### TypeScript

```ts
function numberOfSpecialSubstrings(s: string): number {
    const idx = (c: string) => c.charCodeAt(0) - 'a'.charCodeAt(0);
    const n = s.length;
    const cnt: number[] = Array(26).fill(0);
    let ans = 0;
    for (let i = 0, j = 0; i < n; ++i) {
        const k = idx(s[i]);
        ++cnt[k];
        while (cnt[k] > 1) {
            --cnt[idx(s[j++])];
        }
        ans += i - j + 1;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
