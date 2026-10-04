---
comments: true
difficulty: Medium
rating: 1454
source: Weekly Contest 420 Q2
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [3325. Count Substrings With K-Frequency Characters I](https://leetcode.com/problems/count-substrings-with-k-frequency-characters-i)

[中文文档](/solution/3300-3399/3325.Count%20Substrings%20With%20K-Frequency%20Characters%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một số nguyên <code>k</code>, hãy trả về tổng số <span data-keyword="substring-nonempty">chuỗi con</span> của <code>s</code> trong đó <strong>ít nhất một</strong> ký tự xuất hiện <strong>ít nhất</strong> <code>k</code> lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abacb&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi con hợp lệ là:</p>

<ul>
    <li><code>&quot;aba&quot;</code> (ký tự <code>&#39;a&#39;</code> xuất hiện 2 lần).</li>
    <li><code>&quot;abac&quot;</code> (ký tự <code>&#39;a&#39;</code> xuất hiện 2 lần).</li>
    <li><code>&quot;abacb&quot;</code> (ký tự <code>&#39;a&#39;</code> xuất hiện 2 lần).</li>
    <li><code>&quot;bacb&quot;</code> (ký tự <code>&#39;b&#39;</code> xuất hiện 2 lần).</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcde&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả các chuỗi con đều hợp lệ vì mỗi ký tự xuất hiện ít nhất một lần.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 3000</code></li>
    <li><code>1 &lt;= k &lt;= s.length</code></li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đếm các chuỗi con trong đó có một ký tự xuất hiện ít nhất $k$ lần. Với $n \le 3000$, việc duyệt hai lần là đủ nhanh, nhưng điều kiện này có tính đơn điệu nên ta có thể dùng cửa sổ tuyến tính.
>
> Khi một cửa sổ đã chứa $k$ bản sao của một chữ cái, mọi cửa sổ dài hơn bằng cách mở rộng sang trái vẫn hợp lệ. Vì vậy, ta duy trì hậu tố dài nhất của tiền tố hiện tại sao cho số lần xuất hiện của mọi ký tự đều nhỏ hơn $k$.
>
> Sau khi thêm $c$, nếu số lần xuất hiện của nó đạt $k$, ta tăng $l$ cho đến khi số lần xuất hiện của mọi ký tự lại nhỏ hơn $k$. Khi đó, mọi vị trí bắt đầu trong $[0, l)$ đều hợp lệ, nên ta cộng $l$ vào đáp án.

<!-- thinking:end -->

Ta có thể duyệt vị trí kết thúc của chuỗi con, sau đó dùng cửa sổ trượt để duy trì vị trí bắt đầu của chuỗi con, sao cho số lần xuất hiện của mỗi ký tự trong cửa sổ trượt nhỏ hơn $k$.

Ta có thể dùng một mảng $\textit{cnt}$ để duy trì số lần xuất hiện của mỗi ký tự trong cửa sổ trượt, một biến $\textit{l}$ để duy trì vị trí bắt đầu của cửa sổ trượt, và một biến $\textit{ans}$ để duy trì đáp án.

Khi duyệt vị trí kết thúc, ta thêm ký tự ở vị trí kết thúc vào cửa sổ trượt, sau đó kiểm tra xem số lần xuất hiện của ký tự đó trong cửa sổ trượt có lớn hơn hoặc bằng $k$ hay không. Nếu có, ta loại bỏ ký tự ở vị trí bắt đầu khỏi cửa sổ trượt cho đến khi số lần xuất hiện của mọi ký tự trong cửa sổ trượt nhỏ hơn $k$. Khi đó, với các chuỗi con có vị trí bắt đầu trong khoảng $[0, ..l - 1]$ và vị trí kết thúc là $r$, tất cả đều thỏa mãn yêu cầu của đề bài, nên ta cộng $l$ vào đáp án.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự. Ở đây là tập các chữ cái viết thường nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSubstrings(self, s: str, k: int) -> int:
        cnt = Counter()
        ans = l = 0
        for c in s:
            cnt[c] += 1
            while cnt[c] >= k:
                cnt[s[l]] -= 1
                l += 1
            ans += l
        return ans
```

#### Java

```java
class Solution {
    public int numberOfSubstrings(String s, int k) {
        int[] cnt = new int[26];
        int ans = 0, l = 0;
        for (int r = 0; r < s.length(); ++r) {
            int c = s.charAt(r) - 'a';
            ++cnt[c];
            while (cnt[c] >= k) {
                --cnt[s.charAt(l) - 'a'];
                l++;
            }
            ans += l;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfSubstrings(string s, int k) {
        int n = s.size();
        int ans = 0, l = 0;
        int cnt[26]{};
        for (char& c : s) {
            ++cnt[c - 'a'];
            while (cnt[c - 'a'] >= k) {
                --cnt[s[l++] - 'a'];
            }
            ans += l;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSubstrings(s string, k int) (ans int) {
    l := 0
    cnt := [26]int{}
    for _, c := range s {
        cnt[c-'a']++
        for cnt[c-'a'] >= k {
            cnt[s[l]-'a']--
            l++
        }
        ans += l
    }
    return
}
```

#### TypeScript

```ts
function numberOfSubstrings(s: string, k: number): number {
    let [ans, l] = [0, 0];
    const cnt: number[] = Array(26).fill(0);
    for (const c of s) {
        const x = c.charCodeAt(0) - 'a'.charCodeAt(0);
        ++cnt[x];
        while (cnt[x] >= k) {
            --cnt[s[l++].charCodeAt(0) - 'a'.charCodeAt(0)];
        }
        ans += l;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
