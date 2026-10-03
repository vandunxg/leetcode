---
comments: true
difficulty: Medium
rating: 1680
source: Weekly Contest 317 Q3
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [2457. Minimum Addition to Make Integer Beautiful](https://leetcode.com/problems/minimum-addition-to-make-integer-beautiful)

[中文文档](/solution/2400-2499/2457.Minimum%20Addition%20to%20Make%20Integer%20Beautiful/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên dương <code>n</code> và <code>target</code>.</p>

<p>Một số nguyên được gọi là <strong>đẹp</strong> nếu tổng các chữ số của nó nhỏ hơn hoặc bằng <code>target</code>.</p>

<p>Hãy trả về <em>số nguyên <strong>không âm</strong> nhỏ nhất </em><code>x</code><em> sao cho </em><code>n + x</code><em> là số đẹp</em>. Dữ liệu đầu vào được tạo sao cho luôn có thể biến <code>n</code> thành số đẹp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 16, target = 6
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ban đầu n là 16 và tổng các chữ số của nó là 1 + 6 = 7. Sau khi cộng 4, n trở thành 20 và tổng các chữ số trở thành 2 + 0 = 2. Có thể chứng minh rằng không thể làm cho n trở thành số đẹp bằng cách cộng một số nguyên không âm nhỏ hơn 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 467, target = 6
<strong>Đầu ra:</strong> 33
<strong>Giải thích:</strong> Ban đầu n là 467 và tổng các chữ số của nó là 4 + 6 + 7 = 17. Sau khi cộng 33, n trở thành 500 và tổng các chữ số trở thành 5 + 0 + 0 = 5. Có thể chứng minh rằng không thể làm cho n trở thành số đẹp bằng cách cộng một số nguyên không âm nhỏ hơn 33.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, target = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Ban đầu n là 1 và tổng các chữ số của nó là 1, đã nhỏ hơn hoặc bằng target.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>12</sup></code></li>
	<li><code>1 &lt;= target &lt;= 150</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho luôn có thể biến <code>n</code> thành số đẹp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 10^{12}$, ta cộng một $x$ nhỏ nhất sao cho tổng các chữ số $\le target$. Khi đưa chữ số khác 0 thấp nhất lên một bậc, ta biến phần đuôi thành các số 0 nên tổng chữ số giảm.
>
> Khi $n+x$ vẫn chưa thỏa mãn, ta tìm lũy thừa của 10 $p$ ứng với chữ số khác 0 thấp nhất, rồi đặt $x$ sao cho phần tiền tố cộng 1, nhân với $p$, rồi trừ $n$. Lặp lại cho đến khi tổng các chữ số thỏa mãn.

<!-- thinking:end -->

Ta định nghĩa hàm $f(x)$ biểu diễn tổng các chữ số của số nguyên $x$. Bài toán yêu cầu tìm số nguyên không âm nhỏ nhất $x$ sao cho $f(n + x) \leq target$.

Nếu tổng các chữ số của $y = n+x$ lớn hơn $target$, ta có thể lặp các thao tác sau để giảm tổng chữ số của $y$ xuống nhỏ hơn hoặc bằng $target$:

- Tìm chữ số khác 0 ở vị trí thấp nhất của $y$, giảm nó về $0$, rồi tăng chữ số ở vị trí cao hơn một bậc lên $1$;
- Cập nhật $x$ và tiếp tục thao tác trên cho đến khi tổng các chữ số của $n+x$ nhỏ hơn hoặc bằng $target$.

Sau khi vòng lặp kết thúc, trả về $x$.

Ví dụ, với $n=467$ và $target=6$, quá trình thay đổi $n$ như sau:

$$
\begin{aligned}
& 467 \rightarrow 470 \rightarrow 500 \\
\end{aligned}
$$

Độ phức tạp thời gian là $O(\log^2 n)$, trong đó $n$ là số nguyên được cho trong bài toán. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeIntegerBeautiful(self, n: int, target: int) -> int:
        def f(x: int) -> int:
            y = 0
            while x:
                y += x % 10
                x //= 10
            return y

        x = 0
        while f(n + x) > target:
            y = n + x
            p = 10
            while y % 10 == 0:
                y //= 10
                p *= 10
            x = (y // 10 + 1) * p - n
        return x
```

#### Java

```java
class Solution {
    public long makeIntegerBeautiful(long n, int target) {
        long x = 0;
        while (f(n + x) > target) {
            long y = n + x;
            long p = 10;
            while (y % 10 == 0) {
                y /= 10;
                p *= 10;
            }
            x = (y / 10 + 1) * p - n;
        }
        return x;
    }

    private int f(long x) {
        int y = 0;
        while (x > 0) {
            y += x % 10;
            x /= 10;
        }
        return y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long makeIntegerBeautiful(long long n, int target) {
        using ll = long long;
        auto f = [](ll x) {
            int y = 0;
            while (x) {
                y += x % 10;
                x /= 10;
            }
            return y;
        };

        ll x = 0;
        while (f(n + x) > target) {
            ll y = n + x;
            ll p = 10;
            while (y % 10 == 0) {
                y /= 10;
                p *= 10;
            }
            x = (y / 10 + 1) * p - n;
        }
        return x;
    }
};
```

#### Go

```go
func makeIntegerBeautiful(n int64, target int) (x int64) {
	f := func(x int64) (y int) {
		for ; x > 0; x /= 10 {
			y += int(x % 10)
		}
		return
	}
	for f(n+x) > target {
		y := n + x
		var p int64 = 10
		for y%10 == 0 {
			y /= 10
			p *= 10
		}
		x = (y/10+1)*p - n
	}
	return
}
```

#### TypeScript

```ts
function makeIntegerBeautiful(n: number, target: number): number {
    const f = (x: number): number => {
        let y = 0;
        for (; x > 0; x = Math.floor(x / 10)) {
            y += x % 10;
        }
        return y;
    };

    let x = 0;
    while (f(n + x) > target) {
        let y = n + x;
        let p = 10;
        while (y % 10 === 0) {
            y = Math.floor(y / 10);
            p *= 10;
        }
        x = (Math.floor(y / 10) + 1) * p - n;
    }
    return x;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
