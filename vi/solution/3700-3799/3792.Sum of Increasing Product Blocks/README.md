---
comments: true
difficulty: Medium
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [3792. Sum of Increasing Product Blocks 🔒](https://leetcode.com/problems/sum-of-increasing-product-blocks)

[中文文档](/solution/3700-3799/3792.Sum%20of%20Increasing%20Product%20Blocks/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>.</p>

<p>Một dãy được tạo như sau:</p>

<ul>
	<li>Khối <code>1<sup>st</sup></code> chứa <code>1</code>.</li>
	<li>Khối <code>2<sup>nd</sup></code> chứa <code>2 * 3</code>.</li>
	<li>Khối <code>i<sup>th</sup></code> là tích của <code>i</code> số nguyên liên tiếp tiếp theo.</li>
</ul>

<p>Gọi <code>F(n)</code> là tổng của <code>n</code> khối đầu tiên.</p>

<p>Trả về một số nguyên biểu thị <code>F(n)</code> <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">127</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Khối 1: <code>1</code></li>
	<li>Khối 2: <code>2 * 3 = 6</code></li>
	<li>Khối 3: <code>4 * 5 * 6 = 120</code></li>
</ul>

<p><code>F(3) = 1 + 6 + 120 = 127</code></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6997165</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Khối 1: <code>1</code></li>
	<li>Khối 2: <code>2 * 3 = 6</code></li>
	<li>Khối 3: <code>4 * 5 * 6 = 120</code></li>
	<li>Khối 4: <code>7 * 8 * 9 * 10 = 5040</code></li>
	<li>Khối 5: <code>11 * 12 * 13 * 14 * 15 = 360360</code></li>
	<li>Khối 6: <code>16 * 17 * 18 * 19 * 20 * 21 = 39070080</code></li>
	<li>Khối 7: <code>22 * 23 * 24 * 25 * 26 * 27 * 28 = 5967561600</code></li>
</ul>

<p><code>F(7) = 6006997207 % (10<sup>9</sup> + 7) = 6997165</code></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 1000$ và khối thứ $i$ là tích của $i$ số nguyên liên tiếp. Ta nhân các số trong mỗi khối theo modulo $10^9+7$; tổng số phép tính là $1+2+\cdots+n=O(n^2)$.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp tích của từng khối và cộng dồn vào đáp án. Vì tích có thể rất lớn, cần lấy modulo ở mỗi bước tính toán.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfBlocks(self, n: int) -> int:
        ans = 0
        mod = 10**9 + 7
        k = 1
        for i in range(1, n + 1):
            x = 1
            for j in range(k, k + i):
                x = (x * j) % mod
            ans = (ans + x) % mod
            k += i
        return ans
```

#### Java

```java
class Solution {
    public int sumOfBlocks(int n) {
        final int mod = (int) 1e9 + 7;
        long ans = 0;
        int k = 1;
        for (int i = 1; i <= n; ++i) {
            long x = 1;
            for (int j = k; j < k + i; ++j) {
                x = x * j % mod;
            }
            ans = (ans + x) % mod;
            k += i;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfBlocks(int n) {
        const int mod = 1e9 + 7;
        long long ans = 0;
        int k = 1;
        for (int i = 1; i <= n; ++i) {
            long long x = 1;
            for (int j = k; j < k + i; ++j) {
                x = x * j % mod;
            }
            ans = (ans + x) % mod;
            k += i;
        }
        return ans;
    }
};
```

#### Go

```go
func sumOfBlocks(n int) (ans int) {
	const mod int = 1e9 + 7
	k := 1
	for i := 1; i <= n; i++ {
		x := 1
		for j := k; j < k+i; j++ {
			x = x * j % mod
		}
		ans = (ans + x) % mod
		k += i
	}
	return
}
```

#### TypeScript

```ts
function sumOfBlocks(n: number): number {
    const mod = 1000000007;
    let k = 1;
    let ans = 0;
    for (let i = 1; i <= n; i++) {
        let x = 1;
        for (let j = k; j < k + i; j++) {
            x = (x * j) % mod;
        }
        ans = (ans + x) % mod;
        k += i;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
