---
comments: true
difficulty: Medium
rating: 2127
source: Weekly Contest 372 Q3
tags:
    - Greedy
    - Bit Manipulation
    - Math
---

<!-- problem:start -->

# [2939. Maximum Xor Product](https://leetcode.com/problems/maximum-xor-product)

[中文文档](/solution/2900-2999/2939.Maximum%20Xor%20Product/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba số nguyên <code>a</code>, <code>b</code> và <code>n</code>, hãy trả về <em><strong>giá trị lớn nhất</strong> của</em> <code>(a XOR x) * (b XOR x)</code> <em>với</em> <code>0 &lt;= x &lt; 2<sup>n</sup></code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về nó theo <strong>modulo</strong> <code>10<sup>9 </sup>+ 7</code>.</p>

<p><strong>Lưu ý</strong> rằng <code>XOR</code> là phép XOR theo bit.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 12, b = 5, n = 4
<strong>Đầu ra:</strong> 98
<strong>Giải thích:</strong> Với x = 2, (a XOR x) = 14 và (b XOR x) = 7. Do đó, (a XOR x) * (b XOR x) = 98.
Có thể chứng minh rằng 98 là giá trị lớn nhất của (a XOR x) * (b XOR x) với mọi 0 &lt;= x &lt; 2<sup>n</sup><span style="font-size: 10.8333px;">.</span>
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 6, b = 7 , n = 5
<strong>Đầu ra:</strong> 930
<strong>Giải thích:</strong> Với x = 25, (a XOR x) = 31 và (b XOR x) = 30. Do đó, (a XOR x) * (b XOR x) = 930.
Có thể chứng minh rằng 930 là giá trị lớn nhất của (a XOR x) * (b XOR x) với mọi 0 &lt;= x &lt; 2<sup>n</sup>.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 1, b = 6, n = 3
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Với x = 5, (a XOR x) = 4 và (b XOR x) = 3. Do đó, (a XOR x) * (b XOR x) = 12.
Có thể chứng minh rằng 12 là giá trị lớn nhất của (a XOR x) * (b XOR x) với mọi 0 &lt;= x &lt; 2<sup>n</sup>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= a, b &lt; 2<sup>50</sup></code></li>
	<li><code>0 &lt;= n &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Phép toán bit

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể XOR cả $a$ và $b$ với một $x$ trên các bit $[0,n)$. Các bit phía trên $n$ được giữ nguyên và được tách ra thành $ax,bx$. Ở những vị trí mà $a$ và $b$ đang bằng nhau, ta đặt bit đó thành 1 ở cả hai số để tăng tích.
>
> Ở những vị trí khác nhau, $x$ chỉ có thể tạo bit $1$ ở một trong hai phía. Tích sẽ lớn hơn khi hai thừa số gần nhau, vì vậy ta gán bit cho số hiện nhỏ hơn. Ta áp dụng tham lam từ các bit cao xuống bit thấp, sau đó lấy kết quả theo modulo đã cho.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể đồng thời gán giá trị cho các bit trong đoạn $[0..n)$ của $a$ và $b$ ở dạng nhị phân để tích của $a$ và $b$ đạt lớn nhất.

Vì vậy, trước tiên ta tách phần của $a$ và $b$ nằm cao hơn $n$ bit, lần lượt ký hiệu là $ax$ và $bx$.

Tiếp theo, ta xét từng bit trong đoạn $[0..n)$ từ cao xuống thấp. Gọi các bit hiện tại của $a$ và $b$ lần lượt là $x$ và $y$.

Nếu $x = y$, ta có thể đồng thời đặt bit hiện tại của $ax$ và $bx$ thành $1$. Do đó, ta cập nhật $ax = ax \mid 1 << i$ và $bx = bx \mid 1 << i$. Ngược lại, nếu $ax < bx$, để tích cuối cùng đạt lớn nhất, ta nên đặt bit hiện tại của $ax$ thành $1$. Nếu không, ta đặt bit hiện tại của $bx$ thành $1$.

Cuối cùng, ta trả về $ax \times bx \bmod (10^9 + 7)$ làm đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số nguyên được cho trong đề bài. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumXorProduct(self, a: int, b: int, n: int) -> int:
        mod = 10**9 + 7
        ax, bx = (a >> n) << n, (b >> n) << n
        for i in range(n - 1, -1, -1):
            x = a >> i & 1
            y = b >> i & 1
            if x == y:
                ax |= 1 << i
                bx |= 1 << i
            elif ax > bx:
                bx |= 1 << i
            else:
                ax |= 1 << i
        return ax * bx % mod
```

#### Java

```java
class Solution {
    public int maximumXorProduct(long a, long b, int n) {
        final int mod = (int) 1e9 + 7;
        long ax = (a >> n) << n;
        long bx = (b >> n) << n;
        for (int i = n - 1; i >= 0; --i) {
            long x = a >> i & 1;
            long y = b >> i & 1;
            if (x == y) {
                ax |= 1L << i;
                bx |= 1L << i;
            } else if (ax < bx) {
                ax |= 1L << i;
            } else {
                bx |= 1L << i;
            }
        }
        ax %= mod;
        bx %= mod;
        return (int) (ax * bx % mod);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumXorProduct(long long a, long long b, int n) {
        const int mod = 1e9 + 7;
        long long ax = (a >> n) << n, bx = (b >> n) << n;
        for (int i = n - 1; ~i; --i) {
            int x = a >> i & 1, y = b >> i & 1;
            if (x == y) {
                ax |= 1LL << i;
                bx |= 1LL << i;
            } else if (ax < bx) {
                ax |= 1LL << i;
            } else {
                bx |= 1LL << i;
            }
        }
        ax %= mod;
        bx %= mod;
        return ax * bx % mod;
    }
};
```

#### Go

```go
func maximumXorProduct(a int64, b int64, n int) int {
	const mod int64 = 1e9 + 7
	ax := (a >> n) << n
	bx := (b >> n) << n
	for i := n - 1; i >= 0; i-- {
		x, y := (a>>i)&1, (b>>i)&1
		if x == y {
			ax |= 1 << i
			bx |= 1 << i
		} else if ax < bx {
			ax |= 1 << i
		} else {
			bx |= 1 << i
		}
	}
	ax %= mod
	bx %= mod
	return int(ax * bx % mod)
}
```

#### TypeScript

```ts
function maximumXorProduct(a: number, b: number, n: number): number {
    const mod = BigInt(1e9 + 7);
    let ax = (BigInt(a) >> BigInt(n)) << BigInt(n);
    let bx = (BigInt(b) >> BigInt(n)) << BigInt(n);
    for (let i = BigInt(n - 1); ~i; --i) {
        const x = (BigInt(a) >> i) & 1n;
        const y = (BigInt(b) >> i) & 1n;
        if (x === y) {
            ax |= 1n << i;
            bx |= 1n << i;
        } else if (ax < bx) {
            ax |= 1n << i;
        } else {
            bx |= 1n << i;
        }
    }
    ax %= mod;
    bx %= mod;
    return Number((ax * bx) % mod);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
