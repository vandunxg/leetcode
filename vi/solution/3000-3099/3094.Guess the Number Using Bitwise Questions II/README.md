---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Interactive
---

<!-- problem:start -->

# [3094. Guess the Number Using Bitwise Questions II 🔒](https://leetcode.com/problems/guess-the-number-using-bitwise-questions-ii)

[中文文档](/solution/3000-3099/3094.Guess%20the%20Number%20Using%20Bitwise%20Questions%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Có một số <code>n</code> nằm trong khoảng từ <code>0</code> đến <code>2<sup>30</sup> - 1</code> (bao gồm cả hai đầu) mà bạn cần tìm.</p>

<p>Có một API được định nghĩa sẵn <code>int commonBits(int num)</code> hỗ trợ bạn thực hiện nhiệm vụ này. Tuy nhiên, thách thức là mỗi lần bạn gọi hàm này, <code>n</code> sẽ thay đổi theo một cách nào đó. Nhưng cần nhớ rằng bạn phải tìm <strong>giá trị ban đầu của </strong><code>n</code>.</p>

<p><code>commonBits(int num)</code> hoạt động như sau:</p>

<ul>
	<li>Tính <code>count</code>, là số lượng bit mà cả <code>n</code> và <code>num</code> có cùng giá trị tại vị trí đó trong biểu diễn nhị phân.</li>
	<li><code>n = n XOR num</code></li>
	<li>Trả về <code>count</code>.</li>
</ul>

<p>Trả về <em>số</em> <code>n</code>.</p>

<p><strong>Lưu ý:</strong> Trong thế giới này, mọi số đều nằm trong khoảng từ <code>0</code> đến <code>2<sup>30</sup> - 1</code> (bao gồm cả hai đầu), do đó khi đếm các bit chung, ta chỉ xét 30 bit đầu tiên của các số đó.</p>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 2<sup>30</sup> - 1</code></li>
	<li><code>0 &lt;= num &lt;= 2<sup>30</sup> - 1</code></li>
	<li>Nếu bạn hỏi một <code>num</code> nằm ngoài khoảng đã cho, kết quả sẽ không đáng tin cậy.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> $\texttt{commonBits}(x)$ thực hiện XOR giữa $n$ và $x$, sau đó trả về số lượng bit $1$ chung. Mỗi truy vấn đều làm thay đổi $n$ vĩnh viễn.
>
> Gọi cùng một $x$ hai lần sẽ khôi phục $n$. So sánh hai câu trả lời cho biết bit đó ban đầu có phải là $1$ hay không: nếu kết quả ở lần gọi thứ hai giảm, bit đó ban đầu là $1$.
>
> Với mỗi bit, ta gọi $1\ll i$ hai lần và đặt bit đó nếu số đếm ở lần đầu lớn hơn.

<!-- thinking:end -->

Dựa trên mô tả bài toán, ta nhận thấy:

- Nếu gọi hàm `commonBits` hai lần với cùng một số, giá trị của $n$ sẽ không thay đổi.
- Nếu gọi `commonBits(1 << i)`, bit thứ $i$ của $n$ sẽ bị đảo, nghĩa là nếu bit thứ $i$ của $n$ là $1$, nó sẽ trở thành $0$ sau lần gọi và ngược lại.

Do đó, với mỗi bit $i$, ta có thể gọi `commonBits(1 << i)` hai lần, lần lượt ký hiệu là `count1` và `count2`. Nếu `count1 > count2`, điều đó có nghĩa là bit thứ $i$ của $n$ là $1$, ngược lại là $0$.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition of commonBits API.
# def commonBits(num: int) -> int:


class Solution:
    def findNumber(self) -> int:
        n = 0
        for i in range(32):
            count1 = commonBits(1 << i)
            count2 = commonBits(1 << i)
            if count1 > count2:
                n |= 1 << i
        return n
```

#### Java

```java
/**
 * Definition of commonBits API (defined in the parent class Problem).
 * int commonBits(int num);
 */

public class Solution extends Problem {
    public int findNumber() {
        int n = 0;
        for (int i = 0; i < 32; ++i) {
            int count1 = commonBits(1 << i);
            int count2 = commonBits(1 << i);
            if (count1 > count2) {
                n |= 1 << i;
            }
        }
        return n;
    }
}
```

#### C++

```cpp
/**
 * Definition of commonBits API.
 * int commonBits(int num);
 */

class Solution {
public:
    int findNumber() {
        int n = 0;
        for (int i = 0; i < 32; ++i) {
            int count1 = commonBits(1 << i);
            int count2 = commonBits(1 << i);
            if (count1 > count2) {
                n |= 1 << i;
            }
        }
        return n;
    }
};
```

#### Go

```go
/**
 * Definition of commonBits API.
 * func commonBits(num int) int;
 */

func findNumber() (n int) {
	for i := 0; i < 32; i++ {
		count1 := commonBits(1 << i)
		count2 := commonBits(1 << i)
		if count1 > count2 {
			n |= 1 << i
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
