---
comments: true
difficulty: Easy
rating: 1260
source: Weekly Contest 511 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3996. Even Number of Knight Moves](https://leetcode.com/problems/even-number-of-knight-moves)

[中文文档](/solution/3900-3999/3996.Even%20Number%20of%20Knight%20Moves/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>start</code> và <code>target</code>, trong đó mỗi mảng có dạng <code>[x, y]</code> biểu diễn một ô trên bàn cờ vua 8 x 8 tiêu chuẩn.</p>

<p>Trả về <code>true</code> nếu quân mã có thể đi từ <code>start</code> đến <code>target</code> sau một số bước đi <strong>chẵn</strong>. Nếu không, trả về <code>false</code>.</p>

<p><strong>Lưu ý:</strong> Một nước đi hợp lệ của quân mã là đi hai ô theo một hướng và một ô theo hướng vuông góc với nó. Hình dưới đây minh họa tất cả tám nước đi có thể có từ một ô.</p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3996.Even%20Number%20of%20Knight%20Moves/images/knight.png" style="height: 200px; width: 200px;" /></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">start = [1,1], target = [2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi nước đi có thể là <code>(1, 1) -&gt; (3, 2) -&gt; (2, 4) -&gt; (4, 3) -&gt; (2, 2)</code>.</p>

<p>Quân mã đến đích sau 4 nước đi, đây là số chẵn. Vì vậy, đáp án là <code>true</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">start = [4,5], target = [6,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<p>Không thể đi từ <code>start = [4, 5]</code> đến <code>target = [6, 6]</code> sau một số nước đi chẵn. Vì vậy, đáp án là <code>false</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>start.length == target.length == 2</code></li>
	<li><code>0 &lt;= start[i], target[i] &lt;= 7</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Parity

<!-- thinking:start -->

> **Tư duy**
>
> Một bước đi của quân mã đảo tính chẵn lẻ của $x+y$, tức là màu của ô cờ. Đường đi có độ dài chẵn vẫn ở cùng màu. Trên bàn cờ $8\times 8$, quân mã đi được đến mọi ô, nên hai ô cùng màu khi và chỉ khi tồn tại đường đi có độ dài chẵn.
>
> So sánh $(x+y)\bmod 2$ của hai ô; không cần BFS.

<!-- thinking:end -->

Mỗi nước đi của quân mã có độ lệch $(\pm 1, \pm 2)$ hoặc $(\pm 2, \pm 1)$, vì vậy độ thay đổi của tổng tọa độ $x + y$ luôn là số lẻ. Nói cách khác, mỗi nước đi đều đảo màu của ô cờ (đen/trắng được phân biệt bởi $(x + y) \bmod 2$).

Do đó:

- Sau một số nước đi chẵn, ô bắt đầu và ô đích có cùng màu;
- Sau một số nước đi lẻ, ô bắt đầu và ô đích có màu khác nhau.

Trên bàn cờ vua $8 \times 8$, quân mã có thể đi đến mọi ô, và mọi đường đi đến ô cùng màu đều có độ dài chẵn. Vì vậy, chỉ cần kiểm tra xem $(x + y) \bmod 2$ của ô bắt đầu và ô đích có bằng nhau hay không.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canReach(self, start: list[int], target: list[int]) -> bool:
        return (start[0] + start[1]) % 2 == (target[0] + target[1]) % 2
```

#### Java

```java
class Solution {
    public boolean canReach(int[] start, int[] target) {
        return (start[0] + start[1]) % 2 == (target[0] + target[1]) % 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canReach(vector<int>& start, vector<int>& target) {
        return (start[0] + start[1]) % 2 == (target[0] + target[1]) % 2;
    }
};
```

#### Go

```go
func canReach(start []int, target []int) bool {
	return (start[0]+start[1])%2 == (target[0]+target[1])%2
}
```

#### TypeScript

```ts
function canReach(start: number[], target: number[]): boolean {
    return (start[0] + start[1]) % 2 === (target[0] + target[1]) % 2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
