---
comments: true
difficulty: Medium
rating: 1754
source: Weekly Contest 509 Q2
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [3983. Subsequence After One Replacement](https://leetcode.com/problems/subsequence-after-one-replacement)

[中文文档](/solution/3900-3999/3983.Subsequence%20After%20One%20Replacement/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh thường.</p>

<p>Bạn có thể chọn <strong>nhiều nhất</strong> một chỉ số trong <code>s</code> và thay ký tự tại chỉ số đó bằng một chữ cái tiếng Anh thường bất kỳ.</p>

<p>Trả về <code>true</code> nếu có thể biến <code>s</code> thành một <span data-keyword="subsequence-string">dãy con</span> của <code>t</code>; nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;cat&quot;, t = &quot;chat&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Thay <code>s[1]</code> từ <code>&#39;a&#39;</code> thành <code>&#39;h&#39;</code>. Chuỗi thu được là <code>&quot;cht&quot;</code>.</li>
	<li><code>&quot;cht&quot;</code> là dãy con của <code>&quot;chat&quot;</code> vì ta có thể lần lượt khớp <code>&#39;c&#39;</code>, <code>&#39;h&#39;</code> và <code>&#39;t&#39;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;plane&quot;, t = &quot;apple&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các ký tự <code>&#39;p&#39;</code>, <code>&#39;l&#39;</code> và <code>&#39;e&#39;</code> có thể được khớp trong <code>t</code>, nhưng các ký tự còn lại không thể được khớp mà vẫn giữ đúng thứ tự yêu cầu.</li>
	<li>Ngay cả khi thay thế một ký tự bất kỳ trong <code>s</code>, cũng không thể biến <code>s</code> thành dãy con của <code>t</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, t.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> $s$ cần là dãy con của $t$ sau nhiều nhất một lần thay thế. Cả hai chuỗi dài tới $10^5$, nên không thể thử mọi vị trí thay thế.
>
> Ta duy trì $i_0$ là vị trí khớp khi không thay thế và $i_1$ là vị trí khớp khi được phép thay thế nhiều nhất một lần. Khi quét $t$, ta tiến $i_1$ nếu khớp, nâng nó lên $i_0+1$ để dùng phép thay thế ngay sau $i_0$, rồi tiến $i_0$.
>
> Thành công khi $i_1=|s|$.

<!-- thinking:end -->

Bài toán tương đương với việc kiểm tra xem ta có thể tham lam khớp $s$ như một dãy con của $t$ hay không, trong đó cho phép nhiều nhất một ký tự trong $s$ không khớp, vì ký tự đó có thể được thay bằng bất kỳ chữ cái nào.

Ta dùng hai con trỏ $i_0$ và $i_1$ để duyệt $s$, đồng thời dùng con trỏ $j$ để duyệt $t$:

- $i_0$ là vị trí hiện tại trong $s$ khi khớp mà không dùng phép thay thế.
- $i_1$ là vị trí hiện tại trong $s$ khi còn được phép dùng nhiều nhất một phép thay thế.

Với mỗi ký tự $t[j]$:

1. Nếu $s[i_1] = t[j]$, tăng $i_1$ lên một.
2. Đặt $i_1 = \max(i_1, i_0 + 1)$ để vị trí thay thế không bao giờ nằm trước $i_0$, dành lại một ký tự cho phép thay thế.
3. Nếu $s[i_0] = t[j]$, tăng $i_0$ lên một.
4. Tăng $j$ lên một.

Sau khi quét xong, nếu $i_1 = |s|$ thì mọi ký tự của $s$ đều có thể được khớp theo đúng thứ tự trong $t$ với nhiều nhất một lần thay thế, nên trả về `true`; ngược lại trả về `false`.

Độ phức tạp thời gian là $O(|s| + |t|)$, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canMakeSubsequence(self, s: str, t: str) -> bool:
        m, n = len(s), len(t)
        i0 = i1 = j = 0

        while i1 < m and j < n:
            if s[i1] == t[j]:
                i1 += 1
            i1 = max(i1, i0 + 1)

            if s[i0] == t[j]:
                i0 += 1

            j += 1

        return i1 == m
```

#### Java

```java
class Solution {
    public boolean canMakeSubsequence(String s, String t) {
        int m = s.length(), n = t.length();
        int i0 = 0, i1 = 0, j = 0;

        while (i1 < m && j < n) {
            if (s.charAt(i1) == t.charAt(j)) {
                i1++;
            }

            i1 = Math.max(i1, i0 + 1);

            if (s.charAt(i0) == t.charAt(j)) {
                i0++;
            }

            j++;
        }

        return i1 == m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canMakeSubsequence(string s, string t) {
        int m = s.size(), n = t.size();
        int i0 = 0, i1 = 0, j = 0;

        while (i1 < m && j < n) {
            if (s[i1] == t[j]) {
                i1++;
            }

            i1 = max(i1, i0 + 1);

            if (s[i0] == t[j]) {
                i0++;
            }

            j++;
        }

        return i1 == m;
    }
};
```

#### Go

```go
func canMakeSubsequence(s string, t string) bool {
	m, n := len(s), len(t)
	i0, i1, j := 0, 0, 0

	for i1 < m && j < n {
		if s[i1] == t[j] {
			i1++
		}

		if i1 < i0+1 {
			i1 = i0 + 1
		}

		if s[i0] == t[j] {
			i0++
		}

		j++
	}

	return i1 == m
}
```

#### TypeScript

```ts
function canMakeSubsequence(s: string, t: string): boolean {
    const m = s.length,
        n = t.length;
    let i0 = 0,
        i1 = 0,
        j = 0;

    while (i1 < m && j < n) {
        if (s[i1] === t[j]) {
            i1++;
        }

        i1 = Math.max(i1, i0 + 1);

        if (s[i0] === t[j]) {
            i0++;
        }

        j++;
    }

    return i1 === m;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
