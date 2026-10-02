---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
    - Math
    - Primality Test
---

<!-- problem:start -->

# [762. Prime Number of Set Bits in Binary Representation](https://leetcode.com/problems/prime-number-of-set-bits-in-binary-representation)

[中文文档](/solution/0700-0799/0762.Prime%20Number%20of%20Set%20Bits%20in%20Binary%20Representation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>left</code> và <code>right</code>, hãy trả về <em><strong>số lượng</strong> các số trong đoạn </em><code>[left, right]</code><em> (bao gồm cả hai đầu mút) có <strong>số bit 1 là số nguyên tố</strong> trong biểu diễn nhị phân</em>.</p>

<p>Nhắc lại, <strong>số bit 1</strong> của một số nguyên là số lượng chữ số <code>1</code> khi viết số đó ở dạng nhị phân.</p>

<ul>
	<li>Ví dụ, <code>21</code> viết ở dạng nhị phân là <code>10101</code>, có <code>3</code> bit 1.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> left = 6, right = 10
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
6  -&gt; 110 (2 bit 1, 2 là số nguyên tố)
7  -&gt; 111 (3 bit 1, 3 là số nguyên tố)
8  -&gt; 1000 (1 bit 1, 1 không phải số nguyên tố)
9  -&gt; 1001 (2 bit 1, 2 là số nguyên tố)
10 -&gt; 1010 (2 bit 1, 2 là số nguyên tố)
4 số có số bit 1 là số nguyên tố.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> left = 10, right = 15
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
10 -&gt; 1010 (2 bit 1, 2 là số nguyên tố)
11 -&gt; 1011 (3 bit 1, 3 là số nguyên tố)
12 -&gt; 1100 (2 bit 1, 2 là số nguyên tố)
13 -&gt; 1101 (3 bit 1, 3 là số nguyên tố)
14 -&gt; 1110 (3 bit 1, 3 là số nguyên tố)
15 -&gt; 1111 (4 bit 1, 4 không phải số nguyên tố)
5 số có số bit 1 là số nguyên tố.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= left &lt;= right &lt;= 10<sup>6</sup></code></li>
	<li><code>0 &lt;= right - left &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học + thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Đoạn có độ dài $\le 10^4$ và các giá trị $\le 10^6$, nên mỗi số có nhiều nhất hai mươi bit $1$. Việc đếm bit cho từng số nguyên là hoàn toàn phù hợp.
>
> Chỉ có tám số nguyên tố trong $[1,20]$. Kiểm tra $\textit{bit\_count}$ có thuộc một tập hợp hay không giúp tránh phép thử chia.

<!-- thinking:end -->

Trong bài toán này, cả $\textit{left}$ và $\textit{right}$ đều không vượt quá $10^6$. Vì $2^{20} = 1048576$, số lượng chữ số $1$ trong biểu diễn nhị phân nhiều nhất là $20$. Các số nguyên tố không vượt quá $20$ là $[2, 3, 5, 7, 11, 13, 17, 19]$.

Ta duyệt từng số trong đoạn $[\textit{left},.. \textit{right}]$, đếm số chữ số $1$ trong biểu diễn nhị phân của nó rồi kiểm tra số lượng này có phải số nguyên tố hay không. Nếu đúng, ta tăng đáp án thêm một.

Độ phức tạp thời gian là $O(n\times \log m)$, trong đó $n = \textit{right} - \textit{left} + 1$ và $m$ là số lớn nhất trong đoạn $[\textit{left},.. \textit{right}]$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPrimeSetBits(self, left: int, right: int) -> int:
        primes = {2, 3, 5, 7, 11, 13, 17, 19}
        return sum(i.bit_count() in primes for i in range(left, right + 1))
```

#### Java

```java
class Solution {
    private static Set<Integer> primes = Set.of(2, 3, 5, 7, 11, 13, 17, 19);

    public int countPrimeSetBits(int left, int right) {
        int ans = 0;
        for (int i = left; i <= right; ++i) {
            if (primes.contains(Integer.bitCount(i))) {
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
    int countPrimeSetBits(int left, int right) {
        unordered_set<int> primes{2, 3, 5, 7, 11, 13, 17, 19};
        int ans = 0;
        for (int i = left; i <= right; ++i) {
            ans += primes.count(__builtin_popcount(i));
        }
        return ans;
    }
};
```

#### Go

```go
func countPrimeSetBits(left int, right int) (ans int) {
	primes := map[int]int{}
	for _, v := range []int{2, 3, 5, 7, 11, 13, 17, 19} {
		primes[v] = 1
	}
	for i := left; i <= right; i++ {
		ans += primes[bits.OnesCount(uint(i))]
	}
	return
}
```

#### TypeScript

```ts
function countPrimeSetBits(left: number, right: number): number {
    const primes = new Set<number>([2, 3, 5, 7, 11, 13, 17, 19]);
    let ans = 0;

    for (let i = left; i <= right; i++) {
        const bits = bitCount(i);
        if (primes.has(bits)) {
            ans++;
        }
    }

    return ans;
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_prime_set_bits(left: i32, right: i32) -> i32 {
        let primes = [2, 3, 5, 7, 11, 13, 17, 19];
        let mut ans = 0;

        for i in left..=right {
            let bits = i.count_ones() as i32;
            if primes.contains(&bits) {
                ans += 1;
            }
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int CountPrimeSetBits(int left, int right) {
        var primes = new HashSet<int> { 2, 3, 5, 7, 11, 13, 17, 19 };
        int ans = 0;

        for (int i = left; i <= right; ++i) {
            int bits = BitOperations.PopCount((uint)i);
            if (primes.Contains(bits)) {
                ++ans;
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
