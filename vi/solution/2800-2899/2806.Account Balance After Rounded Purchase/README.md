---
comments: true
difficulty: Easy
rating: 1214
source: Biweekly Contest 110 Q1
tags:
    - Math
---

<!-- problem:start -->

# [2806. Account Balance After Rounded Purchase](https://leetcode.com/problems/account-balance-after-rounded-purchase)

[中文文档](/solution/2800-2899/2806.Account%20Balance%20After%20Rounded%20Purchase/README.md)

## Mô tả

<!-- description:start -->

<p>Ban đầu, số dư tài khoản ngân hàng của bạn là <strong>100</strong> đô la.</p>

<p>Cho một số nguyên <code>purchaseAmount</code> biểu thị số tiền bạn sẽ chi cho một giao dịch mua, hay nói cách khác là giá của món hàng.</p>

<p>Khi thực hiện giao dịch mua, trước hết <code>purchaseAmount</code> được <strong>làm tròn đến bội số gần nhất của 10</strong>. Gọi giá trị này là <code>roundedAmount</code>. Sau đó, <code>roundedAmount</code> đô la được trừ khỏi tài khoản ngân hàng của bạn.</p>

<p>Trả về một số nguyên biểu thị số dư tài khoản ngân hàng cuối cùng sau giao dịch mua này.</p>

<p><strong>Ghi chú:</strong></p>

<ul>
	<li>Trong bài toán này, 0 được coi là một bội số của 10.</li>
	<li>Khi làm tròn, 5 được làm tròn lên (5 được làm tròn thành 10, 15 thành 20, 25 thành 30, v.v.).</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">purchaseAmount = 9</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">90</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bội số của 10 gần 9 nhất là 10. Vì vậy, số dư tài khoản của bạn trở thành 100 - 10 = 90.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">purchaseAmount = 15</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">80</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bội số của 10 gần 15 nhất là 20. Vì vậy, số dư tài khoản của bạn trở thành 100 - 20 = 80.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">purchaseAmount = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">90</span></p>

<p><strong>Giải thích:</strong></p>

<p>10 tự nó đã là một bội số của 10. Vì vậy, số dư tài khoản của bạn trở thành 100 - 10 = 90.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= purchaseAmount &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Số tiền mua nằm trong $[0,100]$ và phải được làm tròn đến bội số gần nhất của $10$ (chọn bội số lớn hơn khi khoảng cách bằng nhau); số dư bằng $100$ trừ đi bội số đó. Có thể liệt kê 11 ứng viên. Duyệt từ $100$ xuống và chỉ cập nhật khi khoảng cách nhỏ hơn sẽ giữ lại bội số lớn hơn khi hai khoảng cách bằng nhau.

<!-- thinking:end -->

Ta liệt kê tất cả các bội số của 10 trong khoảng $[0, 100]$ và tìm bội số gần `purchaseAmount` nhất, gọi là $x$. Đáp án là $100 - x$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def accountBalanceAfterPurchase(self, purchaseAmount: int) -> int:
        diff, x = 100, 0
        for y in range(100, -1, -10):
            if (t := abs(y - purchaseAmount)) < diff:
                diff = t
                x = y
        return 100 - x
```

#### Java

```java
class Solution {
    public int accountBalanceAfterPurchase(int purchaseAmount) {
        int diff = 100, x = 0;
        for (int y = 100; y >= 0; y -= 10) {
            int t = Math.abs(y - purchaseAmount);
            if (t < diff) {
                diff = t;
                x = y;
            }
        }
        return 100 - x;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int accountBalanceAfterPurchase(int purchaseAmount) {
        int diff = 100, x = 0;
        for (int y = 100; y >= 0; y -= 10) {
            int t = abs(y - purchaseAmount);
            if (t < diff) {
                diff = t;
                x = y;
            }
        }
        return 100 - x;
    }
};
```

#### Go

```go
func accountBalanceAfterPurchase(purchaseAmount int) int {
	diff, x := 100, 0
	for y := 100; y >= 0; y -= 10 {
		t := abs(y - purchaseAmount)
		if t < diff {
			diff = t
			x = y
		}
	}
	return 100 - x
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function accountBalanceAfterPurchase(purchaseAmount: number): number {
    let [diff, x] = [100, 0];
    for (let y = 100; y >= 0; y -= 10) {
        const t = Math.abs(y - purchaseAmount);
        if (t < diff) {
            diff = t;
            x = y;
        }
    }
    return 100 - x;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
