---
comments: true
difficulty: Medium
rating: 1662
source: Weekly Contest 330 Q2
tags:
    - Recursion
    - Math
---

<!-- problem:start -->

# [2550. Count Collisions of Monkeys on a Polygon](https://leetcode.com/problems/count-collisions-of-monkeys-on-a-polygon)

[中文文档](/solution/2500-2599/2550.Count%20Collisions%20of%20Monkeys%20on%20a%20Polygon/README.md)

## Mô tả

<!-- description:start -->

<p>Có một đa giác lồi đều với <code>n</code> đỉnh. Các đỉnh được đánh số từ <code>0</code> đến <code>n - 1</code> theo chiều kim đồng hồ, và mỗi đỉnh có <strong>chính xác một con khỉ</strong>. Hình dưới đây minh họa một đa giác lồi có <code>6</code> đỉnh.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2550.Count%20Collisions%20of%20Monkeys%20on%20a%20Polygon/images/hexagon.jpg" style="width: 300px; height: 293px;" />
<p>Đồng thời, mỗi con khỉ di chuyển đến một đỉnh kề. <strong>Va chạm</strong> xảy ra nếu sau khi di chuyển có ít nhất hai con khỉ ở cùng một đỉnh hoặc chúng giao nhau trên một cạnh.</p>

<p>Hãy trả về số cách các con khỉ có thể di chuyển sao cho xảy ra ít nhất <strong>một va chạm</strong>. Vì đáp án có thể rất lớn, hãy trả về phần dư khi chia đáp án cho <code>10<sup>9 </sup>+ 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có tổng cộng 8 cách di chuyển khác nhau.<br />
Hai cách khiến chúng va chạm tại một thời điểm nào đó là:</p>

<ul>
	<li>Khỉ 1 di chuyển theo chiều kim đồng hồ; khỉ 2 di chuyển theo chiều ngược kim đồng hồ; khỉ 3 di chuyển theo chiều kim đồng hồ. Khỉ 1 và khỉ 2 va chạm.</li>
	<li>Khỉ 1 di chuyển theo chiều ngược kim đồng hồ; khỉ 2 di chuyển theo chiều ngược kim đồng hồ; khỉ 3 di chuyển theo chiều kim đồng hồ. Khỉ 1 và khỉ 3 va chạm.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học (Lũy thừa nhanh)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi con khỉ trên đa giác đều $n$ đỉnh di chuyển theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ; ta cần đếm các cách di chuyển có va chạm. Có $2^n$ cách gán hướng di chuyển, và chỉ có hai cách tất cả các con khỉ cùng di chuyển theo một hướng là không xảy ra va chạm.
>
> Vì $n$ có thể đạt tới $10^9$, ta không thể duyệt từng cách. Phép lũy thừa modulo cho ta $2^n$, sau đó trừ $2$ và xử lý modulo an toàn khi kết quả âm.

<!-- thinking:end -->

Theo mô tả của đề bài, mỗi con khỉ có hai cách di chuyển: theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ. Vì vậy, có tổng cộng $2^n$ cách di chuyển. Chỉ có hai cách di chuyển không xảy ra va chạm, đó là tất cả các con khỉ cùng di chuyển theo chiều kim đồng hồ hoặc tất cả cùng di chuyển theo chiều ngược kim đồng hồ. Do đó, số cách di chuyển có va chạm là $2^n - 2$.

Ta có thể dùng lũy thừa nhanh để tính giá trị của $2^n$, sau đó dùng $2^n - 2$ để tính số cách di chuyển có va chạm, cuối cùng lấy phần dư khi chia cho $10^9 + 7$.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là số con khỉ. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def monkeyMove(self, n: int) -> int:
        mod = 10**9 + 7
        return (pow(2, n, mod) - 2) % mod
```

#### Java

```java
class Solution {
    public int monkeyMove(int n) {
        final int mod = (int) 1e9 + 7;
        return (qpow(2, n, mod) - 2 + mod) % mod;
    }

    private int qpow(long a, int n, int mod) {
        long ans = 1;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ans = ans * a % mod;
            }
            a = a * a % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int monkeyMove(int n) {
        const int mod = 1e9 + 7;
        using ll = long long;
        auto qpow = [&](ll a, int n) {
            ll ans = 1;
            for (; n; n >>= 1) {
                if (n & 1) {
                    ans = ans * a % mod;
                }
                a = a * a % mod;
            }
            return ans;
        };
        return (qpow(2, n) - 2 + mod) % mod;
    }
};
```

#### Go

```go
func monkeyMove(n int) int {
	const mod = 1e9 + 7
	qpow := func(a, n int) int {
		ans := 1
		for ; n > 0; n >>= 1 {
			if n&1 == 1 {
				ans = ans * a % mod
			}
			a = a * a % mod
		}
		return ans
	}
	return (qpow(2, n) - 2 + mod) % mod
}
```

#### TypeScript

```ts
function monkeyMove(n: number): number {
    const mod = 10 ** 9 + 7;
    const qpow = (a: number, n: number): number => {
        let ans = 1n;
        for (; n; n >>>= 1) {
            if (n & 1) {
                ans = (ans * BigInt(a)) % BigInt(mod);
            }
            a = Number((BigInt(a) * BigInt(a)) % BigInt(mod));
        }
        return Number(ans);
    };
    return (qpow(2, n) - 2 + mod) % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
