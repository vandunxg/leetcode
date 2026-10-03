---
comments: true
difficulty: Medium
rating: 1257
source: Biweekly Contest 72 Q2
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [2177. Find Three Consecutive Integers That Sum to a Given Number](https://leetcode.com/problems/find-three-consecutive-integers-that-sum-to-a-given-number)

[中文文档](/solution/2100-2199/2177.Find%20Three%20Consecutive%20Integers%20That%20Sum%20to%20a%20Given%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>num</code>, hãy trả về <em>ba số nguyên liên tiếp (dưới dạng một mảng đã sắp xếp)</em><em> có <strong>tổng</strong> bằng </em><code>num</code>. Nếu <code>num</code> không thể được biểu diễn dưới dạng tổng của ba số nguyên liên tiếp, hãy trả về<em> một mảng <strong>rỗng</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 33
<strong>Đầu ra:</strong> [10,11,12]
<strong>Giải thích:</strong> 33 có thể được biểu diễn dưới dạng 10 + 11 + 12 = 33.
10, 11, 12 là 3 số nguyên liên tiếp, nên ta trả về [10, 11, 12].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 4
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không có cách nào biểu diễn 4 dưới dạng tổng của 3 số nguyên liên tiếp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= num &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Ba số nguyên liên tiếp có tổng bằng $3x$, vì vậy $\textit{num}$ phải là bội của $3$, và giá trị ở giữa là $\textit{num}/3$.
>
> Nếu phần dư bằng không, trả về $[x-1,x,x+1]$; ngược lại, trả về một danh sách rỗng.
>
> Phép kiểm tra và việc xây dựng kết quả đều có độ phức tạp $O(1)$.

<!-- thinking:end -->

Giả sử ba số nguyên liên tiếp là $x-1$, $x$ và $x+1$. Tổng của chúng là $3x$, vì vậy $\textit{num}$ phải là bội của $3$. Nếu $\textit{num}$ không phải là bội của $3$, nó không thể được biểu diễn dưới dạng tổng của ba số nguyên liên tiếp, và ta trả về một mảng rỗng. Ngược lại, đặt $x = \frac{\textit{num}}{3}$, khi đó $x-1$, $x$ và $x+1$ là ba số nguyên liên tiếp có tổng bằng $\textit{num}$.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfThree(self, num: int) -> List[int]:
        x, mod = divmod(num, 3)
        return [] if mod else [x - 1, x, x + 1]
```

#### Java

```java
class Solution {
    public long[] sumOfThree(long num) {
        if (num % 3 != 0) {
            return new long[] {};
        }
        long x = num / 3;
        return new long[] {x - 1, x, x + 1};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> sumOfThree(long long num) {
        if (num % 3) {
            return {};
        }
        long long x = num / 3;
        return {x - 1, x, x + 1};
    }
};
```

#### Go

```go
func sumOfThree(num int64) []int64 {
	if num%3 != 0 {
		return []int64{}
	}
	x := num / 3
	return []int64{x - 1, x, x + 1}
}
```

#### TypeScript

```ts
function sumOfThree(num: number): number[] {
    if (num % 3) {
        return [];
    }
    const x = Math.floor(num / 3);
    return [x - 1, x, x + 1];
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_of_three(num: i64) -> Vec<i64> {
        if num % 3 != 0 {
            return Vec::new();
        }
        let x = num / 3;
        vec![x - 1, x, x + 1]
    }
}
```

#### JavaScript

```js
/**
 * @param {number} num
 * @return {number[]}
 */
var sumOfThree = function (num) {
    if (num % 3) {
        return [];
    }
    const x = Math.floor(num / 3);
    return [x - 1, x, x + 1];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
