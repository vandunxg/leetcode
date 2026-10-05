---
comments: true
difficulty: Medium
rating: 1380
source: Weekly Contest 493 Q2
tags:
    - Math
---

<!-- problem:start -->

# [3871. Count Commas in Range II](https://leetcode.com/problems/count-commas-in-range-ii)

[中文文档](/solution/3800-3899/3871.Count%20Commas%20in%20Range%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>.</p>

<p>Hãy trả về <strong>tổng</strong> số dấu phẩy được sử dụng khi viết tất cả các số nguyên từ <code>[1, n]</code> (bao gồm cả hai đầu mút) theo định dạng số <strong>tiêu chuẩn</strong>.</p>

<p>Trong định dạng <strong>tiêu chuẩn</strong>:</p>

<ul>
	<li>Một dấu phẩy được chèn sau <strong>mỗi ba</strong> chữ số tính từ bên phải.</li>
	<li>Các số có <strong>ít hơn</strong> 4 chữ số không chứa dấu phẩy.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1002</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số <code>&quot;1,000&quot;</code>, <code>&quot;1,001&quot;</code> và <code>&quot;1,002&quot;</code> đều chứa một dấu phẩy, nên tổng là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 998</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong>​​​​​​​</strong>Tất cả các số từ 1 đến 998 đều có ít hơn bốn chữ số. Vì vậy, không có dấu phẩy nào được sử dụng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Vẫn là bài toán đếm số dấu phẩy, nhưng giờ đây $n \le 10^{15}$ nên một số có thể chứa nhiều dấu phẩy.
>
> Mỗi khi vượt qua một ngưỡng $10^{3t}$, mọi số nguyên về sau sẽ có thêm một dấu phẩy.
>
> Bắt đầu từ $x=1000$, nhân x với $1000$ và cộng $n-x+1$ cho đến khi $x>n$.
>
> Vòng lặp chạy $O(\log_{1000} n)$ lần.

<!-- thinking:end -->

Dựa trên mô tả bài toán, ta có thể nhận thấy quy luật sau:

- Các số trong đoạn [1, 999] không chứa dấu phẩy;
- Các số trong đoạn [1,000, 999,999] chứa một dấu phẩy;
- Các số trong đoạn [1,000,000, 999,999,999] chứa hai dấu phẩy;
- Và cứ tiếp tục như vậy.

Do đó, ta có thể bắt đầu từ $x = 1000$ và nhân $x$ với 1000 sau mỗi lần lặp cho đến khi $x$ vượt quá $n$. Trong mỗi lần lặp, có $n - x + 1$ số mới có thêm một dấu phẩy, nên ta cộng số lượng này vào đáp án.

Độ phức tạp thời gian là $O(\log n)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countCommas(self, n: int) -> int:
        ans = 0
        x = 1000
        while x <= n:
            ans += n - x + 1
            x *= 1000
        return ans
```

#### Java

```java
class Solution {
    public long countCommas(long n) {
        long ans = 0;
        for (long x = 1000; x <= n; x *= 1000) {
            ans += n - x + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countCommas(long long n) {
        long long ans = 0;
        for (long long x = 1000; x <= n; x *= 1000) {
            ans += n - x + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func countCommas(n int64) (ans int64) {
	for x := int64(1000); x <= n; x *= 1000 {
		ans += n - x + 1
	}
	return
}
```

#### TypeScript

```ts
function countCommas(n: number): number {
    let ans = 0;
    for (let x = 1000; x <= n; x *= 1000) {
        ans += n - x + 1;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
