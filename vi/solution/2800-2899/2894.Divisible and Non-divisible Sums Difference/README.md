---
comments: true
difficulty: Easy
rating: 1140
source: Weekly Contest 366 Q1
tags:
    - Math
---

<!-- problem:start -->

# [2894. Divisible and Non-divisible Sums Difference](https://leetcode.com/problems/divisible-and-non-divisible-sums-difference)

[中文文档](/solution/2800-2899/2894.Divisible%20and%20Non-divisible%20Sums%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>n</code> và <code>m</code>.</p>

<p>Định nghĩa hai số nguyên như sau:</p>

<ul>
	<li><code>num1</code>: Tổng tất cả các số nguyên trong đoạn <code>[1, n]</code> (cả hai đầu đoạn <strong>đều được tính</strong>) <strong>không chia hết</strong> cho <code>m</code>.</li>
	<li><code>num2</code>: Tổng tất cả các số nguyên trong đoạn <code>[1, n]</code> (cả hai đầu đoạn <strong>đều được tính</strong>) <strong>chia hết</strong> cho <code>m</code>.</li>
</ul>

<p>Trả về <em>số nguyên</em> <code>num1 - num2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10, m = 3
<strong>Đầu ra:</strong> 19
<strong>Giải thích:</strong> Trong ví dụ trên:
- Các số nguyên trong đoạn [1, 10] không chia hết cho 3 là [1,2,4,5,7,8,10], num1 là tổng của các số này = 37.
- Các số nguyên trong đoạn [1, 10] chia hết cho 3 là [3,6,9], num2 là tổng của các số này = 18.
Ta trả về 37 - 18 = 19 làm đáp án.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, m = 6
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Trong ví dụ trên:
- Các số nguyên trong đoạn [1, 5] không chia hết cho 6 là [1,2,3,4,5], num1 là tổng của các số này = 15.
- Các số nguyên trong đoạn [1, 5] chia hết cho 6 là [], num2 là tổng của các số này = 0.
Ta trả về 15 - 0 = 15 làm đáp án.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, m = 1
<strong>Đầu ra:</strong> -15
<strong>Giải thích:</strong> Trong ví dụ trên:
- Các số nguyên trong đoạn [1, 5] không chia hết cho 1 là [], num1 là tổng của các số này = 0.
- Các số nguyên trong đoạn [1, 5] chia hết cho 1 là [1,2,3,4,5], num2 là tổng của các số này = 15.
Ta trả về 0 - 15 = -15 làm đáp án.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, m &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n$ đủ nhỏ, ta có thể duyệt đoạn $[1,n]$: trừ các bội của $m$ và cộng các số còn lại, chính là tính $num_1-num_2$.

<!-- thinking:end -->

Ta duyệt mọi số trong đoạn $[1, n]$. Nếu số đó chia hết cho $m$, ta trừ nó khỏi đáp án. Ngược lại, ta cộng nó vào đáp án.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số nguyên đã cho. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def differenceOfSums(self, n: int, m: int) -> int:
        return sum(i if i % m else -i for i in range(1, n + 1))
```

#### Java

```java
class Solution {
    public int differenceOfSums(int n, int m) {
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            ans += i % m == 0 ? -i : i;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int differenceOfSums(int n, int m) {
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            ans += i % m ? i : -i;
        }
        return ans;
    }
};
```

#### Go

```go
func differenceOfSums(n int, m int) (ans int) {
	for i := 1; i <= n; i++ {
		if i%m == 0 {
			ans -= i
		} else {
			ans += i
		}
	}
	return
}
```

#### TypeScript

```ts
function differenceOfSums(n: number, m: number): number {
    let ans = 0;
    for (let i = 1; i <= n; ++i) {
        ans += i % m ? i : -i;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn difference_of_sums(n: i32, m: i32) -> i32 {
        let mut ans = 0;
        for i in 1..=n {
            if i % m != 0 {
                ans += i;
            } else {
                ans -= i;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
