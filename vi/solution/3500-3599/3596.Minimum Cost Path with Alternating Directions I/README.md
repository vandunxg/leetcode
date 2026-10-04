---
comments: true
difficulty: Medium
tags:
    - Brainteaser
    - Math
---

<!-- problem:start -->

# [3596. Minimum Cost Path with Alternating Directions I 🔒](https://leetcode.com/problems/minimum-cost-path-with-alternating-directions-i)

[中文文档](/solution/3500-3599/3596.Minimum%20Cost%20Path%20with%20Alternating%20Directions%20I/README.md)

## Mô tả

<!-- description:start -->
<p>Cho hai số nguyên <code>m</code> và <code>n</code> lần lượt biểu thị số hàng và số cột của một lưới.</p>

<p>Chi phí để đi vào ô <code>(i, j)</code> được định nghĩa là <code>(i + 1) * (j + 1)</code>.</p>

<p>Đường đi luôn bắt đầu bằng việc đi vào ô <code>(0, 0)</code> ở bước 1 và trả chi phí đi vào ô đó.</p>

<p>Ở mỗi bước, bạn di chuyển đến một ô <strong>kề</strong> theo quy luật luân phiên sau:</p>

<ul>
	<li>Ở các bước được đánh số <strong>lẻ</strong>, bạn phải di chuyển <strong>sang phải</strong> hoặc <strong>xuống dưới</strong>.</li>
	<li>Ở các bước được đánh số <strong>chẵn</strong>, bạn phải di chuyển <strong>sang trái</strong> hoặc <strong>lên trên</strong>.</li>
</ul>

<p>Trả về <strong>tổng chi phí nhỏ nhất</strong> cần thiết để đi đến ô <code>(m - 1, n - 1)</code>. Nếu không thể đi đến đó, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 1, n = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Bạn bắt đầu ở ô <code>(0, 0)</code>.</li>
	<li>Chi phí để đi vào ô <code>(0, 0)</code> là <code>(0 + 1) * (0 + 1) = 1</code>.</li>
	<li>Vì bạn đang ở ô đích nên tổng chi phí là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">m = 2, n = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Bạn bắt đầu ở ô <code>(0, 0)</code> với chi phí <code>(0 + 1) * (0 + 1) = 1</code>.</li>
	<li>Bước 1 (lẻ): Bạn có thể đi xuống ô <code>(1, 0)</code> với chi phí <code>(1 + 1) * (0 + 1) = 2</code>.</li>
	<li>Vậy tổng chi phí là <code>1 + 2 = 3</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Câu đố mẹo

<!-- thinking:start -->

> **Tư duy**
>
> Quy luật hướng di chuyển luân phiên khiến chỉ một vài kích thước lưới có thể đi đến đích. Các trường hợp còn lại là lưới $1\times 1$ (chi phí $1$), $2\times 1$ hoặc $1\times 2$ (chi phí $3$).
>
> Mọi kích thước khác đều trả về $-1$. Ba phép kiểm tra hằng số có thể thay thế cho việc tìm đường đi ngắn nhất.

<!-- thinking:end -->

Do các quy tắc di chuyển trong đề bài, thực tế chỉ có ba trường hợp sau có thể đi đến ô đích:

1. Lưới $1 \times 1$, với chi phí bằng $1$.
2. Lưới có $2$ hàng và $1$ cột, với chi phí bằng $3$.
3. Lưới có $1$ hàng và $2$ cột, với chi phí bằng $3$.

Với mọi trường hợp khác, không thể đi đến ô đích, nên trả về $-1$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, m: int, n: int) -> int:
        if m == 1 and n == 1:
            return 1
        if m == 2 and n == 1:
            return 3
        if m == 1 and n == 2:
            return 3
        return -1
```

#### Java

```java
class Solution {
    public int minCost(int m, int n) {
        if (m == 1 && n == 1) {
            return 1;
        }
        if (m == 1 && n == 2) {
            return 3;
        }
        if (m == 2 && n == 1) {
            return 3;
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCost(int m, int n) {
        if (m == 1 && n == 1) {
            return 1;
        }
        if (m == 1 && n == 2) {
            return 3;
        }
        if (m == 2 && n == 1) {
            return 3;
        }
        return -1;
    }
};
```

#### Go

```go
func minCost(m int, n int) int {
	if m == 1 && n == 1 {
		return 1
	}
	if m == 1 && n == 2 {
		return 3
	}
	if m == 2 && n == 1 {
		return 3
	}
	return -1
}
```

#### TypeScript

```ts
function minCost(m: number, n: number): number {
    if (m === 1 && n === 1) {
        return 1;
    }
    if (m === 1 && n === 2) {
        return 3;
    }
    if (m === 2 && n === 1) {
        return 3;
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
