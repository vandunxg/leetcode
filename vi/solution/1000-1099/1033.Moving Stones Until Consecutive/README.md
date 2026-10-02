---
comments: true
difficulty: Medium
rating: 1421
source: Weekly Contest 134 Q1
tags:
    - Brainteaser
    - Math
---

<!-- problem:start -->

# [1033. Moving Stones Until Consecutive](https://leetcode.com/problems/moving-stones-until-consecutive)

[中文文档](/solution/1000-1099/1033.Moving%20Stones%20Until%20Consecutive/README.md)

## Mô tả

<!-- description:start -->

<p>Có ba viên đá ở ba vị trí khác nhau trên trục X. Cho ba số nguyên <code>a</code>, <code>b</code> và <code>c</code> là vị trí của các viên đá.</p>

<p>Trong một lượt, bạn nhấc viên đá ở một đầu mút (tức viên đá ở vị trí thấp nhất hoặc cao nhất), rồi chuyển nó đến một vị trí còn trống nằm giữa hai đầu mút đó. Cụ thể, giả sử các viên đá hiện ở các vị trí <code>x</code>, <code>y</code> và <code>z</code>, với <code>x &lt; y &lt; z</code>. Bạn nhấc viên đá ở vị trí <code>x</code> hoặc <code>z</code> và chuyển đến vị trí nguyên <code>k</code>, sao cho <code>x &lt; k &lt; z</code> và <code>k != y</code>.</p>

<p>Trò chơi kết thúc khi bạn không thể thực hiện thêm lượt nào (tức là ba viên đá nằm ở ba vị trí liên tiếp).</p>

<p>Hãy trả về <em>mảng số nguyên </em><code>answer</code><em> có độ dài </em><code>2</code><em>, trong đó</em>:</p>

<ul>
	<li><code>answer[0]</code> <em>là số lượt ít nhất bạn có thể thực hiện, còn</em></li>
	<li><code>answer[1]</code> <em>là số lượt nhiều nhất bạn có thể thực hiện</em>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 1, b = 2, c = 5
<strong>Đầu ra:</strong> [1,2]
<strong>Giải thích:</strong> Chuyển viên đá ở vị trí 5 đến vị trí 3, hoặc chuyển viên đá ở vị trí 5 đến vị trí 4 rồi đến vị trí 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 4, b = 3, c = 2
<strong>Đầu ra:</strong> [0,0]
<strong>Giải thích:</strong> Ta không thể thực hiện lượt nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 3, b = 5, c = 1
<strong>Đầu ra:</strong> [1,2]
<strong>Giải thích:</strong> Chuyển viên đá ở vị trí 1 đến vị trí 4; hoặc chuyển viên đá ở vị trí 1 đến vị trí 2 rồi đến vị trí 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= a, b, c &lt;= 100</code></li>
	<li><code>a</code>, <code>b</code> và <code>c</code> có các giá trị khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có thể thử các vị trí khác nhau, nhưng kết quả tối ưu chỉ phụ thuộc vào các khoảng cách sau khi sắp xếp, nên không cần mô phỏng.
>
> Gọi $x<y<z$. Nếu các viên đá đã ở ba vị trí liên tiếp thì không cần lượt nào. Nếu khoảng cách giữa $y$ và một đầu mút không quá $2$, chỉ cần một lượt để lấp khoảng trống; nếu không, cần di chuyển cả hai đầu mút một lần, nên số lượt ít nhất là $2$. Số lượt nhiều nhất đạt được bằng cách liên tục chuyển các viên đá ở hai đầu vào những vị trí trống bên trong, tổng cộng $z-x-2$ lượt.
>
> Sau khi sắp xếp, dựa vào các trường hợp trên ta trả về $[\textit{mi},\textit{mx}]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numMovesStones(self, a: int, b: int, c: int) -> List[int]:
        x, z = min(a, b, c), max(a, b, c)
        y = a + b + c - x - z
        mi = mx = 0
        if z - x > 2:
            mi = 1 if y - x < 3 or z - y < 3 else 2
            mx = z - x - 2
        return [mi, mx]
```

#### Java

```java
class Solution {
    public int[] numMovesStones(int a, int b, int c) {
        int x = Math.min(a, Math.min(b, c));
        int z = Math.max(a, Math.max(b, c));
        int y = a + b + c - x - z;
        int mi = 0, mx = 0;
        if (z - x > 2) {
            mi = y - x < 3 || z - y < 3 ? 1 : 2;
            mx = z - x - 2;
        }
        return new int[] {mi, mx};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> numMovesStones(int a, int b, int c) {
        int x = min({a, b, c});
        int z = max({a, b, c});
        int y = a + b + c - x - z;
        int mi = 0, mx = 0;
        if (z - x > 2) {
            mi = y - x < 3 || z - y < 3 ? 1 : 2;
            mx = z - x - 2;
        }
        return {mi, mx};
    }
};
```

#### Go

```go
func numMovesStones(a int, b int, c int) []int {
	x := min(a, min(b, c))
	z := max(a, max(b, c))
	y := a + b + c - x - z
	mi, mx := 0, 0
	if z-x > 2 {
		mi = 2
		if y-x < 3 || z-y < 3 {
			mi = 1
		}
		mx = z - x - 2
	}
	return []int{mi, mx}
}
```

#### TypeScript

```ts
function numMovesStones(a: number, b: number, c: number): number[] {
    const x = Math.min(a, Math.min(b, c));
    const z = Math.max(a, Math.max(b, c));
    const y = a + b + c - x - z;
    let mi = 0,
        mx = 0;
    if (z - x > 2) {
        mi = y - x < 3 || z - y < 3 ? 1 : 2;
        mx = z - x - 2;
    }
    return [mi, mx];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
