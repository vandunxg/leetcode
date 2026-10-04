---
comments: true
difficulty: Medium
tags:
    - Math
    - Dynamic Programming
    - Number Theory
---

<!-- problem:start -->

# [2979. Most Expensive Item That Can Not Be Bought 🔒](https://leetcode.com/problems/most-expensive-item-that-can-not-be-bought)

[Tài liệu tiếng Trung](/solution/2900-2999/2979.Most%20Expensive%20Item%20That%20Can%20Not%20Be%20Bought/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số <strong>nguyên tố</strong> <strong>phân biệt</strong> <code>primeOne</code> và <code>primeTwo</code>.</p>

<p>Alice và Bob đang đi chợ. Chợ có <strong>vô hạn</strong> mặt hàng, với <strong>bất kỳ</strong> số nguyên dương <code>x</code> nào cũng có một mặt hàng có giá <code>x</code>. Alice có <strong>vô hạn</strong> đồng xu mệnh giá <code>primeOne</code> và <code>primeTwo</code>. Cô ấy muốn mua một số mặt hàng để tặng Bob và cần biết mặt hàng <strong>đắt nhất</strong> mà cô ấy <strong>không thể mua</strong>.</p>

<p>Trả về <em>giá của mặt hàng <strong>đắt nhất</strong> mà Alice không thể mua để tặng Bob</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> primeOne = 2, primeTwo = 5
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Giá của các mặt hàng không thể mua là [1,3]. Có thể chứng minh rằng mọi mặt hàng có giá lớn hơn 3 đều có thể mua được bằng cách kết hợp các đồng xu mệnh giá 2 và 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> primeOne = 5, primeTwo = 7
<strong>Đầu ra:</strong> 23
<strong>Giải thích:</strong> Giá của các mặt hàng không thể mua là [1,2,3,4,6,8,9,11,13,16,18,23]. Có thể chứng minh rằng mọi mặt hàng có giá lớn hơn 23 đều có thể mua được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt; primeOne, primeTwo &lt; 10<sup>4</sup></code></li>
	<li><code>primeOne</code>, <code>primeTwo</code> là các số nguyên tố.</li>
	<li><code>primeOne * primeTwo &lt; 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Định lý Chicken McNugget

<!-- thinking:start -->

> **Tư duy**
>
> Cả hai mệnh giá đều là số nguyên tố, nên nguyên tố cùng nhau; các giá trị có thể biểu diễn có dạng $a\cdot primeOne+b\cdot primeTwo$. Định lý Chicken McNugget cho biết số nguyên lớn nhất không thể biểu diễn là $ab-a-b$, vì vậy không cần dùng knapsack.
>
> Trả về tích trừ đi hai số nguyên tố.

<!-- thinking:end -->

Theo Định lý Chicken McNugget, với hai số nguyên dương nguyên tố cùng nhau $a$ và $b$, số lớn nhất không thể biểu diễn dưới dạng tổ hợp của $a$ và $b$ là $ab - a - b$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostExpensiveItem(self, primeOne: int, primeTwo: int) -> int:
        return primeOne * primeTwo - primeOne - primeTwo
```

#### Java

```java
class Solution {
    public int mostExpensiveItem(int primeOne, int primeTwo) {
        return primeOne * primeTwo - primeOne - primeTwo;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int mostExpensiveItem(int primeOne, int primeTwo) {
        return primeOne * primeTwo - primeOne - primeTwo;
    }
};
```

#### Go

```go
func mostExpensiveItem(primeOne int, primeTwo int) int {
	return primeOne*primeTwo - primeOne - primeTwo
}
```

#### TypeScript

```ts
function mostExpensiveItem(primeOne: number, primeTwo: number): number {
    return primeOne * primeTwo - primeOne - primeTwo;
}
```

#### Rust

```rust
impl Solution {
    pub fn most_expensive_item(prime_one: i32, prime_two: i32) -> i32 {
        prime_one * prime_two - prime_one - prime_two
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
