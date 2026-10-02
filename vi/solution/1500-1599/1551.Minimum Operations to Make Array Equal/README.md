---
comments: true
difficulty: Medium
rating: 1293
source: Weekly Contest 202 Q2
tags:
    - Math
---

<!-- problem:start -->

# [1551. Minimum Operations to Make Array Equal](https://leetcode.com/problems/minimum-operations-to-make-array-equal)

[中文文档](/solution/1500-1599/1551.Minimum%20Operations%20to%20Make%20Array%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>arr</code> có độ dài <code>n</code>, trong đó <code>arr[i] = (2 * i) + 1</code> với mọi giá trị hợp lệ của <code>i</code> (tức là&nbsp;<code>0 &lt;= i &lt; n</code>).</p>

<p>Trong một phép toán, ta có thể chọn hai chỉ số <code>x</code> và <code>y</code> sao cho <code>0 &lt;= x, y &lt; n</code>, sau đó trừ <code>1</code> khỏi <code>arr[x]</code> và cộng <code>1</code> vào <code>arr[y]</code> (tức là thực hiện <code>arr[x] -=1 </code>và <code>arr[y] += 1</code>). Mục tiêu là làm cho mọi phần tử trong mảng <strong>bằng nhau</strong>. Đề bài <strong>đảm bảo</strong> có thể làm mọi phần tử bằng nhau bằng một số phép toán.</p>

<p>Cho số nguyên <code>n</code> là độ dài mảng, hãy trả về <em>số phép toán nhỏ nhất</em> cần thực hiện để mọi phần tử của arr bằng nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> arr = [1, 3, 5]
Phép toán đầu tiên chọn x = 2 và y = 0, đưa arr về [2, 3, 4]
Trong phép toán thứ hai, tiếp tục chọn x = 2 và y = 0, do đó arr = [3, 3, 3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6
<strong>Đầu ra:</strong> 9
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Mảng có dạng $1,3,5,\ldots,2n-1$. Mỗi bước tăng một phần tử và giảm một phần tử khác cho đến khi mọi giá trị bằng nhau. Mô phỏng từng bước là lãng phí với $n\le 10^4$, còn tổng mảng không đổi nên giá trị đích có thể xác định ngay.
>
> Tổng là $n^2$, do đó giá trị chung là $n$. Chỉ nửa nhỏ hơn cần tăng: số lẻ thứ $i$ là $2i+1$ còn thiếu $n-(2i+1)$. Tổng các phần thiếu này chính là số phép toán nhỏ nhất.

<!-- thinking:end -->

Theo mô tả đề bài, mảng $arr$ là một cấp số cộng có số hạng đầu là $1$ và công sai là $2$. Vì vậy, tổng $n$ số hạng đầu của mảng là:

$$
\begin{aligned}
S_n &= \frac{n}{2} \times (a_1 + a_n) \\
&= \frac{n}{2} \times (1 + (2n - 1)) \\
&= n^2
\end{aligned}
$$

Vì trong một phép toán, một số giảm một đơn vị còn một số khác tăng một đơn vị nên tổng mọi phần tử trong mảng không đổi. Do đó, khi mọi phần tử bằng nhau, giá trị mỗi phần tử là $S_n / n = n$. Suy ra số phép toán nhỏ nhất để làm mọi phần tử trong mảng bằng nhau là:

$$
\sum_{i=0}^{\frac{n}{2}} (n - (2i + 1))
$$

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, n: int) -> int:
        return sum(n - (i << 1 | 1) for i in range(n >> 1))
```

#### Java

```java
class Solution {
    public int minOperations(int n) {
        int ans = 0;
        for (int i = 0; i < n >> 1; ++i) {
            ans += n - (i << 1 | 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(int n) {
        int ans = 0;
        for (int i = 0; i < n >> 1; ++i) {
            ans += n - (i << 1 | 1);
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(n int) (ans int) {
	for i := 0; i < n>>1; i++ {
		ans += n - (i<<1 | 1)
	}
	return
}
```

#### TypeScript

```ts
function minOperations(n: number): number {
    let ans = 0;
    for (let i = 0; i < n >> 1; ++i) {
        ans += n - ((i << 1) | 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
