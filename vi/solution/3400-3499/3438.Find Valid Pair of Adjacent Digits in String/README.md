---
comments: true
difficulty: Easy
rating: 1225
source: Biweekly Contest 149 Q1
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [3438. Find Valid Pair of Adjacent Digits in String](https://leetcode.com/problems/find-valid-pair-of-adjacent-digits-in-string)

[中文文档](/solution/3400-3499/3438.Find%20Valid%20Pair%20of%20Adjacent%20Digits%20in%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ số. Một <strong>cặp hợp lệ</strong> được định nghĩa là hai chữ số <strong>kề nhau</strong> trong <code>s</code> sao cho:</p>

<ul>
    <li>Chữ số đầu tiên <strong>khác</strong> chữ số thứ hai.</li>
    <li>Mỗi chữ số trong cặp xuất hiện trong <code>s</code> <strong>đúng</strong> số lần bằng giá trị số của nó.</li>
</ul>

<p>Trả về <strong>cặp hợp lệ</strong> đầu tiên được tìm thấy trong chuỗi <code>s</code> khi duyệt từ trái sang phải. Nếu không tồn tại cặp hợp lệ, trả về một chuỗi rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;2523533&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;23&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chữ số <code>&#39;2&#39;</code> xuất hiện 2 lần và chữ số <code>&#39;3&#39;</code> xuất hiện 3 lần. Mỗi chữ số trong cặp <code>&quot;23&quot;</code> xuất hiện trong <code>s</code> đúng số lần bằng giá trị số của nó. Vì vậy, kết quả là <code>&quot;23&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;221&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;21&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chữ số <code>&#39;2&#39;</code> xuất hiện 2 lần và chữ số <code>&#39;1&#39;</code> xuất hiện 1 lần. Vì vậy, kết quả là <code>&quot;21&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;22&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có cặp chữ số kề nhau nào hợp lệ.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= s.length &lt;= 100</code></li>
    <li><code>s</code> chỉ gồm các chữ số từ <code>&#39;1&#39;</code> đến <code>&#39;9&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp hợp lệ gồm hai chữ số khác nhau, trong đó số lần xuất hiện của mỗi chữ số trong toàn chuỗi đúng bằng chính chữ số đó. Vì $|s|\le 100$, chỉ cần đếm số lần xuất hiện rồi quét một lần các cặp kề nhau.
>
> Nếu vừa quét vừa đếm, các lần xuất hiện ở bên phải cặp đang xét chưa được tính, khiến tần suất bị sai.
>
> Trước tiên, ta điền một mảng tần suất có độ dài $10$, sau đó trả về cặp chữ số kề nhau đầu tiên từ trái sang phải thỏa mãn $x\neq y$, $cnt[x]=x$ và $cnt[y]=y$.

<!-- thinking:end -->

Ta có thể dùng một mảng $\textit{cnt}$ có độ dài $10$ để ghi nhận số lần xuất hiện của mỗi chữ số trong chuỗi $\textit{s}$.

Sau đó, ta duyệt các cặp chữ số kề nhau trong chuỗi $\textit{s}$. Nếu hai chữ số khác nhau và số lần xuất hiện của mỗi chữ số đúng bằng chính chữ số đó, ta đã tìm thấy một cặp chữ số kề nhau hợp lệ và trả về cặp này.

Sau khi duyệt xong, nếu không tìm thấy cặp chữ số kề nhau hợp lệ nào, ta trả về một chuỗi rỗng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $\textit{s}$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự của chuỗi $\textit{s}$. Trong bài toán này, $\Sigma = \{1, 2, \ldots, 9\}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findValidPair(self, s: str) -> str:
        cnt = [0] * 10
        for x in map(int, s):
            cnt[x] += 1
        for x, y in pairwise(map(int, s)):
            if x != y and cnt[x] == x and cnt[y] == y:
                return f"{x}{y}"
        return ""
```

#### Java

```java
class Solution {
    public String findValidPair(String s) {
        int[] cnt = new int[10];
        for (char c : s.toCharArray()) {
            ++cnt[c - '0'];
        }
        for (int i = 1; i < s.length(); ++i) {
            int x = s.charAt(i - 1) - '0';
            int y = s.charAt(i) - '0';
            if (x != y && cnt[x] == x && cnt[y] == y) {
                return s.substring(i - 1, i + 1);
            }
        }
        return "";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string findValidPair(string s) {
        int cnt[10]{};
        for (char c : s) {
            ++cnt[c - '0'];
        }
        for (int i = 1; i < s.size(); ++i) {
            int x = s[i - 1] - '0';
            int y = s[i] - '0';
            if (x != y && cnt[x] == x && cnt[y] == y) {
                return s.substr(i - 1, 2);
            }
        }
        return "";
    }
};
```

#### Go

```go
func findValidPair(s string) string {
    cnt := [10]int{}
    for _, c := range s {
        cnt[c-'0']++
    }
    for i := 1; i < len(s); i++ {
        x, y := int(s[i-1]-'0'), int(s[i]-'0')
        if x != y && cnt[x] == x && cnt[y] == y {
            return s[i-1 : i+1]
        }
    }
    return ""
}
```

#### TypeScript

```ts
function findValidPair(s: string): string {
    const cnt: number[] = Array(10).fill(0);
    for (const c of s) {
        ++cnt[+c];
    }
    for (let i = 1; i < s.length; ++i) {
        const x = +s[i - 1];
        const y = +s[i];
        if (x !== y && cnt[x] === x && cnt[y] === y) {
            return `${x}${y}`;
        }
    }
    return '';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
