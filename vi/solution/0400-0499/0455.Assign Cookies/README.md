---
comments: true
difficulty: Easy
tags:
    - Greedy
    - Array
    - Two Pointers
    - Sorting
    - Quick Sort
---

<!-- problem:start -->

# [455. Assign Cookies](https://leetcode.com/problems/assign-cookies)

[中文文档](/solution/0400-0499/0455.Assign%20Cookies/README.md)

## Mô tả

<!-- description:start -->

<p>Giả sử bạn là một phụ huynh tuyệt vời và muốn phát bánh quy cho các con. Tuy nhiên, mỗi đứa trẻ chỉ được nhận tối đa một chiếc.</p>

<p>Mỗi đứa trẻ <code>i</code> có mức độ tham ăn <code>g[i]</code>, tức kích thước bánh quy tối thiểu để trẻ hài lòng; mỗi chiếc bánh quy <code>j</code> có kích thước <code>s[j]</code>. Nếu <code>s[j] &gt;= g[i]</code>, ta có thể phát bánh quy <code>j</code> cho trẻ <code>i</code> để trẻ hài lòng. Mục tiêu là tối đa hóa và trả về số trẻ được hài lòng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> g = [1,2,3], s = [1,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có 3 đứa trẻ và 2 chiếc bánh quy. Mức độ tham ăn của 3 đứa trẻ lần lượt là 1, 2, 3. 
Dù có 2 chiếc bánh quy, cả hai đều chỉ có kích thước 1, nên chỉ có thể làm hài lòng đứa trẻ có mức độ tham ăn bằng 1.
Kết quả cần trả về là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> g = [1,2], s = [1,2,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 2 đứa trẻ và 3 chiếc bánh quy. Mức độ tham ăn của hai đứa trẻ lần lượt là 1 và 2. 
Cả 3 chiếc bánh quy đều đủ lớn để làm hài lòng tất cả trẻ. Kết quả cần trả về là 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= g.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= s.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= g[i], s[j] &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài toán này giống với <a href="https://leetcode.com/problems/maximum-matching-of-players-with-trainers/description/" target="_blank"> 2410: Maximum Matching of Players With Trainers.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chiếc bánh chỉ phát cho tối đa một trẻ và ta muốn làm hài lòng nhiều trẻ nhất có thể. Nên dành bánh nhỏ cho trẻ có mức độ tham ăn thấp; nếu không, bánh lớn có thể bị dùng lãng phí.
>
> Sắp xếp cả hai mảng. Con trỏ bánh quy bỏ qua các kích thước quá nhỏ; khi gặp bánh đủ lớn thì con trỏ trẻ và con trỏ bánh cùng tiến lên. Nếu hết bánh, số trẻ đã được phát bánh chính là đáp án.
>
> Sau khi sắp xếp, hai con trỏ luôn thử chiếc bánh hiện tại với đứa trẻ dễ hài lòng nhất trong số còn lại.

<!-- thinking:end -->

Theo đề bài, nên ưu tiên phát bánh quy cho trẻ có mức độ tham ăn thấp hơn để làm hài lòng nhiều trẻ nhất có thể.

Vì vậy, trước tiên ta sắp xếp hai mảng, rồi dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến đầu mảng $g$ và $s$. Mỗi lần, ta so sánh giá trị $g[i]$ và $s[j]$:

- Nếu $s[j] < g[i]$, chiếc bánh hiện tại không thể làm hài lòng đứa trẻ này. Ta cần tìm chiếc bánh lớn hơn nên tăng $j$ lên một. Nếu $j$ vượt quá giới hạn, đứa trẻ hiện tại không thể được làm hài lòng; khi đó, số trẻ đã được phát bánh thành công là $i$, có thể trả về ngay.
- Nếu $s[j] \ge g[i]$, chiếc bánh hiện tại đủ để làm hài lòng đứa trẻ này. Ta phát chiếc bánh đó cho trẻ, rồi tăng cả $i$ và $j$ lên một.

Nếu đã duyệt hết mảng $g$, nghĩa là tất cả trẻ đều đã được phát bánh; ta trả về tổng số trẻ.

Độ phức tạp thời gian là $O(m \times \log m + n \times \log n)$, độ phức tạp không gian là $O(\log m + \log n)$, trong đó $m$ và $n$ lần lượt là độ dài của mảng $g$ và $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()
        j = 0
        for i, x in enumerate(g):
            while j < len(s) and s[j] < g[i]:
                j += 1
            if j >= len(s):
                return i
            j += 1
        return len(g)
```

#### Java

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int m = g.length;
        int n = s.length;
        for (int i = 0, j = 0; i < m; ++i) {
            while (j < n && s[j] < g[i]) {
                ++j;
            }
            if (j++ >= n) {
                return i;
            }
        }
        return m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int m = g.size(), n = s.size();
        for (int i = 0, j = 0; i < m; ++i) {
            while (j < n && s[j] < g[i]) {
                ++j;
            }
            if (j++ >= n) {
                return i;
            }
        }
        return m;
    }
};
```

#### Go

```go
func findContentChildren(g []int, s []int) int {
	sort.Ints(g)
	sort.Ints(s)
	j := 0
	for i, x := range g {
		for j < len(s) && s[j] < x {
			j++
		}
		if j >= len(s) {
			return i
		}
		j++
	}
	return len(g)
}
```

#### TypeScript

```ts
function findContentChildren(g: number[], s: number[]): number {
    g.sort((a, b) => a - b);
    s.sort((a, b) => a - b);
    const m = g.length;
    const n = s.length;
    for (let i = 0, j = 0; i < m; ++i) {
        while (j < n && s[j] < g[i]) {
            ++j;
        }
        if (j++ >= n) {
            return i;
        }
    }
    return m;
}
```

#### JavaScript

```js
/**
 * @param {number[]} g
 * @param {number[]} s
 * @return {number}
 */
var findContentChildren = function (g, s) {
    g.sort((a, b) => a - b);
    s.sort((a, b) => a - b);
    const m = g.length;
    const n = s.length;
    for (let i = 0, j = 0; i < m; ++i) {
        while (j < n && s[j] < g[i]) {
            ++j;
        }
        if (j++ >= n) {
            return i;
        }
    }
    return m;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
