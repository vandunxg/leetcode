---
comments: true
difficulty: Easy
rating: 1434
source: Weekly Contest 338 Q1
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [2600. K Items With the Maximum Sum](https://leetcode.com/problems/k-items-with-the-maximum-sum)

[中文文档](/solution/2600-2699/2600.K%20Items%20With%20the%20Maximum%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Có một chiếc túi chứa các vật phẩm, trên mỗi vật phẩm có ghi một số <code>1</code>, <code>0</code> hoặc <code>-1</code>.</p>

<p>Cho bốn số nguyên <strong>không âm</strong> <code>numOnes</code>, <code>numZeros</code>, <code>numNegOnes</code> và <code>k</code>.</p>

<p>Ban đầu, chiếc túi chứa:</p>

<ul>
	<li><code>numOnes</code> vật phẩm có ghi số <code>1</code>.</li>
	<li><code>numZeroes</code> vật phẩm có ghi số <code>0</code>.</li>
	<li><code>numNegOnes</code> vật phẩm có ghi số <code>-1</code>.</li>
</ul>

<p>Ta muốn chọn chính xác <code>k</code> vật phẩm trong số các vật phẩm có sẵn. Hãy trả về <em><strong>tổng lớn nhất</strong> có thể của các số ghi trên những vật phẩm được chọn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> numOnes = 3, numZeros = 2, numNegOnes = 0, k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có một chiếc túi gồm các vật phẩm mang các số {1, 1, 1, 0, 0}. Ta chọn 2 vật phẩm ghi số 1 và nhận được tổng bằng 2.
Có thể chứng minh rằng 2 là tổng lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> numOnes = 3, numZeros = 2, numNegOnes = 0, k = 4
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có một chiếc túi gồm các vật phẩm mang các số {1, 1, 1, 0, 0}. Ta chọn 3 vật phẩm ghi số 1 và 1 vật phẩm ghi số 0, nhận được tổng bằng 3.
Có thể chứng minh rằng 3 là tổng lớn nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= numOnes, numZeros, numNegOnes &lt;= 50</code></li>
	<li><code>0 &lt;= k &lt;= numOnes + numZeros + numNegOnes</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ba loại giá trị là cố định. Nếu liệt kê số lượng $1$, $0$ và $-1$ được chọn, số trường hợp cần xét tăng tuyến tính theo $k$. Vì các số lượng nhiều nhất chỉ là $50$, cách vét cạn vẫn có thể chạy được, nhưng không cần thiết.
>
> Tổng là một tổ hợp tuyến tính của $1$, $0$ và $-1$: chọn thêm một $1$ luôn tốt hơn chọn thêm một $0$, và lựa chọn đó tốt hơn chọn thêm một $-1$. Do đó, phương án tối ưu duy nhất là chọn tất cả các $1$ trước, sau đó đến các $0$, rồi mới đến các $-1$.
>
> So sánh $k$ với $\textit{numOnes}$ và $\textit{numZeros}$ sẽ xác định một trong ba trường hợp trên trong thời gian hằng số, mà không cần tạo danh sách các vật phẩm được chọn.

<!-- thinking:end -->

Theo mô tả bài toán, ta nên chọn càng nhiều vật phẩm ghi số $1$ càng tốt, sau đó chọn các vật phẩm ghi số $0$, và cuối cùng chọn các vật phẩm ghi số $-1$.

Cụ thể:

- Nếu số vật phẩm ghi số $1$ trong túi lớn hơn hoặc bằng $k$, ta chọn $k$ vật phẩm và tổng các số là $k$.
- Nếu số vật phẩm ghi số $1$ nhỏ hơn $k$, ta chọn $\textit{numOnes}$ vật phẩm, khi đó tổng bằng $\textit{numOnes}$. Nếu số vật phẩm ghi số $0$ lớn hơn hoặc bằng $k - \textit{numOnes}$, ta chọn thêm $k - \textit{numOnes}$ vật phẩm, nên tổng vẫn bằng $\textit{numOnes}$.
- Nếu không, ta chọn $k - \textit{numOnes} - \textit{numZeros}$ vật phẩm ghi số $-1$, khi đó tổng là $\textit{numOnes} - (k - \textit{numOnes} - \textit{numZeros})$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kItemsWithMaximumSum(
        self, numOnes: int, numZeros: int, numNegOnes: int, k: int
    ) -> int:
        if numOnes >= k:
            return k
        if numZeros >= k - numOnes:
            return numOnes
        return numOnes - (k - numOnes - numZeros)
```

#### Java

```java
class Solution {
    public int kItemsWithMaximumSum(int numOnes, int numZeros, int numNegOnes, int k) {
        if (numOnes >= k) {
            return k;
        }
        if (numZeros >= k - numOnes) {
            return numOnes;
        }
        return numOnes - (k - numOnes - numZeros);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int kItemsWithMaximumSum(int numOnes, int numZeros, int numNegOnes, int k) {
        if (numOnes >= k) {
            return k;
        }
        if (numZeros >= k - numOnes) {
            return numOnes;
        }
        return numOnes - (k - numOnes - numZeros);
    }
};
```

#### Go

```go
func kItemsWithMaximumSum(numOnes int, numZeros int, numNegOnes int, k int) int {
	if numOnes >= k {
		return k
	}
	if numZeros >= k-numOnes {
		return numOnes
	}
	return numOnes - (k - numOnes - numZeros)
}
```

#### TypeScript

```ts
function kItemsWithMaximumSum(
    numOnes: number,
    numZeros: number,
    numNegOnes: number,
    k: number,
): number {
    if (numOnes >= k) {
        return k;
    }
    if (numZeros >= k - numOnes) {
        return numOnes;
    }
    return numOnes - (k - numOnes - numZeros);
}
```

#### Rust

```rust
impl Solution {
    pub fn k_items_with_maximum_sum(
        num_ones: i32,
        num_zeros: i32,
        num_neg_ones: i32,
        k: i32,
    ) -> i32 {
        if num_ones > k {
            return k;
        }

        if num_ones + num_zeros > k {
            return num_ones;
        }

        num_ones - (k - num_ones - num_zeros)
    }
}
```

#### C#

```cs
public class Solution {
    public int KItemsWithMaximumSum(int numOnes, int numZeros, int numNegOnes, int k) {
        if (numOnes >= k) {
            return k;
        }
        if (numZeros >= k - numOnes) {
            return numOnes;
        }
        return numOnes - (k - numOnes - numZeros);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
