---
comments: true
difficulty: Medium
rating: 1399
source: Biweekly Contest 76 Q2
tags:
    - Math
    - Enumeration
---

<!-- problem:start -->

# [2240. Number of Ways to Buy Pens and Pencils](https://leetcode.com/problems/number-of-ways-to-buy-pens-and-pencils)

[中文文档](/solution/2200-2299/2240.Number%20of%20Ways%20to%20Buy%20Pens%20and%20Pencils/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>total</code> biểu thị số tiền bạn có. Bạn cũng được cho hai số nguyên <code>cost1</code> và <code>cost2</code> lần lượt biểu thị giá của một chiếc bút mực và bút chì. Bạn có thể dùng <strong>một phần hoặc toàn bộ</strong> số tiền để mua nhiều chiếc (hoặc không mua chiếc nào) của mỗi loại dụng cụ viết.</p>

<p>Hãy trả về <em><strong>số cách khác nhau</strong> để bạn có thể mua một số bút mực và bút chì.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> total = 20, cost1 = 10, cost2 = 5
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Giá của một chiếc bút mực là 10 và giá của một chiếc bút chì là 5.
- Nếu mua 0 chiếc bút mực, bạn có thể mua 0, 1, 2, 3 hoặc 4 chiếc bút chì.
- Nếu mua 1 chiếc bút mực, bạn có thể mua 0, 1 hoặc 2 chiếc bút chì.
- Nếu mua 2 chiếc bút mực, bạn không thể mua thêm bút chì.
Tổng số cách mua bút mực và bút chì là 5 + 3 + 1 = 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> total = 5, cost1 = 10, cost2 = 10
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Giá của cả bút mực và bút chì đều là 10, cao hơn total, nên bạn không thể mua dụng cụ viết nào. Vì vậy, chỉ có 1 cách: mua 0 chiếc bút mực và 0 chiếc bút chì.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= total, cost1, cost2 &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta có ngân sách $\textit{total}$ và hai mức giá cố định; số lượng của mỗi loại có thể là bất kỳ số không âm nào. Không cần dùng bảng knapsack đầy đủ: sau khi cố định số bút mực, các số lượng bút chì hợp lệ tạo thành một đoạn liên tiếp bắt đầu từ 0.
>
> Liệt kê số bút mực $x$ từ $0$ đến $\lfloor \textit{total}/\textit{cost1} \rfloor$. Phần ngân sách còn lại mua được từ $0$ đến $\lfloor (\textit{total}-x\cdot\textit{cost1})/\textit{cost2} \rfloor$ chiếc bút chì, nên số cách đóng góp là giá trị đó cộng thêm một.

<!-- thinking:end -->

Ta có thể liệt kê số bút mực cần mua, ký hiệu là $x$. Với mỗi $x$, số bút chì tối đa có thể mua là $\frac{\textit{total} - x \times \textit{cost1}}{\textit{cost2}}$. Số cách tương ứng với mỗi $x$ là giá trị này cộng thêm 1. Cộng số cách của mọi $x$ để nhận được đáp án.

Độ phức tạp thời gian là $O(\frac{\textit{total}}{\textit{cost1}})$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def waysToBuyPensPencils(self, total: int, cost1: int, cost2: int) -> int:
        ans = 0
        for x in range(total // cost1 + 1):
            y = (total - (x * cost1)) // cost2 + 1
            ans += y
        return ans
```

#### Java

```java
class Solution {
    public long waysToBuyPensPencils(int total, int cost1, int cost2) {
        long ans = 0;
        for (int x = 0; x <= total / cost1; ++x) {
            int y = (total - x * cost1) / cost2 + 1;
            ans += y;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long waysToBuyPensPencils(int total, int cost1, int cost2) {
        long long ans = 0;
        for (int x = 0; x <= total / cost1; ++x) {
            int y = (total - x * cost1) / cost2 + 1;
            ans += y;
        }
        return ans;
    }
};
```

#### Go

```go
func waysToBuyPensPencils(total int, cost1 int, cost2 int) (ans int64) {
	for x := 0; x <= total/cost1; x++ {
		y := (total-x*cost1)/cost2 + 1
		ans += int64(y)
	}
	return
}
```

#### TypeScript

```ts
function waysToBuyPensPencils(total: number, cost1: number, cost2: number): number {
    let ans = 0;
    for (let x = 0; x <= Math.floor(total / cost1); ++x) {
        const y = Math.floor((total - x * cost1) / cost2) + 1;
        ans += y;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn ways_to_buy_pens_pencils(total: i32, cost1: i32, cost2: i32) -> i64 {
        let mut ans: i64 = 0;
        for pen in 0..=total / cost1 {
            ans += (((total - pen * cost1) / cost2) as i64) + 1;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
