---
comments: true
difficulty: Hard
tags:
    - Math
    - Enumeration
---

<!-- problem:start -->

# [829. Consecutive Numbers Sum](https://leetcode.com/problems/consecutive-numbers-sum)

[中文文档](/solution/0800-0899/0829.Consecutive%20Numbers%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy trả về <em>số cách biểu diễn </em><code>n</code><em> thành tổng của các số nguyên dương liên tiếp.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 5 = 2 + 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 9
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 9 = 4 + 5 = 2 + 3 + 4
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 15
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 15 = 8 + 7 = 4 + 5 + 6 = 1 + 2 + 3 + 4 + 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Suy luận toán học

<!-- thinking:start -->

> **Tư duy**
>
> Biểu diễn $n$ thành tổng của $k$ số nguyên dương liên tiếp tương đương với $n=k\cdot a+k(k-1)/2$. $n$ có thể lên đến $10^9$, nên ta liệt kê độ dài $k$ thay vì số hạng đầu.
>
> $2n/k-k+1$ phải là số nguyên dương chẵn. Chỉ cần thử các $k$ thỏa mãn $k(k+1)\le 2n$ và đếm những độ dài thỏa điều kiện chia hết.

<!-- thinking:end -->

Các số nguyên dương liên tiếp tạo thành một cấp số cộng có công sai $d = 1$. Gọi số hạng đầu là $a$ và số lượng số hạng là $k$. Khi đó, $n = (a + a + k - 1) \times k / 2$, tương đương với $n \times 2 = (a \times 2 + k - 1) \times k$. Từ đây, ta suy ra $k$ phải chia hết $n \times 2$, đồng thời $(n \times 2) / k - k + 1$ phải là số chẵn.

Vì $a \geq 1$, suy ra $n \times 2 = (a \times 2 + k - 1) \times k \geq k \times (k + 1)$.

Tóm lại, ta có các điều kiện:

1. $n \times 2$ phải chia hết cho $k$;
2. $k \times (k + 1) \leq n \times 2$;
3. $(n \times 2) / k - k + 1$ phải là số chẵn.

Ta bắt đầu liệt kê từ $k = 1$ và dừng khi $k \times (k + 1) > n \times 2$. Trong quá trình đó, kiểm tra xem $n \times 2$ có chia hết cho $k$ hay không, đồng thời $(n \times 2) / k - k + 1$ có phải số chẵn hay không. Nếu thỏa cả hai điều kiện, tăng đáp án lên một.

Sau khi liệt kê xong, trả về đáp án.

Độ phức tạp thời gian là $O(\sqrt{n})$, trong đó $n$ là số nguyên dương đã cho. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def consecutiveNumbersSum(self, n: int) -> int:
        n <<= 1
        ans, k = 0, 1
        while k * (k + 1) <= n:
            if n % k == 0 and (n // k - k + 1) % 2 == 0:
                ans += 1
            k += 1
        return ans
```

#### Java

```java
class Solution {

    public int consecutiveNumbersSum(int n) {
        n <<= 1;
        int ans = 0;
        for (int k = 1; k * (k + 1) <= n; ++k) {
            if (n % k == 0 && (n / k + 1 - k) % 2 == 0) {
                ++ans;
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
    int consecutiveNumbersSum(int n) {
        n <<= 1;
        int ans = 0;
        for (int k = 1; k * (k + 1) <= n; ++k) {
            if (n % k == 0 && (n / k + 1 - k) % 2 == 0) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func consecutiveNumbersSum(n int) int {
	n <<= 1
	ans := 0
	for k := 1; k*(k+1) <= n; k++ {
		if n%k == 0 && (n/k+1-k)%2 == 0 {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function consecutiveNumbersSum(n: number): number {
    let ans = 0;
    n <<= 1;
    for (let k = 1; k * (k + 1) <= n; ++k) {
        if (n % k === 0 && (Math.floor(n / k) + 1 - k) % 2 === 0) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
