---
comments: true
difficulty: Easy
rating: 1282
source: Weekly Contest 238 Q1
tags:
    - Math
---

<!-- problem:start -->

# [1837. Sum of Digits in Base K](https://leetcode.com/problems/sum-of-digits-in-base-k)

[中文文档](/solution/1800-1899/1837.Sum%20of%20Digits%20in%20Base%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> (ở cơ số <code>10</code>) và một cơ số <code>k</code>, hãy trả về <em><strong>tổng</strong> các chữ số của </em><code>n</code><em> <strong>sau khi</strong> chuyển </em><code>n</code><em> từ cơ số </em><code>10</code><em> sang cơ số </em><code>k</code>.</p>

<p>Sau khi chuyển đổi, mỗi chữ số được hiểu là một số ở cơ số <code>10</code>, và tổng cần được trả về ở cơ số <code>10</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 34, k = 6
<strong>Đầu ra:</strong> 9
<strong>Giải thích: </strong>34 (cơ số 10) được biểu diễn ở cơ số 6 là 54. 5 + 4 = 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10, k = 10
<strong>Đầu ra:</strong> 1
<strong>Giải thích: </strong>n đã ở cơ số 10. 1 + 0 = 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>2 &lt;= k &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tổng các chữ số của $n$ ở cơ số $k$. Không cần tạo ra chuỗi biểu diễn các chữ số.
>
> Liên tục cộng $n\bmod k$ rồi thay $n$ bằng $n/k$ cho đến khi $n=0$. Đây chính là cách khai triển ở cơ số $k$.

<!-- thinking:end -->

Ta chia $n$ cho $k$ và lấy phần dư cho đến khi phần thương bằng $0$. Tổng các phần dư chính là kết quả.

Độ phức tạp thời gian là $O(\log_{k}n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumBase(self, n: int, k: int) -> int:
        ans = 0
        while n:
            ans += n % k
            n //= k
        return ans
```

#### Java

```java
class Solution {
    public int sumBase(int n, int k) {
        int ans = 0;
        while (n != 0) {
            ans += n % k;
            n /= k;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumBase(int n, int k) {
        int ans = 0;
        while (n) {
            ans += n % k;
            n /= k;
        }
        return ans;
    }
};
```

#### Go

```go
func sumBase(n int, k int) (ans int) {
	for n > 0 {
		ans += n % k
		n /= k
	}
	return
}
```

#### TypeScript

```ts
function sumBase(n: number, k: number): number {
    let ans = 0;
    while (n) {
        ans += n % k;
        n = Math.floor(n / k);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_base(mut n: i32, k: i32) -> i32 {
        let mut ans = 0;
        while n != 0 {
            ans += n % k;
            n /= k;
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number} k
 * @return {number}
 */
var sumBase = function (n, k) {
    let ans = 0;
    while (n) {
        ans += n % k;
        n = Math.floor(n / k);
    }
    return ans;
};
```

#### C

```c
int sumBase(int n, int k) {
    int ans = 0;
    while (n) {
        ans += n % k;
        n /= k;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
