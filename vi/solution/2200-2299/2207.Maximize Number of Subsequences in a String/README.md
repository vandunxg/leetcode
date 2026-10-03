---
comments: true
difficulty: Medium
rating: 1550
source: Biweekly Contest 74 Q2
tags:
    - Greedy
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [2207. Maximize Number of Subsequences in a String](https://leetcode.com/problems/maximize-number-of-subsequences-in-a-string)

[Tài liệu tiếng Trung](/solution/2200-2299/2207.Maximize%20Number%20of%20Subsequences%20in%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <strong>được đánh chỉ số từ 0</strong> <code>text</code> và một chuỗi <strong>được đánh chỉ số từ 0</strong> khác <code>pattern</code> có độ dài <code>2</code>, cả hai chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Bạn có thể thêm <strong>hoặc</strong> <code>pattern[0]</code> <strong>hoặc</strong> <code>pattern[1]</code> vào bất kỳ vị trí nào trong <code>text</code> <strong>đúng một lần</strong>. Lưu ý rằng ký tự có thể được thêm vào đầu hoặc cuối <code>text</code>.</p>

<p>Trả về <em>số lần <strong>lớn nhất</strong></em> mà <code>pattern</code> có thể xuất hiện dưới dạng <strong>subsequence</strong> trong <em><code>text</code> sau khi được sửa đổi</em>.</p>

<p><b>Subsequence</b> là một chuỗi có thể thu được từ chuỗi khác bằng cách xóa một số hoặc không xóa ký tự nào mà không thay đổi thứ tự của các ký tự còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;abdcdbc&quot;, pattern = &quot;ac&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Nếu thêm pattern[0] = &#39;a&#39; vào giữa text[1] và text[2], ta được &quot;ab<u><strong>a</strong></u>dcdbc&quot;. Khi đó, số subsequence &quot;ac&quot; là 4.
Một số chuỗi khác có 4 subsequence &quot;ac&quot; sau khi thêm một ký tự vào text là &quot;<u><strong>a</strong></u>abdcdbc&quot; và &quot;abd<u><strong>a</strong></u>cdbc&quot;.
Tuy nhiên, các chuỗi như &quot;abdc<u><strong>a</strong></u>dbc&quot;, &quot;abd<u><strong>c</strong></u>cdbc&quot; và &quot;abdcdbc<u><strong>c</strong></u>&quot;, dù có thể tạo ra, chỉ có 3 subsequence &quot;ac&quot; nên không tối ưu.
Có thể chứng minh rằng không thể thu được nhiều hơn 4 subsequence &quot;ac&quot; chỉ bằng cách thêm một ký tự.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;aabb&quot;, pattern = &quot;ab&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Một số chuỗi có thể tạo ra từ text và có 6 subsequence &quot;ab&quot; là &quot;<u><strong>a</strong></u>aabb&quot;, &quot;aa<u><strong>a</strong></u>bb&quot; và &quot;aab<u><strong>b</strong></u>b&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= text.length &lt;= 10<sup>5</sup></code></li>
	<li><code>pattern.length == 2</code></li>
	<li><code>text</code> và <code>pattern</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể chèn một ký tự của $pattern$ vào bất kỳ vị trí nào để tối đa hóa số subsequence $pattern$. Vì $|text| \le 10^5$, không thể đếm lại sau mỗi lần chèn.
>
> Nếu chưa chèn, ta có thể duyệt từ trái sang phải: $x$ đếm số $pattern[0]$ đã gặp, còn mỗi $pattern[1]$ sẽ tạo thêm $x$ subsequence. Một lần chèn chỉ có thể tạo lợi ích khi thêm một tiền tố mới hoặc một hậu tố mới: chèn ở đầu sẽ thêm số subsequence bằng số $pattern[1]$ hiện có, còn chèn ở cuối sẽ thêm số subsequence bằng số $pattern[0]$ hiện có.
>
> Một lần duyệt cho ta đáp án ban đầu cùng với $x$ và $y$; sau đó cộng thêm $\max(x, y)$. Khi hai ký tự giống nhau, hãy đếm $pattern[1]$ trước khi tăng $x$, để vị trí hiện tại không được ghép với chính nó.

<!-- thinking:end -->

Ta có thể sử dụng hai biến $x$ và $y$ để lần lượt ghi nhận số lượng $\textit{pattern}[0]$ và $\textit{pattern}[1]$ hiện có trong chuỗi.

Sau đó, duyệt chuỗi $\textit{text}$. Với ký tự hiện tại $c$:

- Nếu $c$ bằng $\textit{pattern}[1]$, tăng $y$ lên một. Tại thời điểm này, mọi $\textit{pattern}[0]$ đã gặp trước đó đều có thể kết hợp với $c$ hiện tại để tạo thành một subsequence $\textit{pattern}$, nên cộng $x$ vào đáp án.
- Nếu $c$ bằng $\textit{pattern}[0]$, tăng $x$ lên một.

Sau khi duyệt xong, vì được phép chèn một ký tự, nếu thêm $\textit{pattern}[0]$ vào đầu chuỗi, ta có thể tạo thêm $y$ subsequence $\textit{pattern}$. Nếu thêm $\textit{pattern}[1]$ vào cuối chuỗi, ta có thể tạo thêm $x$ subsequence $\textit{pattern}$. Do đó, cộng giá trị lớn hơn giữa $x$ và $y$ vào đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $\textit{text}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSubsequenceCount(self, text: str, pattern: str) -> int:
        ans = x = y = 0
        for c in text:
            if c == pattern[1]:
                y += 1
                ans += x
            if c == pattern[0]:
                x += 1
        ans += max(x, y)
        return ans
```

#### Java

```java
class Solution {
    public long maximumSubsequenceCount(String text, String pattern) {
        long ans = 0;
        int x = 0, y = 0;
        for (int i = 0; i < text.length(); ++i) {
            if (text.charAt(i) == pattern.charAt(1)) {
                ++y;
                ans += x;
            }
            if (text.charAt(i) == pattern.charAt(0)) {
                ++x;
            }
        }
        ans += Math.max(x, y);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumSubsequenceCount(string text, string pattern) {
        long long ans = 0;
        int x = 0, y = 0;
        for (char& c : text) {
            if (c == pattern[1]) {
                ++y;
                ans += x;
            }
            if (c == pattern[0]) {
                ++x;
            }
        }
        ans += max(x, y);
        return ans;
    }
};
```

#### Go

```go
func maximumSubsequenceCount(text string, pattern string) (ans int64) {
	x, y := 0, 0
	for _, c := range text {
		if byte(c) == pattern[1] {
			y++
			ans += int64(x)
		}
		if byte(c) == pattern[0] {
			x++
		}
	}
	ans += int64(max(x, y))
	return
}
```

#### TypeScript

```ts
function maximumSubsequenceCount(text: string, pattern: string): number {
    let ans = 0;
    let [x, y] = [0, 0];
    for (const c of text) {
        if (c === pattern[1]) {
            ++y;
            ans += x;
        }
        if (c === pattern[0]) {
            ++x;
        }
    }
    ans += Math.max(x, y);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
