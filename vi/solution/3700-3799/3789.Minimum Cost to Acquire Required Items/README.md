---
comments: true
difficulty: Medium
rating: 1579
source: Weekly Contest 482 Q2
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [3789. Minimum Cost to Acquire Required Items](https://leetcode.com/problems/minimum-cost-to-acquire-required-items)

[中文文档](/solution/3700-3799/3789.Minimum%20Cost%20to%20Acquire%20Required%20Items/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho năm số nguyên <code>cost1</code>, <code>cost2</code>, <code>costBoth</code>, <code>need1</code> và <code>need2</code>.</p>

<p>Có ba loại vật phẩm:</p>

<ul>
	<li>Vật phẩm <strong>loại 1</strong> có giá <code>cost1</code> và chỉ đóng góp 1 đơn vị vào yêu cầu loại 1.</li>
	<li>Vật phẩm <strong>loại 2</strong> có giá <code>cost2</code> và chỉ đóng góp 1 đơn vị vào yêu cầu loại 2.</li>
	<li>Vật phẩm <strong>loại 3</strong> có giá <code>costBoth</code> và đóng góp 1 đơn vị vào <strong>cả hai</strong> yêu cầu loại 1 và loại 2.</li>
</ul>

<p>Bạn phải thu thập đủ vật phẩm sao cho tổng đóng góp cho loại 1 <strong>ít nhất</strong> là <code>need1</code> và tổng đóng góp cho loại 2 <strong>ít nhất</strong> là <code>need2</code>.</p>

<p>Trả về một số nguyên biểu thị <strong>tổng chi phí nhỏ nhất</strong> có thể để đạt được các yêu cầu này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">cost1 = 3, cost2 = 2, costBoth = 1, need1 = 3, need2 = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi mua ba vật phẩm loại 3 với chi phí <code>3 * 1 = 3</code>, tổng đóng góp cho loại 1 là 3 (<code>&gt;= need1 = 3</code>) và cho loại 2 là 3 (<code>&gt;= need2 = 2</code>).<br data-end="229" data-start="226" />
Bất kỳ cách kết hợp hợp lệ nào khác cũng sẽ tốn nhiều hơn, nên tổng chi phí nhỏ nhất là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">cost1 = 5, cost2 = 4, costBoth = 15, need1 = 2, need2 = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">22</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta mua <code>need1 = 2</code> vật phẩm loại 1 và <code>need2 = 3</code> vật phẩm loại 2: <code>2 * 5 + 3 * 4 = 10 + 12 = 22</code>.<br />
Bất kỳ cách kết hợp hợp lệ nào khác cũng sẽ tốn nhiều hơn, nên tổng chi phí nhỏ nhất là 22.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">cost1 = 5, cost2 = 4, costBoth = 15, need1 = 0, need2 = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì không cần vật phẩm nào (<code>need1 = need2 = 0</code>), ta không mua gì và trả 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= cost1, cost2, costBoth &lt;= 10<sup>6</sup></code></li>
	<li><code>0 &lt;= need1, need2 &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi nhu cầu có thể được đáp ứng bằng vật phẩm riêng lẻ hoặc vật phẩm bundle, và các nhu cầu quá lớn nên không thể mô phỏng từng đơn vị. Chỉ có ba chiến lược cần xét: chỉ mua vật phẩm riêng lẻ, chỉ mua vật phẩm bundle, hoặc mua $\min(need1,need2)$ vật phẩm bundle rồi hoàn tất bằng các vật phẩm riêng lẻ. Ta chọn giá trị nhỏ nhất trong ba trường hợp.

<!-- thinking:end -->

Ta có thể chia chiến lược mua hàng thành ba trường hợp:

1. Chỉ mua vật phẩm loại 1 và loại 2. Tổng chi phí là $a = \textit{need1} \times \textit{cost1} + \textit{need2} \times \textit{cost2}$.
2. Chỉ mua vật phẩm loại 3. Tổng chi phí là $b = \textit{costBoth} \times \max(\textit{need1}, \textit{need2})$.
3. Mua một số vật phẩm loại 3, rồi mua riêng vật phẩm loại 1 và loại 2 cho các nhu cầu còn lại. Gọi $\textit{mn} = \min(\textit{need1}, \textit{need2})$, khi đó tổng chi phí là $c = \textit{costBoth} \times \textit{mn} + (\textit{need1} - \textit{mn}) \times \textit{cost1} + (\textit{need2} - \textit{mn}) \times \textit{cost2}$.

Cuối cùng, ta trả về giá trị nhỏ nhất trong ba trường hợp, $\min(a, b, c)$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(
        self, cost1: int, cost2: int, costBoth: int, need1: int, need2: int
    ) -> int:
        a = need1 * cost1 + need2 * cost2
        b = costBoth * max(need1, need2)
        mn = min(need1, need2)
        c = costBoth * mn + (need1 - mn) * cost1 + (need2 - mn) * cost2
        return min(a, b, c)
```

#### Java

```java
class Solution {
    public long minimumCost(int cost1, int cost2, int costBoth, int need1, int need2) {
        long a = (long) need1 * cost1 + (long) need2 * cost2;
        long b = (long) costBoth * Math.max(need1, need2);
        int mn = Math.min(need1, need2);
        long c = (long) costBoth * mn + (long) (need1 - mn) * cost1 + (long) (need2 - mn) * cost2;
        return Math.min(a, Math.min(b, c));
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumCost(int cost1, int cost2, int costBoth, int need1, int need2) {
        long long a = 1LL * need1 * cost1 + 1LL * need2 * cost2;
        long long b = 1LL * costBoth * max(need1, need2);
        int mn = min(need1, need2);
        long long c = 1LL * costBoth * mn
            + 1LL * (need1 - mn) * cost1
            + 1LL * (need2 - mn) * cost2;
        return min({a, b, c});
    }
};
```

#### Go

```go
func minimumCost(cost1 int, cost2 int, costBoth int, need1 int, need2 int) int64 {
	a := int64(need1)*int64(cost1) + int64(need2)*int64(cost2)
	b := int64(costBoth) * int64(max(need1, need2))
	mn := min(need1, need2)
	c := int64(costBoth)*int64(mn) +
		int64(need1-mn)*int64(cost1) +
		int64(need2-mn)*int64(cost2)
	return min(a, min(b, c))
}
```

#### TypeScript

```ts
function minimumCost(
    cost1: number,
    cost2: number,
    costBoth: number,
    need1: number,
    need2: number,
): number {
    const a = need1 * cost1 + need2 * cost2;
    const b = costBoth * Math.max(need1, need2);
    const mn = Math.min(need1, need2);
    const c = costBoth * mn + (need1 - mn) * cost1 + (need2 - mn) * cost2;
    return Math.min(a, b, c);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
