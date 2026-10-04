---
comments: true
difficulty: Easy
rating: 1339
source: Weekly Contest 451 Q1
tags:
    - Math
---

<!-- problem:start -->

# [3560. Find Minimum Log Transportation Cost](https://leetcode.com/problems/find-minimum-log-transportation-cost)

[中文文档](/solution/3500-3599/3560.Find%20Minimum%20Log%20Transportation%20Cost/README.md)

## Mô tả

<!-- description:start -->
<p>Cho ba số nguyên <code>n</code>, <code>m</code> và <code>k</code>.</p>

<p>Có hai khúc gỗ dài lần lượt <code>n</code> và <code>m</code> đơn vị, cần được vận chuyển bằng ba xe tải, trong đó mỗi xe có thể chở một khúc gỗ dài <strong>không quá</strong> <code>k</code> đơn vị.</p>

<p>Bạn có thể cắt các khúc gỗ thành những mảnh nhỏ hơn. Chi phí cắt một khúc gỗ dài <code>x</code> thành hai khúc dài <code>len1</code> và <code>len2</code> là <code>cost = len1 * len2</code>, với điều kiện <code>len1 + len2 = x</code>.</p>

<p>Hãy trả về <strong>tổng chi phí nhỏ nhất</strong> để phân bổ các khúc gỗ lên các xe tải. Nếu không cần cắt khúc gỗ nào thì tổng chi phí là 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 6, m = 5, k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cắt khúc gỗ dài 6 thành hai khúc dài 1 và 5, với chi phí bằng <code>1 * 5 == 5</code>. Khi đó, ba khúc gỗ dài 1, 5 và 5 có thể được chở trên mỗi xe một khúc.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, m = 4, k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hai khúc gỗ đã có thể được chở trên các xe tải, nên không cần cắt.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= k &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= n, m &lt;= 2 * k</code></li>
    <li>Dữ liệu đầu vào được tạo sao cho luôn có thể vận chuyển các khúc gỗ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều nhất một khúc gỗ dài hơn $k$, và chỉ cần một lần cắt để cả hai mảnh đều không dài quá $k$. Gọi $x$ là khúc gỗ dài hơn; nếu $x \le k$ thì không cần cắt.
>
> Cắt thành hai khúc dài $k$ và $x-k$ có chi phí $k \cdot (x-k)$, đây là cách cắt hợp lệ duy nhất. Giá trị này có độ phức tạp $O(1)$.

<!-- thinking:end -->

Nếu độ dài của cả hai khúc gỗ đều không vượt quá tải trọng tối đa $k$ của xe tải, thì không cần cắt và ta chỉ cần trả về $0$.

Ngược lại, điều đó có nghĩa là chỉ có một khúc gỗ dài hơn $k$, và ta cần cắt nó thành hai mảnh. Gọi độ dài của khúc gỗ dài hơn là $x$, khi đó chi phí cắt là $k \times (x - k)$.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCuttingCost(self, n: int, m: int, k: int) -> int:
        x = max(n, m)
        return 0 if x <= k else k * (x - k)
```

#### Java

```java
class Solution {
    public long minCuttingCost(int n, int m, int k) {
        int x = Math.max(n, m);
        return x <= k ? 0 : 1L * k * (x - k);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minCuttingCost(int n, int m, int k) {
        int x = max(n, m);
        return x <= k ? 0 : 1LL * k * (x - k);
    }
};
```

#### Go

```go
func minCuttingCost(n int, m int, k int) int64 {
    x := max(n, m)
    if x <= k {
        return 0
    }
    return int64(k * (x - k))
}
```

#### TypeScript

```ts
function minCuttingCost(n: number, m: number, k: number): number {
    const x = Math.max(n, m);
    return x <= k ? 0 : k * (x - k);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
