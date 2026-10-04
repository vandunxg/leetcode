---
comments: true
difficulty: Hard
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [3329. Count Substrings With K-Frequency Characters II 🔒](https://leetcode.com/problems/count-substrings-with-k-frequency-characters-ii)

[中文文档](/solution/3300-3399/3329.Count%20Substrings%20With%20K-Frequency%20Characters%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một số nguyên <code>k</code>, hãy trả về tổng số <span data-keyword="substring-nonempty">chuỗi con</span> của <code>s</code> mà trong đó có <strong>ít nhất một</strong> ký tự xuất hiện <strong>ít nhất</strong> <code>k</code> lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abacb&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi con hợp lệ là:</p>

<ul>
    <li>&quot;<code>aba&quot;</code> (ký tự <code>&#39;a&#39;</code> xuất hiện 2 lần).</li>
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

<p>Mọi chuỗi con đều hợp lệ vì mỗi ký tự xuất hiện ít nhất một lần.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 3 * 10<sup>5</sup></code></li>
    <li><code>1 &lt;= k &lt;= s.length</code></li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Số lượng cần đếm giống với phần I, nhưng $n \le 3 \times 10^5$, vì vậy cửa sổ phải được duy trì trong thời gian tuyến tính.
>
> Mệnh đề “có ký tự xuất hiện ít nhất $k$ lần” có tính đơn điệu theo đầu trái; ta duy trì hậu tố dài nhất mà số lần xuất hiện của mọi ký tự đều nhỏ hơn $k$.
>
> Code giống với phần I: sau khi thêm ký tự bên phải, ta tăng $l$ nếu cần rồi cộng $l$ vào đáp án.

<!-- thinking:end -->

Ta có thể duyệt điểm cuối bên phải của chuỗi con, sau đó dùng sliding window để duy trì điểm đầu bên trái của chuỗi con, đảm bảo số lần xuất hiện của mỗi ký tự trong sliding window nhỏ hơn $k$.

Ta có thể dùng một mảng $\textit{cnt}$ để duy trì số lần xuất hiện của mỗi ký tự trong sliding window, một biến $\textit{l}$ để duy trì điểm đầu bên trái của sliding window, và một biến $\textit{ans}$ để duy trì đáp án.

Khi duyệt điểm cuối bên phải, ta thêm ký tự tại điểm cuối bên phải vào sliding window, sau đó kiểm tra xem số lần xuất hiện của ký tự này trong sliding window có lớn hơn hoặc bằng $k$ hay không. Nếu có, ta loại bỏ ký tự tại điểm đầu bên trái khỏi sliding window cho đến khi số lần xuất hiện của mọi ký tự trong sliding window nhỏ hơn $k$. Khi đó, với các chuỗi con có điểm đầu bên trái nằm trong khoảng $[0, ..l - 1]$ và điểm cuối bên phải là $r$, tất cả đều thỏa mãn yêu cầu của bài toán, nên ta cộng $l$ vào đáp án.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự, cụ thể là tập các chữ cái viết thường, nên $|\Sigma| = 26$.

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
    public long numberOfSubstrings(String s, int k) {
        int[] cnt = new int[26];
        long ans = 0;
        for (int l = 0, r = 0; r < s.length(); ++r) {
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
    long long numberOfSubstrings(string s, int k) {
        int n = s.size();
        long long ans = 0, l = 0;
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
func numberOfSubstrings(s string, k int) (ans int64) {
    l := 0
    cnt := [26]int{}
    for _, c := range s {
        cnt[c-'a']++
        for cnt[c-'a'] >= k {
            cnt[s[l]-'a']--
            l++
        }
        ans += int64(l)
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
        const x = c.charCodeAt(0) - 97;
        ++cnt[x];
        while (cnt[x] >= k) {
            --cnt[s[l++].charCodeAt(0) - 97];
        }
        ans += l;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
