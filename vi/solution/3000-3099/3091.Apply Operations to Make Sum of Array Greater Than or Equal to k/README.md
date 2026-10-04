---
comments: true
difficulty: Medium
rating: 1521
source: Weekly Contest 390 Q2
tags:
    - Greedy
    - Math
    - Enumeration
---

<!-- problem:start -->

# [3091. Apply Operations to Make Sum of Array Greater Than or Equal to k](https://leetcode.com/problems/apply-operations-to-make-sum-of-array-greater-than-or-equal-to-k)

[中文文档](/solution/3000-3099/3091.Apply%20Operations%20to%20Make%20Sum%20of%20Array%20Greater%20Than%20or%20Equal%20to%20k/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <strong>dương</strong> <code>k</code>. Ban đầu, ta có một mảng <code>nums = [1]</code>.</p>

<p>Ta có thể thực hiện <strong>bất kỳ</strong> thao tác nào sau đây trên mảng <strong>bao nhiêu lần tùy ý</strong> (<strong>có thể không thực hiện lần nào</strong>):</p>

<ul>
	<li>Chọn một phần tử bất kỳ trong mảng và <strong>tăng</strong> giá trị của nó lên <code>1</code>.</li>
	<li>Sao chép một phần tử bất kỳ trong mảng và thêm bản sao đó vào cuối mảng.</li>
</ul>

<p>Trả về <em><strong>số thao tác nhỏ nhất</strong> cần thực hiện để <strong>tổng</strong> các phần tử của mảng cuối cùng lớn hơn hoặc bằng </em><code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 11</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể thực hiện các thao tác sau trên mảng <code>nums = [1]</code>:</p>

<ul>
	<li>Tăng phần tử lên <code>1</code> ba lần. Khi đó mảng trở thành <code>nums = [4]</code>.</li>
	<li>Sao chép phần tử hai lần. Khi đó mảng trở thành <code>nums = [4,4,4]</code>.</li>
</ul>

<p>Tổng các phần tử của mảng cuối cùng là <code>4 + 4 + 4 = 12</code>, lớn hơn hoặc bằng <code>k = 11</code>.<br />
Tổng số thao tác đã thực hiện là <code>3 + 2 = 5</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tổng của mảng ban đầu đã lớn hơn hoặc bằng <code>1</code>, nên không cần thực hiện thao tác nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mảng ban đầu là $[1]$. Ta có thể tăng một phần tử hoặc thêm một bản sao của phần tử đó, với mục tiêu đạt tổng ít nhất là $k \le 10^5$.
>
> Vì thao tác thêm bản sao sẽ sao chép giá trị hiện tại, ta nên tăng một phần tử lên $x$ rồi sao chép $x$. Chi phí là $(x-1)+(\lceil k/x \rceil-1)$.
>
> Ta liệt kê số lần tăng $a$ (khi đó $x=a+1$), tính số lần sao chép tương ứng, rồi lấy giá trị nhỏ nhất của $a+b$.

<!-- thinking:end -->

Ta nên thực hiện thao tác sao chép (tức thao tác $2$) ở cuối để giảm số thao tác.

Do đó, ta liệt kê số lần thực hiện thao tác $1$, ký hiệu là $a$, trong đoạn $[0, k]$. Khi đó, số lần thực hiện thao tác $2$, ký hiệu là $b$, là $\left\lceil \frac{k}{a+1} \right\rceil - 1$. Ta lấy giá trị nhỏ nhất của $a+b$.

Độ phức tạp thời gian là $O(k)$, trong đó $k$ là số nguyên dương đầu vào $k$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, k: int) -> int:
        ans = k
        for a in range(k):
            x = a + 1
            b = (k + x - 1) // x - 1
            ans = min(ans, a + b)
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(int k) {
        int ans = k;
        for (int a = 0; a < k; ++a) {
            int x = a + 1;
            int b = (k + x - 1) / x - 1;
            ans = Math.min(ans, a + b);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(int k) {
        int ans = k;
        for (int a = 0; a < k; ++a) {
            int x = a + 1;
            int b = (k + x - 1) / x - 1;
            ans = min(ans, a + b);
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(k int) int {
	ans := k
	for a := 0; a < k; a++ {
		x := a + 1
		b := (k+x-1)/x - 1
		ans = min(ans, a+b)
	}
	return ans
}
```

#### TypeScript

```ts
function minOperations(k: number): number {
    let ans = k;
    for (let a = 0; a < k; ++a) {
        const x = a + 1;
        const b = Math.ceil(k / x) - 1;
        ans = Math.min(ans, a + b);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(k: i32) -> i32 {
        let mut ans = k;
        for a in 0..k {
            let x = a + 1;
            let b = (k + x - 1) / x - 1;
            ans = ans.min(a + b);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
