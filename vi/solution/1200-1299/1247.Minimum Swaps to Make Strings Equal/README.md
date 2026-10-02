---
comments: true
difficulty: Medium
rating: 1597
source: Weekly Contest 161 Q1
tags:
    - Greedy
    - Math
    - String
---

<!-- problem:start -->

# [1247. Minimum Swaps to Make Strings Equal](https://leetcode.com/problems/minimum-swaps-to-make-strings-equal)

[中文文档](/solution/1200-1299/1247.Minimum%20Swaps%20to%20Make%20Strings%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s1</code> và <code>s2</code> có cùng độ dài, chỉ gồm các chữ cái <code>&quot;x&quot;</code> và <code>&quot;y&quot;</code>. Nhiệm vụ của bạn là làm cho hai chuỗi bằng nhau. Bạn có thể hoán đổi hai ký tự bất kỳ thuộc <strong>hai</strong> chuỗi <strong>khác nhau</strong>, tức là hoán đổi <code>s1[i]</code> và <code>s2[j]</code>.</p>

<p>Trả về số lần hoán đổi ít nhất cần thiết để làm cho <code>s1</code> và <code>s2</code> bằng nhau; nếu không thể, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;xx&quot;, s2 = &quot;yy&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Hoán đổi s1[0] và s2[1], ta được s1 = &quot;yx&quot;, s2 = &quot;yx&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;xy&quot;, s2 = &quot;yx&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hoán đổi s1[0] và s2[0], ta được s1 = &quot;yy&quot;, s2 = &quot;xx&quot;.
Hoán đổi s1[0] và s2[1], ta được s1 = &quot;xy&quot;, s2 = &quot;xy&quot;.
Lưu ý, bạn không thể hoán đổi s1[0] và s1[1] để biến s1 thành &quot;yx&quot;, vì chỉ được hoán đổi ký tự giữa hai chuỗi khác nhau.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;xx&quot;, s2 = &quot;xy&quot;
<strong>Đầu ra:</strong> -1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s1.length, s2.length &lt;= 1000</code></li>
	<li><code>s1.length == s2.length</code></li>
	<li><code>s1, s2</code> chỉ chứa <code>&#39;x&#39;</code> hoặc <code>&#39;y&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Các vị trí đã khớp không cần hoán đổi. Các cặp lệch chỉ có dạng $xy$ hoặc $yx$. Hai cặp lệch cùng dạng chỉ cần một lần hoán đổi; một cặp mỗi dạng cần hai lần; nếu tổng số cặp lệch là số lẻ thì không thể giải được.
>
> Ta đếm số cặp của mỗi dạng, loại trường hợp tổng là số lẻ; nếu không, lấy một nửa mỗi số đếm và cộng thêm tối đa một lần hoán đổi giữa hai dạng. Cách ghép không phụ thuộc vào vị trí.

<!-- thinking:end -->

Theo đề bài, hai chuỗi $s_1$ và $s_2$ chỉ chứa ký tự $x$ và $y$, đồng thời có cùng độ dài. Vì vậy, ta xét từng cặp ký tự tương ứng $s_1[i]$ và $s_2[i]$.

Nếu $s_1[i] = s_2[i]$, không cần hoán đổi và ta chuyển sang ký tự tiếp theo. Nếu $s_1[i] \neq s_2[i]$, cần xử lý cặp này. Ta đếm từng dạng: nếu $s_1[i] = x$ và $s_2[i] = y$, ký hiệu là $xy$; nếu $s_1[i] = y$ và $s_2[i] = x$, ký hiệu là $yx$.

Nếu $xy + yx$ là số lẻ, không thể hoàn tất việc hoán đổi và ta trả về $-1$. Nếu $xy + yx$ là số chẵn, số lần hoán đổi cần thiết là $\left \lfloor \frac{xy}{2} \right \rfloor + \left \lfloor \frac{yx}{2} \right \rfloor + xy \bmod{2} + yx \bmod{2}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài hai chuỗi $s_1$ và $s_2$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSwap(self, s1: str, s2: str) -> int:
        xy = yx = 0
        for a, b in zip(s1, s2):
            xy += a < b
            yx += a > b
        if (xy + yx) % 2:
            return -1
        return xy // 2 + yx // 2 + xy % 2 + yx % 2
```

#### Java

```java
class Solution {
    public int minimumSwap(String s1, String s2) {
        int xy = 0, yx = 0;
        for (int i = 0; i < s1.length(); ++i) {
            char a = s1.charAt(i), b = s2.charAt(i);
            if (a < b) {
                ++xy;
            }
            if (a > b) {
                ++yx;
            }
        }
        if ((xy + yx) % 2 == 1) {
            return -1;
        }
        return xy / 2 + yx / 2 + xy % 2 + yx % 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSwap(string s1, string s2) {
        int xy = 0, yx = 0;
        for (int i = 0; i < s1.size(); ++i) {
            char a = s1[i], b = s2[i];
            xy += a < b;
            yx += a > b;
        }
        if ((xy + yx) % 2) {
            return -1;
        }
        return xy / 2 + yx / 2 + xy % 2 + yx % 2;
    }
};
```

#### Go

```go
func minimumSwap(s1 string, s2 string) int {
	xy, yx := 0, 0
	for i := range s1 {
		if s1[i] < s2[i] {
			xy++
		}
		if s1[i] > s2[i] {
			yx++
		}
	}
	if (xy+yx)%2 == 1 {
		return -1
	}
	return xy/2 + yx/2 + xy%2 + yx%2
}
```

#### TypeScript

```ts
function minimumSwap(s1: string, s2: string): number {
    let xy = 0,
        yx = 0;

    for (let i = 0; i < s1.length; ++i) {
        const a = s1[i],
            b = s2[i];
        xy += a < b ? 1 : 0;
        yx += a > b ? 1 : 0;
    }

    if ((xy + yx) % 2 !== 0) {
        return -1;
    }

    return Math.floor(xy / 2) + Math.floor(yx / 2) + (xy % 2) + (yx % 2);
}
```

#### JavaScript

```js
var minimumSwap = function (s1, s2) {
    let xy = 0,
        yx = 0;
    for (let i = 0; i < s1.length; ++i) {
        const a = s1[i],
            b = s2[i];
        if (a < b) {
            ++xy;
        }
        if (a > b) {
            ++yx;
        }
    }
    if ((xy + yx) % 2 === 1) {
        return -1;
    }
    return Math.floor(xy / 2) + Math.floor(yx / 2) + (xy % 2) + (yx % 2);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
