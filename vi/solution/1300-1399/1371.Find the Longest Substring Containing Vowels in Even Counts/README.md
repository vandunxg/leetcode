---
comments: true
difficulty: Medium
rating: 2040
source: Biweekly Contest 21 Q2
tags:
    - Bit Manipulation
    - Hash Table
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [1371. Find the Longest Substring Containing Vowels in Even Counts](https://leetcode.com/problems/find-the-longest-substring-containing-vowels-in-even-counts)

[中文文档](/solution/1300-1399/1371.Find%20the%20Longest%20Substring%20Containing%20Vowels%20in%20Even%20Counts/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy trả về độ dài của chuỗi con dài nhất mà mỗi nguyên âm xuất hiện số lần chẵn. Cụ thể, &#39;a&#39;, &#39;e&#39;, &#39;i&#39;, &#39;o&#39; và &#39;u&#39; phải xuất hiện số lần chẵn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;eleetminicoworoep&quot;
<strong>Đầu ra:</strong> 13
<strong>Giải thích: </strong>Chuỗi con dài nhất là &quot;leetminicowor&quot;, trong đó các nguyên âm <strong>e</strong>, <strong>i</strong> và <strong>o</strong> mỗi ký tự xuất hiện hai lần, còn <strong>a</strong> và <strong>u</strong> không xuất hiện lần nào.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leetcodeisgreat&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Chuỗi con dài nhất là &quot;leetc&quot;, trong đó chữ e xuất hiện hai lần.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bcbcbc&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Trong trường hợp này, toàn bộ chuỗi &quot;bcbcbc&quot; là chuỗi con dài nhất vì tất cả nguyên âm: <strong>a</strong>, <strong>e</strong>, <strong>i</strong>, <strong>o</strong> và <strong>u</strong> đều không xuất hiện lần nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 x 10^5</code></li>
	<li><code>s</code>&nbsp;chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix XOR + Mảng hoặc Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm chuỗi con dài nhất mà mỗi nguyên âm xuất hiện số lần chẵn. Vì $n \le 5 \times 10^5$, không thể thử mọi cặp đầu mút. Số lần xuất hiện chẵn tương đương với việc hai prefix có cùng parity mask. Dùng mask 5 bit để theo dõi tính chẵn lẻ; vị trí xuất hiện đầu tiên của mỗi mask và lần xuất hiện lại tại $i$ cho độ dài $i-j$.

<!-- thinking:end -->

Theo đề bài, nếu dùng một số để biểu diễn tính chẵn lẻ số lần xuất hiện của từng nguyên âm trong một prefix của chuỗi $\textit{s}$, thì hai prefix có cùng giá trị sẽ xác định một chuỗi con hợp lệ nằm giữa chúng.

Ta có thể dùng 5 bit thấp của một số nhị phân để biểu diễn tính chẵn lẻ của 5 nguyên âm. Bit thứ $i$ bằng $1$ nghĩa là nguyên âm tương ứng xuất hiện số lần lẻ trong chuỗi con; bằng $0$ nghĩa là xuất hiện số lần chẵn.

Ta dùng $\textit{mask}$ để biểu diễn số nhị phân này và dùng mảng hoặc hash table $\textit{d}$ để ghi lại vị trí xuất hiện đầu tiên của mỗi $\textit{mask}$. Ban đầu, đặt $\textit{d}[0] = -1$, biểu thị vị trí bắt đầu của chuỗi rỗng là $-1$.

Ta duyệt chuỗi $\textit{s}$; nếu gặp nguyên âm, đảo bit tương ứng trong $\textit{mask}$. Tiếp theo, kiểm tra xem $\textit{mask}$ đã xuất hiện trước đó chưa. Nếu rồi, ta tìm được một chuỗi con hợp lệ có độ dài bằng vị trí hiện tại trừ đi lần xuất hiện gần nhất của $\textit{mask}$. Nếu chưa, lưu vị trí hiện tại của $\textit{mask}$ vào $\textit{d}$.

Sau khi duyệt hết chuỗi, ta thu được chuỗi con hợp lệ dài nhất.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài của chuỗi $\textit{s}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findTheLongestSubstring(self, s: str) -> int:
        d = {0: -1}
        ans = mask = 0
        for i, c in enumerate(s):
            if c in "aeiou":
                mask ^= 1 << (ord(c) - ord("a"))
            if mask in d:
                j = d[mask]
                ans = max(ans, i - j)
            else:
                d[mask] = i
        return ans
```

#### Java

```java
class Solution {
    public int findTheLongestSubstring(String s) {
        String vowels = "aeiou";
        int[] d = new int[32];
        Arrays.fill(d, 1 << 29);
        d[0] = 0;
        int ans = 0, mask = 0;
        for (int i = 1; i <= s.length(); ++i) {
            char c = s.charAt(i - 1);
            for (int j = 0; j < 5; ++j) {
                if (c == vowels.charAt(j)) {
                    mask ^= 1 << j;
                    break;
                }
            }
            ans = Math.max(ans, i - d[mask]);
            d[mask] = Math.min(d[mask], i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findTheLongestSubstring(string s) {
        string vowels = "aeiou";
        vector<int> d(32, INT_MAX);
        d[0] = 0;
        int ans = 0, mask = 0;
        for (int i = 1; i <= s.length(); ++i) {
            char c = s[i - 1];
            for (int j = 0; j < 5; ++j) {
                if (c == vowels[j]) {
                    mask ^= 1 << j;
                    break;
                }
            }
            ans = max(ans, i - d[mask]);
            d[mask] = min(d[mask], i);
        }
        return ans;
    }
};
```

#### Go

```go
func findTheLongestSubstring(s string) (ans int) {
    vowels := "aeiou"
    d := [32]int{}
    for i := range d {
        d[i] = 1 << 29
    }
    d[0] = 0
    mask := 0
    for i := 1; i <= len(s); i++ {
        c := s[i-1]
        for j := 0; j < 5; j++ {
            if c == vowels[j] {
                mask ^= 1 << j
                break
            }
        }
        ans = max(ans, i-d[mask])
        d[mask] = min(d[mask], i)
    }
    return
}
```

#### TypeScript

```ts
function findTheLongestSubstring(s: string): number {
    const vowels = 'aeiou';
    const d: number[] = Array(32).fill(1 << 29);
    d[0] = 0;
    let [ans, mask] = [0, 0];
    for (let i = 1; i <= s.length; i++) {
        const c = s[i - 1];
        for (let j = 0; j < 5; j++) {
            if (c === vowels[j]) {
                mask ^= 1 << j;
                break;
            }
        }
        ans = Math.max(ans, i - d[mask]);
        d[mask] = Math.min(d[mask], i);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
