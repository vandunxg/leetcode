---
comments: true
difficulty: Medium
rating: 1467
source: Biweekly Contest 63 Q2
tags:
    - Greedy
    - Math
    - String
    - Game Theory
---

<!-- problem:start -->

# [2038. Remove Colored Pieces if Both Neighbors are the Same Color](https://leetcode.com/problems/remove-colored-pieces-if-both-neighbors-are-the-same-color)

[中文文档](/solution/2000-2099/2038.Remove%20Colored%20Pieces%20if%20Both%20Neighbors%20are%20the%20Same%20Color/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> quân cờ được xếp thành một hàng, mỗi quân được tô màu <code>&#39;A&#39;</code> hoặc <code>&#39;B&#39;</code>. Cho chuỗi <code>colors</code> có độ dài <code>n</code>, trong đó <code>colors[i]</code> là màu của quân cờ thứ <code>i<sup>th</sup></code>.</p>

<p>Alice và Bob chơi một trò chơi trong đó họ <strong>luân phiên lượt</strong> xóa các quân cờ khỏi hàng. Alice đi<strong> trước</strong>.</p>

<ul>
	<li>Alice chỉ được phép xóa một quân cờ màu <code>&#39;A&#39;</code> nếu <strong>cả hai quân cờ ở hai bên</strong> cũng có màu <code>&#39;A&#39;</code>. Cô ấy <strong>không được</strong> xóa quân cờ màu <code>&#39;B&#39;</code>.</li>
	<li>Bob chỉ được phép xóa một quân cờ màu <code>&#39;B&#39;</code> nếu <strong>cả hai quân cờ ở hai bên</strong> cũng có màu <code>&#39;B&#39;</code>. Anh ấy <strong>không được</strong> xóa quân cờ màu <code>&#39;A&#39;</code>.</li>
	<li>Alice và Bob <strong>không được</strong> xóa các quân cờ ở đầu hàng.</li>
	<li>Nếu một người chơi không thể thực hiện nước đi trong lượt của mình, người đó sẽ <strong>thua</strong> và người chơi còn lại sẽ <strong>thắng</strong>.</li>
</ul>

<p>Giả sử Alice và Bob đều chơi tối ưu, hãy trả về <code>true</code><em> nếu Alice thắng, hoặc trả về </em><code>false</code><em> nếu Bob thắng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> colors = &quot;AAABABB&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
A<u>A</u>ABABB -&gt; AABABB
Alice đi trước.
Cô xóa quân &#39;A&#39; thứ hai từ trái sang vì đó là quân &#39;A&#39; duy nhất có hai quân cờ ở hai bên đều là &#39;A&#39;.

Bây giờ đến lượt Bob.
Bob không thể thực hiện nước đi vì không có quân &#39;B&#39; nào có hai quân cờ ở hai bên đều là &#39;B&#39;.
Do đó, Alice thắng, nên trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> colors = &quot;AA&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
Alice đi trước.
Chỉ có hai quân &#39;A&#39; và cả hai đều ở đầu hàng, nên cô không thể thực hiện nước đi.
Do đó, Bob thắng, nên trả về false.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> colors = &quot;ABBBBBBBAAA&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
ABBBBBBBA<u>A</u>A -&gt; ABBBBBBBAA
Alice đi trước.
Cô chỉ có thể xóa quân &#39;A&#39; áp chót tính từ bên phải.

ABBBB<u>B</u>BBAA -&gt; ABBBBBBAA
Tiếp theo là lượt của Bob.
Bob có nhiều lựa chọn để xóa quân &#39;B&#39;. Anh ấy có thể chọn bất kỳ quân nào.

Đến lượt thứ hai của Alice, cô không còn quân nào có thể xóa.
Do đó, Bob thắng, nên trả về false.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;=&nbsp;colors.length &lt;= 10<sup>5</sup></code></li>
	<li><code>colors</code>&nbsp;chỉ gồm các chữ cái&nbsp;<code>&#39;A&#39;</code>&nbsp;và&nbsp;<code>&#39;B&#39;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Một nước đi xóa một quân cờ nằm giữa hai quân cùng màu; các đoạn liên tiếp của hai người chơi không ảnh hưởng lẫn nhau. Với $n \le 10^5$, ta chỉ cần so sánh số nước đi.
>
> Một đoạn liên tiếp có độ dài $\ell$ tạo ra $\max(\ell-2,0)$ nước đi. Alice thắng khi tổng số nước đi của cô ấy lớn hơn Bob.

<!-- thinking:end -->

Ta đếm số bộ ba ký tự liên tiếp trong chuỗi `colors` đều là `'A'` hoặc đều là `'B'`, lần lượt ký hiệu là $a$ và $b$.

Cuối cùng, ta kiểm tra xem $a$ có lớn hơn $b$ hay không. Nếu có, ta trả về `true`. Nếu không, ta trả về `false`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi `colors`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def winnerOfGame(self, colors: str) -> bool:
        a = b = 0
        for c, v in groupby(colors):
            m = len(list(v)) - 2
            if m > 0 and c == 'A':
                a += m
            elif m > 0 and c == 'B':
                b += m
        return a > b
```

#### Java

```java
class Solution {
    public boolean winnerOfGame(String colors) {
        int n = colors.length();
        int a = 0, b = 0;
        for (int i = 0, j = 0; i < n; i = j) {
            while (j < n && colors.charAt(j) == colors.charAt(i)) {
                ++j;
            }
            int m = j - i - 2;
            if (m > 0) {
                if (colors.charAt(i) == 'A') {
                    a += m;
                } else {
                    b += m;
                }
            }
        }
        return a > b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool winnerOfGame(string colors) {
        int n = colors.size();
        int a = 0, b = 0;
        for (int i = 0, j = 0; i < n; i = j) {
            while (j < n && colors[j] == colors[i]) {
                ++j;
            }
            int m = j - i - 2;
            if (m > 0) {
                if (colors[i] == 'A') {
                    a += m;
                } else {
                    b += m;
                }
            }
        }
        return a > b;
    }
};
```

#### Go

```go
func winnerOfGame(colors string) bool {
	n := len(colors)
	a, b := 0, 0
	for i, j := 0, 0; i < n; i = j {
		for j < n && colors[j] == colors[i] {
			j++
		}
		m := j - i - 2
		if m > 0 {
			if colors[i] == 'A' {
				a += m
			} else {
				b += m
			}
		}
	}
	return a > b
}
```

#### TypeScript

```ts
function winnerOfGame(colors: string): boolean {
    const n = colors.length;
    let [a, b] = [0, 0];
    for (let i = 0, j = 0; i < n; i = j) {
        while (j < n && colors[j] === colors[i]) {
            ++j;
        }
        const m = j - i - 2;
        if (m > 0) {
            if (colors[i] === 'A') {
                a += m;
            } else {
                b += m;
            }
        }
    }
    return a > b;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
