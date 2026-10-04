---
comments: true
difficulty: Easy
rating: 1530
source: Biweekly Contest 100 Q1
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [2591. Distribute Money to Maximum Children](https://leetcode.com/problems/distribute-money-to-maximum-children)

[中文文档](/solution/2500-2599/2591.Distribute%20Money%20to%20Maximum%20Children/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>money</code> biểu thị số tiền (tính bằng đô la) bạn có và một số nguyên khác <code>children</code> biểu thị số trẻ em mà bạn phải chia tiền cho.</p>

<p>Bạn phải chia tiền theo các quy tắc sau:</p>

<ul>
	<li>Phải chia hết toàn bộ số tiền.</li>
	<li>Mỗi người phải nhận ít nhất <code>1</code> đô la.</li>
	<li>Không ai được nhận <code>4</code> đô la.</li>
</ul>

<p>Trả về <em>số lượng <strong>lớn nhất</strong> trẻ em có thể nhận <strong>chính xác</strong> </em><code>8</code> <em>đô la nếu bạn chia tiền theo các quy tắc trên</em>. Nếu không thể chia tiền, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> money = 20, children = 3
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Số trẻ nhận 8 đô la nhiều nhất là 1. Một cách chia tiền là:
- 8 đô la cho đứa trẻ đầu tiên.
- 9 đô la cho đứa trẻ thứ hai.
- 3 đô la cho đứa trẻ thứ ba.
Có thể chứng minh rằng không có cách chia nào để số trẻ nhận 8 đô la lớn hơn 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> money = 16, children = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có thể chia cho mỗi đứa trẻ 8 đô la.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= money &lt;= 200</code></li>
	<li><code>2 &lt;= children &lt;= 30</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi người nhận ít nhất $1$, cố gắng để nhiều người nhất nhận chính xác $8$, và không ai được nhận $4$. Nếu chia chưa đến một đô la cho mỗi đứa trẻ thì điều đó là không thể.
>
> Nếu tổng tiền lớn hơn $8\times \textit{children}$, chắc chắn có người nhận nhiều hơn $8$, nên nhiều nhất chỉ có $\textit{children}-1$ người nhận 8. Nếu tổng tiền đúng bằng $8n-4$, việc để $n-1$ người nhận $8$ sẽ khiến một người nhận $4$, nên phải giảm thêm một người. Trong các trường hợp khác, trước tiên hãy chia cho mỗi người $1$ đô la; mỗi $7$ đô la còn lại sẽ tạo thêm một người nhận 8, tức là $\lfloor(\textit{money}-\textit{children})/7\rfloor$.

<!-- thinking:end -->

Nếu $money \lt children$, chắc chắn sẽ có một đứa trẻ không nhận được tiền, trả về $-1$.

Nếu $money \gt 8 \times children$, có $children-1$ đứa trẻ nhận $8$ đô la, còn đứa trẻ cuối cùng nhận $money - 8 \times (children-1)$ đô la, trả về $children-1$.

Nếu $money = 8 \times children - 4$, có $children-2$ đứa trẻ nhận $8$ đô la, còn hai đứa trẻ còn lại chia nhau $12$ đô la (miễn là không ai nhận $4$, nhận $8$ đô la là được), trả về $children-2$.

Giả sử có $x$ đứa trẻ nhận $8$ đô la, số tiền còn lại là $money- 8 \times x$. Miễn là số tiền này lớn hơn hoặc bằng số trẻ còn lại $children-x$, các điều kiện đều được đáp ứng. Vì vậy, ta chỉ cần tìm giá trị lớn nhất của $x$, đó chính là đáp án.

Độ phức tạp thời gian là $O(1)$, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distMoney(self, money: int, children: int) -> int:
        if money < children:
            return -1
        if money > 8 * children:
            return children - 1
        if money == 8 * children - 4:
            return children - 2
        # money-8x >= children-x, x <= (money-children)/7
        return (money - children) // 7
```

#### Java

```java
class Solution {
    public int distMoney(int money, int children) {
        if (money < children) {
            return -1;
        }
        if (money > 8 * children) {
            return children - 1;
        }
        if (money == 8 * children - 4) {
            return children - 2;
        }
        // money-8x >= children-x, x <= (money-children)/7
        return (money - children) / 7;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int distMoney(int money, int children) {
        if (money < children) {
            return -1;
        }
        if (money > 8 * children) {
            return children - 1;
        }
        if (money == 8 * children - 4) {
            return children - 2;
        }
        // money-8x >= children-x, x <= (money-children)/7
        return (money - children) / 7;
    }
};
```

#### Go

```go
func distMoney(money int, children int) int {
	if money < children {
		return -1
	}
	if money > 8*children {
		return children - 1
	}
	if money == 8*children-4 {
		return children - 2
	}
	// money-8x >= children-x, x <= (money-children)/7
	return (money - children) / 7
}
```

#### TypeScript

```ts
function distMoney(money: number, children: number): number {
    if (money < children) {
        return -1;
    }
    if (money > 8 * children) {
        return children - 1;
    }
    if (money === 8 * children - 4) {
        return children - 2;
    }
    return Math.floor((money - children) / 7);
}
```

#### Rust

```rust
impl Solution {
    pub fn dist_money(money: i32, children: i32) -> i32 {
        if money < children {
            return -1;
        }

        if money > children * 8 {
            return children - 1;
        }

        if money == children * 8 - 4 {
            return children - 2;
        }

        (money - children) / 7
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
