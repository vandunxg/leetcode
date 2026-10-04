---
comments: true
difficulty: Easy
rating: 1201
source: Biweekly Contest 153 Q1
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [3498. Reverse Degree of a String](https://leetcode.com/problems/reverse-degree-of-a-string)

[中文文档](/solution/3400-3499/3498.Reverse%20Degree%20of%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>, hãy tính <strong>độ đảo</strong> của chuỗi.</p>

<p><strong>Độ đảo</strong> được tính như sau:</p>

<ol>
	<li>Với mỗi ký tự, nhân vị trí của ký tự đó trong bảng chữ cái <em>đảo ngược</em> (<code>&#39;a&#39;</code> = 26, <code>&#39;b&#39;</code> = 25, ..., <code>&#39;z&#39;</code> = 1) với vị trí của nó trong chuỗi <strong>(đánh số từ 1)</strong>.</li>
	<li>Cộng các tích này cho mọi ký tự trong chuỗi.</li>
</ol>

<p>Trả về <strong>độ đảo</strong> của <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">148</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Ký tự</th>
			<th style="border: 1px solid black;">Vị trí trong bảng chữ cái đảo ngược</th>
			<th style="border: 1px solid black;">Vị trí trong chuỗi</th>
			<th style="border: 1px solid black;">Tích</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>&#39;a&#39;</code></td>
			<td style="border: 1px solid black;">26</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">26</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>&#39;b&#39;</code></td>
			<td style="border: 1px solid black;">25</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">50</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>&#39;c&#39;</code></td>
			<td style="border: 1px solid black;">24</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">72</td>
		</tr>
	</tbody>
</table>

<p>Độ đảo là <code>26 + 50 + 72 = 148</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;zaza&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">160</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Ký tự</th>
			<th style="border: 1px solid black;">Vị trí trong bảng chữ cái đảo ngược</th>
			<th style="border: 1px solid black;">Vị trí trong chuỗi</th>
			<th style="border: 1px solid black;">Tích</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>&#39;z&#39;</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>&#39;a&#39;</code></td>
			<td style="border: 1px solid black;">26</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">52</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>&#39;z&#39;</code></td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>&#39;a&#39;</code></td>
			<td style="border: 1px solid black;">26</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">104</td>
		</tr>
	</tbody>
</table>

<p>Độ đảo là <code>1 + 52 + 3 + 104 = 160</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Độ đảo là tổng của tích giữa thứ hạng trong bảng chữ cái đảo ngược của mỗi ký tự và chỉ số bắt đầu từ $1$ của ký tự đó. Vì $|s|\le 1000$, chỉ cần duyệt chuỗi một lần.
>
> $\texttt{a}$ tương ứng với $26$, còn $\texttt{z}$ tương ứng với $1$, tức là $26-(\textit{ord}(c)-\textit{ord}(\texttt{a}))$.
>
> Duyệt các cặp $(i,c)$ với $i$ bắt đầu từ $1$ và cộng tích tương ứng.

<!-- thinking:end -->

Ta có thể mô phỏng độ đảo của từng ký tự trong chuỗi. Với mỗi ký tự, tính vị trí của nó trong bảng chữ cái đảo ngược, nhân với vị trí của ký tự trong chuỗi, rồi cộng tất cả các kết quả lại.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reverseDegree(self, s: str) -> int:
        ans = 0
        for i, c in enumerate(s, 1):
            x = 26 - (ord(c) - ord("a"))
            ans += i * x
        return ans
```

#### Java

```java
class Solution {
    public int reverseDegree(String s) {
        int n = s.length();
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            int x = 26 - (s.charAt(i - 1) - 'a');
            ans += i * x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int reverseDegree(string s) {
        int n = s.length();
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            int x = 26 - (s[i - 1] - 'a');
            ans += i * x;
        }
        return ans;
    }
};
```

#### Go

```go
func reverseDegree(s string) (ans int) {
	for i, c := range s {
		x := 26 - int(c-'a')
		ans += (i + 1) * x
	}
	return
}
```

#### TypeScript

```ts
function reverseDegree(s: string): number {
    let ans = 0;
    for (let i = 1; i <= s.length; ++i) {
        const x = 26 - (s.charCodeAt(i - 1) - 'a'.charCodeAt(0));
        ans += i * x;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
