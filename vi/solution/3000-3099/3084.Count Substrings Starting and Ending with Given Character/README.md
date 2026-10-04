---
comments: true
difficulty: Medium
rating: 1323
source: Weekly Contest 389 Q2
tags:
    - Math
    - String
    - Counting
---

<!-- problem:start -->

# [3084. Count Substrings Starting and Ending with Given Character](https://leetcode.com/problems/count-substrings-starting-and-ending-with-given-character)

[中文文档](/solution/3000-3099/3084.Count%20Substrings%20Starting%20and%20Ending%20with%20Given%20Character/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một ký tự <code>c</code>. Hãy trả về <em>tổng số <span data-keyword="substring-nonempty">chuỗi con</span> của </em><code>s</code><em> bắt đầu và kết thúc bằng </em><code>c</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">s = &quot;abada&quot;, c = &quot;a&quot;</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">6</span></p>

<p><strong>Giải thích:</strong> Các chuỗi con bắt đầu và kết thúc bằng <code>&quot;a&quot;</code> là: <code>&quot;<strong><u>a</u></strong>bada&quot;</code>, <code>&quot;<u><strong>aba</strong></u>da&quot;</code>, <code>&quot;<u><strong>abada</strong></u>&quot;</code>, <code>&quot;ab<u><strong>a</strong></u>da&quot;</code>, <code>&quot;ab<u><strong>ada</strong></u>&quot;</code>, <code>&quot;abad<u><strong>a</strong></u>&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">s = &quot;zzz&quot;, c = &quot;z&quot;</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">6</span></p>

<p><strong>Giải thích:</strong> Có tổng cộng <code>6</code> chuỗi con trong <code>s</code> và tất cả đều bắt đầu và kết thúc bằng <code>&quot;z&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> và <code>c</code> chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi con phải bắt đầu và kết thúc bằng cùng một ký tự đã cho. $n \le 10^5$, nên ta không thể liệt kê tất cả các điểm kết thúc.
>
> Mỗi lần xuất hiện của $c$ tự tạo thành một chuỗi con, và mỗi cặp lần xuất hiện tạo thêm một chuỗi con.
>
> Nếu $c$ xuất hiện $\textit{cnt}$ lần, đáp án là $\textit{cnt}+\textit{cnt}(\textit{cnt}-1)/2$.

<!-- thinking:end -->

Trước hết, ta đếm số lần ký tự $c$ xuất hiện trong chuỗi $s$, gọi là $cnt$.

Mỗi ký tự $c$ có thể tự tạo thành một chuỗi con, nên có $cnt$ chuỗi con thỏa mãn điều kiện. Mỗi ký tự $c$ có thể tạo thành một chuỗi con với các ký tự $c$ khác, nên có $\frac{cnt \times (cnt - 1)}{2}$ chuỗi con thỏa mãn điều kiện.

Do đó, đáp án là $cnt + \frac{cnt \times (cnt - 1)}{2}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubstrings(self, s: str, c: str) -> int:
        cnt = s.count(c)
        return cnt + cnt * (cnt - 1) // 2
```

#### Java

```java
class Solution {
    public long countSubstrings(String s, char c) {
        long cnt = s.chars().filter(ch -> ch == c).count();
        return cnt + cnt * (cnt - 1) / 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countSubstrings(string s, char c) {
        long long cnt = ranges::count(s, c);
        return cnt + cnt * (cnt - 1) / 2;
    }
};
```

#### Go

```go
func countSubstrings(s string, c byte) int64 {
	cnt := int64(strings.Count(s, string(c)))
	return cnt + cnt*(cnt-1)/2
}
```

#### TypeScript

```ts
function countSubstrings(s: string, c: string): number {
    const cnt = s.split('').filter(ch => ch === c).length;
    return cnt + Math.floor((cnt * (cnt - 1)) / 2);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
