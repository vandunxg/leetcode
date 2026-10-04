---
comments: true
difficulty: Hard
rating: 2220
source: Biweekly Contest 96 Q4
tags:
    - Math
    - Greatest Common Divisor
    - Number Theory
    - Euclidean Algorithm
---

<!-- problem:start -->

# [2543. Check if Point Is Reachable](https://leetcode.com/problems/check-if-point-is-reachable)

[中文文档](/solution/2500-2599/2543.Check%20if%20Point%20Is%20Reachable/README.md)

## Mô tả

<!-- description:start -->

<p>Có một lưới vô hạn. Ban đầu bạn đang ở điểm <code>(1, 1)</code>, và cần đi đến điểm <code>(targetX, targetY)</code> bằng một số hữu hạn bước.</p>

<p>Trong một <strong>bước</strong>, bạn có thể di chuyển từ điểm <code>(x, y)</code> đến một trong các điểm sau:</p>

<ul>
	<li><code>(x, y - x)</code></li>
	<li><code>(x - y, y)</code></li>
	<li><code>(2 * x, y)</code></li>
	<li><code>(x, 2 * y)</code></li>
</ul>

<p>Cho hai số nguyên <code>targetX</code> và <code>targetY</code> lần lượt biểu diễn tọa độ X và Y của vị trí cuối cùng, hãy trả về <code>true</code> <em>nếu có thể đi từ</em> <code>(1, 1)</code> <em>đến điểm đó bằng một số bước bất kỳ, và trả về </em><code>false</code><em> nếu không thể</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> targetX = 6, targetY = 9
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể đi từ (1,1) đến (6,9) bằng bất kỳ chuỗi bước nào, nên kết quả trả về là false.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> targetX = 4, targetY = 7
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Bạn có thể đi theo đường đi (1,1) -&gt; (1,2) -&gt; (1,4) -&gt; (1,8) -&gt; (1,7) -&gt; (2,7) -&gt; (4,7).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= targetX, targetY&nbsp;&lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mathematics

<!-- thinking:start -->

> **Tư duy**
>
> Từ $(1,1)$, ta có thể đi đến $(x+y,y)$, $(x,x+y)$ hoặc nhân đôi một tọa độ. Vì các tọa độ có thể đạt tới $10^9$, việc tìm kiếm là không thể thực hiện được.
>
> Hai bước đầu tiên bảo toàn $\gcd$; phép nhân đôi chỉ nhân gcd với một lũy thừa của hai. Vì vậy, gcd của đích phải là một lũy thừa của hai. Ngược lại, bằng cách liên tục chia các thừa số hai và thay tọa độ lẻ lớn hơn bằng $(x+y)/2$, ta có thể thu gọn mọi cặp như vậy về $(1,1)$. Do đó, chỉ cần kiểm tra $x\&(x-1)=0$ trên $\gcd(x,y)$.

<!-- thinking:end -->

Ta nhận thấy hai loại bước đầu tiên không làm thay đổi ước chung lớn nhất (gcd) của tọa độ ngang và dọc, còn hai loại bước cuối cùng có thể nhân gcd của hai tọa độ với một lũy thừa của $2$. Nói cách khác, gcd cuối cùng của hai tọa độ phải là một lũy thừa của $2$. Nếu gcd không phải là một lũy thừa của $2$ thì không thể đi đến điểm đó.

Tiếp theo, ta chứng minh rằng mọi $(x, y)$ thỏa mãn $gcd(x, y)=2^k$ đều có thể đạt tới.

Ta đảo ngược hướng di chuyển, tức là đi từ điểm cuối trở về. Khi đó $(x, y)$ có thể di chuyển đến $(x, x+y)$, $(x+y, y)$, $(\frac{x}{2}, y)$ và $(x, \frac{y}{2})$.

Chừng nào $x$ hoặc $y$ còn chẵn, ta chia nó cho $2$ cho đến khi cả $x$ và $y$ đều lẻ. Lúc này, nếu $x \neq y$, không mất tính tổng quát, giả sử $x \gt y$, khi đó $\frac{x+y}{2} \lt x$. Vì $x+y$ là số chẵn, ta có thể di chuyển từ $(x, y)$ đến $(x+y, y)$, sau đó đến $(\frac{x+y}{2}, y)$ qua các phép biến đổi. Điều đó có nghĩa là ta luôn có thể làm cho $x$ và $y$ liên tục giảm. Khi vòng lặp kết thúc, nếu $x=y=1$ thì điểm đó có thể đạt tới.

Độ phức tạp thời gian là $O(\log(\min(targetX, targetY)))$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isReachable(self, targetX: int, targetY: int) -> bool:
        x = gcd(targetX, targetY)
        return x & (x - 1) == 0
```

#### Java

```java
class Solution {
    public boolean isReachable(int targetX, int targetY) {
        int x = gcd(targetX, targetY);
        return (x & (x - 1)) == 0;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isReachable(int targetX, int targetY) {
        int x = gcd(targetX, targetY);
        return (x & (x - 1)) == 0;
    }
};
```

#### Go

```go
func isReachable(targetX int, targetY int) bool {
	x := gcd(targetX, targetY)
	return x&(x-1) == 0
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### TypeScript

```ts
function isReachable(targetX: number, targetY: number): boolean {
    const x = gcd(targetX, targetY);
    return (x & (x - 1)) === 0;
}

function gcd(a: number, b: number): number {
    return b == 0 ? a : gcd(b, a % b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
