---
comments: true
difficulty: Easy
rating: 1265
source: Weekly Contest 330 Q1
tags:
    - Array
    - Hash Table
    - Math
    - Simulation
---

<!-- problem:start -->

# [2549. Count Distinct Numbers on Board](https://leetcode.com/problems/count-distinct-numbers-on-board)

[中文文档](/solution/2500-2599/2549.Count%20Distinct%20Numbers%20on%20Board/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>n</code>, ban đầu được đặt trên một bảng. Trong <code>10<sup>9</sup></code> ngày, mỗi ngày bạn thực hiện quy trình sau:</p>

<ul>
	<li>Với mỗi số <code>x</code> đang có trên bảng, tìm tất cả các số <code>1 &lt;= i &lt;= n</code> sao cho <code>x % i == 1</code>.</li>
	<li>Sau đó, đặt các số đó lên bảng.</li>
</ul>

<p>Trả về <em>số lượng số nguyên <strong>phân biệt</strong> có trên bảng sau khi đã trôi qua</em> <code>10<sup>9</sup></code> <em>ngày</em>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Một khi một số được đặt lên bảng, số đó sẽ ở lại trên bảng cho đến hết quá trình.</li>
	<li><code>%</code>&nbsp;là phép modulo. Ví dụ,&nbsp;<code>14 % 3</code> bằng <code>2</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ban đầu, 5 có trên bảng.
Ngày tiếp theo, 2 và 4 được thêm vào vì 5 % 2 == 1 và 5 % 4 == 1.
Sau ngày đó, 3 được thêm vào bảng vì 4 % 3 == 1.
Sau một tỷ ngày, các số phân biệt trên bảng là 2, 3, 4 và 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Vì 3 % 2 == 1, 2 sẽ được thêm vào bảng.
Sau một tỷ ngày, hai số phân biệt duy nhất trên bảng là 2 và 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tư duy ngược

<!-- thinking:start -->

> **Tư duy**
>
> Nếu $x$ nằm trên bảng và $0<y<x$ với $x\bmod y=1$, ta có thể thêm $y$. Với $n>1$, $n\bmod(n-1)=1$, nên $n-1$ xuất hiện, rồi đến $n-2,\ldots,2$; số $1$ không bao giờ xuất hiện.
>
> Vì vậy, số lượng phần tử phân biệt là $n-1$, hoặc là $1$ khi $n=1$. Không cần mô phỏng quá trình này.

<!-- thinking:end -->

Vì mỗi thao tác trên số $n$ trên bảng cũng khiến số $n-1$ xuất hiện trên bảng, các số cuối cùng trên bảng là $[2,...n]$, tức là có $n-1$ số.

Lưu ý rằng $n$ có thể bằng $1$, nên cần xử lý riêng trường hợp này.

Độ phức tạp thời gian là $O(1)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distinctIntegers(self, n: int) -> int:
        return max(1, n - 1)
```

#### Java

```java
class Solution {
    public int distinctIntegers(int n) {
        return Math.max(1, n - 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int distinctIntegers(int n) {
        return max(1, n - 1);
    }
};
```

#### Go

```go
func distinctIntegers(n int) int {
	return max(1, n-1)
}
```

#### TypeScript

```ts
function distinctIntegers(n: number): number {
    return Math.max(1, n - 1);
}
```

#### Rust

```rust
impl Solution {
    pub fn distinct_integers(n: i32) -> i32 {
        (1).max(n - 1)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
