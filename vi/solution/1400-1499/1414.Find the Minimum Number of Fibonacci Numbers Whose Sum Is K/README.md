---
comments: true
difficulty: Medium
rating: 1465
source: Biweekly Contest 24 Q2
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [1414. Find the Minimum Number of Fibonacci Numbers Whose Sum Is K](https://leetcode.com/problems/find-the-minimum-number-of-fibonacci-numbers-whose-sum-is-k)

[中文文档](/solution/1400-1499/1414.Find%20the%20Minimum%20Number%20of%20Fibonacci%20Numbers%20Whose%20Sum%20Is%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên&nbsp;<code>k</code>, <em>hãy trả về số lượng nhỏ nhất các số Fibonacci có tổng bằng </em><code>k</code>. Có thể sử dụng cùng một số Fibonacci nhiều lần.</p>

<p>Các số Fibonacci được định nghĩa như sau:</p>

<ul>
	<li><code>F<sub>1</sub> = 1</code></li>
	<li><code>F<sub>2</sub> = 1</code></li>
	<li><code>F<sub>n</sub> = F<sub>n-1</sub> + F<sub>n-2</sub></code> với <code>n &gt; 2.</code></li>
</ul>
Với các ràng buộc đã cho, đảm bảo luôn tìm được các số Fibonacci có tổng bằng <code>k</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 7
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các số Fibonacci là: 1, 1, 2, 3, 5, 8, 13, ...
Với k = 7, ta có thể sử dụng 2 + 5 = 7.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 10
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Với k = 10, ta có thể sử dụng 2 + 8 = 10.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 19
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Với k = 19, ta có thể sử dụng 1 + 5 + 13 = 19.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Vì $k\le 10^9$, không cần tìm kiếm theo kiểu unbounded knapsack trên các số Fibonacci. Mọi số nguyên dương đều có biểu diễn Zeckendorf dưới dạng tổng của các số Fibonacci phân biệt.
>
> Liên tục trừ đi số Fibonacci lớn nhất không vượt quá $k$. Nếu vẫn có thể sử dụng số hạng trước đó, thì trước đó đã có thể chọn một số hạng tiếp theo lớn hơn. Sinh các số cho đến khi vượt qua $k$ một chút, sau đó duyệt ngược.

<!-- thinking:end -->

Mỗi lần, ta có thể tham lam chọn số Fibonacci lớn nhất không vượt quá $k$, sau đó trừ số này khỏi $k$ và tăng đáp án lên một. Lặp lại quá trình này cho đến khi $k = 0$.

Vì mỗi lần ta tham lam chọn số Fibonacci lớn nhất không vượt quá $k$, giả sử số này là $b$, số trước đó là $a$, và số tiếp theo là $c$. Sau khi trừ $b$ khỏi $k$, giá trị thu được nhỏ hơn $a$, nghĩa là sau khi chọn $b$, ta sẽ không chọn $a$. Bởi vì nếu có thể chọn $a$, thì trước đó ta đã có thể tham lam chọn số Fibonacci tiếp theo $c$ thay cho $b$, trái với giả định. Do đó, sau khi chọn $b$, ta có thể tiếp tục giảm số Fibonacci một cách tham lam.

Độ phức tạp thời gian là $O(\log k)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMinFibonacciNumbers(self, k: int) -> int:
        a = b = 1
        while b <= k:
            a, b = b, a + b
        ans = 0
        while k:
            if k >= b:
                k -= b
                ans += 1
            a, b = b - a, a
        return ans
```

#### Java

```java
class Solution {
    public int findMinFibonacciNumbers(int k) {
        int a = 1, b = 1;
        while (b <= k) {
            int c = a + b;
            a = b;
            b = c;
        }
        int ans = 0;
        while (k > 0) {
            if (k >= b) {
                k -= b;
                ++ans;
            }
            int c = b - a;
            b = a;
            a = c;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMinFibonacciNumbers(int k) {
        int a = 1, b = 1;
        while (b <= k) {
            int c = a + b;
            a = b;
            b = c;
        }
        int ans = 0;
        while (k > 0) {
            if (k >= b) {
                k -= b;
                ++ans;
            }
            int c = b - a;
            b = a;
            a = c;
        }
        return ans;
    }
};
```

#### Go

```go
func findMinFibonacciNumbers(k int) (ans int) {
	a, b := 1, 1
	for b <= k {
		c := a + b
		a = b
		b = c
	}

	for k > 0 {
		if k >= b {
			k -= b
			ans++
		}
		c := b - a
		b = a
		a = c
	}
	return
}
```

#### TypeScript

```ts
function findMinFibonacciNumbers(k: number): number {
    let [a, b] = [1, 1];
    while (b <= k) {
        let c = a + b;
        a = b;
        b = c;
    }

    let ans = 0;
    while (k > 0) {
        if (k >= b) {
            k -= b;
            ans++;
        }
        let c = b - a;
        b = a;
        a = c;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_min_fibonacci_numbers(mut k: i32) -> i32 {
        let mut a = 1;
        let mut b = 1;
        while b <= k {
            let c = a + b;
            a = b;
            b = c;
        }

        let mut ans = 0;
        while k > 0 {
            if k >= b {
                k -= b;
                ans += 1;
            }
            let c = b - a;
            b = a;
            a = c;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
