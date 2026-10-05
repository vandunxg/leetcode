---
comments: true
difficulty: Easy
rating: 1210
source: Weekly Contest 505 Q1
tags:
    - Bit Manipulation
    - Dynamic Programming
    - Enumeration
---

<!-- problem:start -->

# [3954. Sum of Compatible Numbers in Range I](https://leetcode.com/problems/sum-of-compatible-numbers-in-range-i)

[Tài liệu tiếng Trung](/solution/3900-3999/3954.Sum%20of%20Compatible%20Numbers%20in%20Range%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>n</code> và <code>k</code>.</p>

<p>Một số nguyên <strong>dương</strong> <code>x</code> được gọi là <strong>tương thích</strong> nếu thỏa mãn cả hai điều kiện sau:</p>

<ul>
    <li><code>abs(n - x) &lt;= k</code></li>
    <li><code>(n &amp; x) == 0</code></li>
</ul>

<p>Trả về tổng của tất cả các số nguyên <strong>tương thích</strong> <code>x</code>.</p>

<p><strong>Lưu ý</strong>:</p>

<ul>
    <li>Ở đây, <code>&amp;</code> là toán tử <strong>AND theo bit</strong>.</li>
    <li><strong>Khoảng cách tuyệt đối</strong> giữa hai số nguyên <code>i</code> và <code>j</code> được định nghĩa là <code>abs(i - j)</code>.</li>
</ul>
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số nguyên tương thích là:</p>

<ul>
	<li><code>x = 1</code>, vì <code>abs(2 - 1) = 1</code> và <code>2 &amp; 1 = 0</code>.</li>
	<li><code>x = 4</code>, vì <code>abs(2 - 4) = 2</code> và <code>2 &amp; 4 = 0</code>.</li>
	<li><code>x = 5</code>, vì <code>abs(2 - 5) = 3</code> và <code>2 &amp; 5 = 0</code>.</li>
</ul>

<p>Do đó, đáp án là <code>1 + 4 + 5 = 10</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có số nguyên tương thích nào trong khoảng <code>[4, 6]</code>. Do đó, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng $[n-k,n+k]$ có độ dài $O(k)$ và $k\le 100$, nên ta kiểm tra $n\&x=0$ với từng $x$ rồi cộng các giá trị thỏa mãn. Đầu mút trái được chặn dưới tại $1$ để bỏ qua các số nguyên không dương.
>
> Không cần dùng digit-DP.

<!-- thinking:end -->

Ta duyệt $x$ trong khoảng $[\max(1, n - k), n + k]$. Nếu kết quả AND theo bit của $n$ và $x$ bằng $0$, ta cộng $x$ vào đáp án.

Sau khi kết thúc vòng lặp, chỉ cần trả về đáp án.

Độ phức tạp thời gian là $O(k)$, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfGoodIntegers(self, n: int, k: int) -> int:
        ans = 0
        for x in range(max(1, n - k), n + k + 1):
            if (n & x) == 0:
                ans += x
        return ans
```

#### Java

```java
class Solution {
    public int sumOfGoodIntegers(int n, int k) {
        int ans = 0;
        int start = Math.max(1, n - k);
        int end = n + k;
        for (int x = start; x <= end; x++) {
            if ((n & x) == 0) {
                ans += x;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfGoodIntegers(int n, int k) {
        int ans = 0;
        int start = max(1, n - k);
        int end = n + k;
        for (int x = start; x <= end; ++x) {
            if ((n & x) == 0) {
                ans += x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sumOfGoodIntegers(n int, k int) (ans int) {
	start := max(1, n-k)
	end := n + k
	for x := start; x <= end; x++ {
		if (n & x) == 0 {
			ans += x
		}
	}
	return
}
```

#### TypeScript

```ts
function sumOfGoodIntegers(n: number, k: number): number {
    let ans = 0;
    const start = Math.max(1, n - k);
    const end = n + k;
    for (let x = start; x <= end; x++) {
        if ((n & x) === 0) {
            ans += x;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
