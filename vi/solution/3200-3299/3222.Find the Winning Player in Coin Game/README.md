---
comments: true
difficulty: Easy
rating: 1269
source: Biweekly Contest 135 Q1
tags:
    - Math
    - Game Theory
    - Simulation
---

<!-- problem:start -->

# [3222. Find the Winning Player in Coin Game](https://leetcode.com/problems/find-the-winning-player-in-coin-game)

[中文文档](/solution/3200-3299/3222.Find%20the%20Winning%20Player%20in%20Coin%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <strong>dương</strong> <code>x</code> và <code>y</code>, <em>lần lượt</em> biểu thị số đồng xu có giá trị 75 và 10.</p>

<p>Alice và Bob đang chơi một trò chơi. Ở mỗi lượt, bắt đầu từ <strong>Alice</strong>, người chơi phải lấy các đồng xu có <strong>tổng</strong> giá trị bằng 115. Nếu không thể thực hiện, người chơi đó <strong>thua</strong> trò chơi.</p>

<p>Trả về <em>tên</em> người chơi thắng trò chơi nếu cả hai người đều chơi <strong>tối ưu</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">x = 2, y = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;Alice&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trò chơi kết thúc sau một lượt:</p>

<ul>
    <li>Alice lấy 1 đồng xu có giá trị 75 và 4 đồng xu có giá trị 10.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">x = 4, y = 11</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;Bob&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trò chơi kết thúc sau 2 lượt:</p>

<ul>
    <li>Alice lấy 1 đồng xu có giá trị 75 và 4 đồng xu có giá trị 10.</li>
    <li>Bob lấy 1 đồng xu có giá trị 75 và 4 đồng xu có giá trị 10.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= x, y &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lượt tiêu tốn $2$ đồng xu có giá trị $75$ và $8$ đồng xu có giá trị $10$. Với $x,y\le 100$, ta có thể mô phỏng từng lượt, nhưng số lượt đầy đủ chỉ là min của hai thương.
>
> Đặt $k=\min(\lfloor x/2\rfloor,\lfloor y/8\rfloor)$, trừ số xu đã dùng, rồi kiểm tra xem còn đủ cho thêm một lượt chưa (một đồng xu có giá trị $75$ và ít nhất bốn đồng xu có giá trị $10$). Nếu đủ, Alice thực hiện lượt đó; nếu không, Bob thắng. Phép kiểm tra này có độ phức tạp $O(1)$.

<!-- thinking:end -->

Vì mỗi lượt tiêu tốn $2$ đồng xu có giá trị $75$ và $8$ đồng xu có giá trị $10$, ta có thể tính số lượt $k = \min(x / 2, y / 8)$, rồi cập nhật các giá trị của $x$ và $y$, trong đó $x$ và $y$ là số đồng xu còn lại sau $k$ lượt.

Nếu $x > 0$ và $y \geq 4$, Alice có thể tiếp tục thực hiện, Bob thua, nên trả về "Alice"; nếu không, trả về "Bob".

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def losingPlayer(self, x: int, y: int) -> str:
        k = min(x // 2, y // 8)
        x -= k * 2
        y -= k * 8
        return "Alice" if x and y >= 4 else "Bob"
```

#### Java

```java
class Solution {
    public String losingPlayer(int x, int y) {
        int k = Math.min(x / 2, y / 8);
        x -= k * 2;
        y -= k * 8;
        return x > 0 && y >= 4 ? "Alice" : "Bob";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string losingPlayer(int x, int y) {
        int k = min(x / 2, y / 8);
        x -= k * 2;
        y -= k * 8;
        return x && y >= 4 ? "Alice" : "Bob";
    }
};
```

#### Go

```go
func losingPlayer(x int, y int) string {
    k := min(x/2, y/8)
    x -= 2 * k
    y -= 8 * k
    if x > 0 && y >= 4 {
        return "Alice"
    }
    return "Bob"
}
```

#### TypeScript

```ts
function losingPlayer(x: number, y: number): string {
    const k = Math.min((x / 2) | 0, (y / 8) | 0);
    x -= k * 2;
    y -= k * 8;
    return x && y >= 4 ? 'Alice' : 'Bob';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
