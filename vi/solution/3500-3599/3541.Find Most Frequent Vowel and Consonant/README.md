---
comments: true
difficulty: Easy
rating: 1238
source: Biweekly Contest 156 Q1
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [3541. Find Most Frequent Vowel and Consonant](https://leetcode.com/problems/find-most-frequent-vowel-and-consonant)

[中文文档](/solution/3500-3599/3541.Find%20Most%20Frequent%20Vowel%20and%20Consonant/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường (từ <code>&#39;a&#39;</code> đến <code>&#39;z&#39;</code>).</p>

<p>Nhiệm vụ của bạn là:</p>

<ul>
    <li>Tìm nguyên âm (một trong các ký tự <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> hoặc <code>&#39;u&#39;</code>) có tần suất <strong>lớn nhất</strong>.</li>
    <li>Tìm phụ âm (tất cả các chữ cái khác, không bao gồm nguyên âm) có tần suất <strong>lớn nhất</strong>.</li>
</ul>

<p>Trả về tổng của hai tần suất trên.</p>

<p><strong>Lưu ý</strong>: Nếu nhiều nguyên âm hoặc phụ âm có cùng tần suất lớn nhất, bạn có thể chọn bất kỳ ký tự nào trong số đó. Nếu chuỗi không có nguyên âm hoặc không có phụ âm, hãy coi tần suất tương ứng là 0.</p>
<strong>Tần suất</strong> của chữ cái <code>x</code> là số lần nó xuất hiện trong chuỗi.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;successes&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các nguyên âm là: <code>&#39;u&#39;</code> (tần suất 1), <code>&#39;e&#39;</code> (tần suất 2). Tần suất lớn nhất là 2.</li>
    <li>Các phụ âm là: <code>&#39;s&#39;</code> (tần suất 4), <code>&#39;c&#39;</code> (tần suất 2). Tần suất lớn nhất là 4.</li>
    <li>Kết quả là <code>2 + 4 = 6</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aeiaeia&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các nguyên âm là: <code>&#39;a&#39;</code> (tần suất 3), <code>&#39;e&#39;</code> (tần suất 2), <code>&#39;i&#39;</code> (tần suất 2). Tần suất lớn nhất là 3.</li>
    <li><code>s</code> không có phụ âm. Do đó, tần suất phụ âm lớn nhất = 0.</li>
    <li>Kết quả là <code>3 + 0 = 3</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 100</code></li>
    <li><code>s</code> chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm tổng của tần suất nguyên âm lớn nhất và tần suất phụ âm lớn nhất, không phụ thuộc vào ký tự nào đạt được tần suất đó. Chỉ cần duyệt một lượt để đếm và duy trì hai giá trị lớn nhất.
>
> Coi lớp ký tự bị thiếu có tần suất bằng $0$, rồi cộng hai giá trị lớn nhất.

<!-- thinking:end -->

Trước tiên, ta dùng một hash table hoặc một mảng có độ dài $26$, $\textit{cnt}$, để đếm tần suất của mỗi chữ cái. Sau đó, ta duyệt qua bảng này để tìm nguyên âm và phụ âm xuất hiện nhiều nhất, rồi trả về tổng tần suất của chúng.

Ta có thể dùng biến $\textit{a}$ để lưu tần suất lớn nhất của các nguyên âm và một biến $\textit{b}$ để lưu tần suất lớn nhất của các phụ âm. Trong quá trình duyệt, nếu chữ cái hiện tại là nguyên âm, ta cập nhật $\textit{a}$; ngược lại, ta cập nhật $\textit{b}$.

Cuối cùng, ta trả về $\textit{a} + \textit{b}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $|\Sigma|$ là kích thước của bảng chữ cái, ở đây là $26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxFreqSum(self, s: str) -> int:
        cnt = Counter(s)
        a = b = 0
        for c, v in cnt.items():
            if c in "aeiou":
                a = max(a, v)
            else:
                b = max(b, v)
        return a + b
```

#### Java

```java
class Solution {
    public int maxFreqSum(String s) {
        int[] cnt = new int[26];
        for (char c : s.toCharArray()) {
            ++cnt[c - 'a'];
        }
        int a = 0, b = 0;
        for (int i = 0; i < cnt.length; ++i) {
            char c = (char) (i + 'a');
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
                a = Math.max(a, cnt[i]);
            } else {
                b = Math.max(b, cnt[i]);
            }
        }
        return a + b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxFreqSum(string s) {
        int cnt[26]{};
        for (char c : s) {
            ++cnt[c - 'a'];
        }
        int a = 0, b = 0;
        for (int i = 0; i < 26; ++i) {
            char c = 'a' + i;
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
                a = max(a, cnt[i]);
            } else {
                b = max(b, cnt[i]);
            }
        }
        return a + b;
    }
};
```

#### Go

```go
func maxFreqSum(s string) int {
    cnt := [26]int{}
    for _, c := range s {
        cnt[c-'a']++
    }
    a, b := 0, 0
    for i := range cnt {
        c := byte(i + 'a')
        if c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u' {
            a = max(a, cnt[i])
        } else {
            b = max(b, cnt[i])
        }
    }
    return a + b
}
```

#### TypeScript

```ts
function maxFreqSum(s: string): number {
    const cnt: number[] = Array(26).fill(0);
    for (const c of s) {
        ++cnt[c.charCodeAt(0) - 97];
    }
    let [a, b] = [0, 0];
    for (let i = 0; i < 26; ++i) {
        const c = String.fromCharCode(i + 97);
        if ('aeiou'.includes(c)) {
            a = Math.max(a, cnt[i]);
        } else {
            b = Math.max(b, cnt[i]);
        }
    }
    return a + b;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn max_freq_sum(s: String) -> i32 {
        let mut cnt: HashMap<char, i32> = HashMap::new();
        for c in s.chars() {
            *cnt.entry(c).or_insert(0) += 1;
        }
        let mut a = 0;
        let mut b = 0;
        for (c, v) in cnt {
            if "aeiou".contains(c) {
                a = a.max(v);
            } else {
                b = b.max(v);
            }
        }
        a + b
    }
}
```

#### C#

```cs
public class Solution {
    public int MaxFreqSum(string s) {
        int[] cnt = new int[26];
        foreach (char c in s) {
            cnt[c - 'a']++;
        }
        int a = 0, b = 0;
        for (int i = 0; i < 26; i++) {
            char c = (char)('a' + i);
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
                a = Math.Max(a, cnt[i]);
            } else {
                b = Math.Max(b, cnt[i]);
            }
        }
        return a + b;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
