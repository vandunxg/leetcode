---
comments: true
difficulty: Medium
tags:
    - Math
    - Dynamic Programming
    - Probability and Statistics
---

<!-- problem:start -->

# [808. Soup Servings](https://leetcode.com/problems/soup-servings)

[中文文档](/solution/0800-0899/0808.Soup%20Servings/README.md)

## Mô tả

<!-- description:start -->

<p>Có hai loại súp, <strong>A</strong> và <strong>B</strong>, mỗi loại ban đầu có <code>n</code> mL. Ở mỗi lượt, một trong bốn thao tác múc sau được chọn <em>ngẫu nhiên</em>, mỗi thao tác có xác suất <code>0.25</code> và <strong>độc lập</strong> với các lượt trước:</p>

<ul>
	<li>múc 100 mL súp A và 0 mL súp B</li>
	<li>múc 75 mL súp A và 25 mL súp B</li>
	<li>múc 50 mL súp A và 50 mL súp B</li>
	<li>múc 25 mL súp A và 75 mL súp B</li>
</ul>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Không có thao tác nào múc 0 mL súp A và 100 mL súp B.</li>
	<li>Lượng súp A và B được múc <em>đồng thời</em> trong cùng một lượt.</li>
	<li>Nếu thao tác yêu cầu múc <strong>nhiều hơn</strong> lượng súp còn lại, hãy múc hết phần súp đó.</li>
</ul>

<p>Quá trình dừng ngay sau lượt mà <em>một trong hai loại súp</em> được dùng hết.</p>

<p>Hãy trả về xác suất súp A được dùng hết <em>trước</em> súp B, cộng với một nửa xác suất cả hai loại súp được dùng hết trong <strong>cùng một lượt</strong>. Chấp nhận đáp án có sai số tối đa <code>10<sup>-5</sup></code> so với kết quả thực tế.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 50
<strong>Đầu ra:</strong> 0.62500
<strong>Giải thích:</strong> 
Nếu thực hiện một trong hai thao tác múc đầu tiên, súp A sẽ hết trước.
Nếu thực hiện thao tác thứ ba, súp A và B sẽ hết cùng lúc.
Nếu thực hiện thao tác thứ tư, súp B sẽ hết trước.
Vậy xác suất súp A hết trước cộng với một nửa xác suất súp A và B hết cùng lúc là 0.25 * (1 + 1 + 0.5 + 0) = 0.625.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 100
<strong>Đầu ra:</strong> 0.71875
<strong>Giải thích:</strong> 
Nếu thực hiện thao tác múc thứ nhất, súp A sẽ hết trước.
Nếu thực hiện thao tác múc thứ hai, súp A sẽ hết khi thao tác này được thực hiện lần [1, 2, 3], còn cả A và B sẽ hết khi thực hiện lần thứ 4.
Nếu thực hiện thao tác thứ ba, súp A sẽ hết khi thao tác này được thực hiện lần [1, 2], còn cả A và B sẽ hết khi thực hiện lần thứ 3.
Nếu thực hiện thao tác thứ tư, súp A sẽ hết khi thao tác này được thực hiện lần thứ 1, còn cả A và B sẽ hết khi thực hiện lần thứ 2.
Vậy xác suất súp A hết trước cộng với một nửa xác suất súp A và B hết cùng lúc là 0.71875.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Bốn thao tác làm cạn súp có xác suất như nhau; cần tính xác suất A hết trước (nếu cùng hết thì tính $1/2$). $n$ có thể lên đến $10^9$, nên không thể mô phỏng theo từng mL. Mỗi lần múc lấy đi ít nhất $25\,\mathrm{ml}$, vì vậy có thể thu nhỏ không gian trạng thái xuống $\lceil n/25\rceil$.
>
> $dfs(i,j)$ là xác suất cần tìm khi còn lần lượt $i$ và $j$ đơn vị súp, với các trường hợp cơ sở trả về $1$, $0$ hoặc $1/2$. Khi $n$ lớn, A gần như chắc chắn hết trước, nên code trả về $1$ nếu $n>4800$.

<!-- thinking:end -->

Trong bài này, vì lượng súp trong mỗi thao tác đều là bội số của $25$, ta có thể xem mỗi $25ml$ súp là một đơn vị. Nhờ đó, quy mô dữ liệu giảm còn $\left \lceil \frac{n}{25} \right \rceil$.

Ta định nghĩa hàm $dfs(i, j)$ biểu thị xác suất cần tìm khi còn $i$ đơn vị súp $A$ và $j$ đơn vị súp $B$.

Khi $i \leq 0$ và $j \leq 0$, cả hai loại súp đã hết nên trả về $0.5$. Khi $i \leq 0$, súp $A$ hết trước nên trả về $1$. Khi $j \leq 0$, súp $B$ hết trước nên trả về $0$.

Tiếp theo, có bốn lựa chọn tương ứng với các thao tác:

- Múc $4$ đơn vị súp $A$ và $0$ đơn vị súp $B$;
- Múc $3$ đơn vị súp $A$ và $1$ đơn vị súp $B$;
- Múc $2$ đơn vị súp $A$ và $2$ đơn vị súp $B$;
- Múc $1$ đơn vị súp $A$ và $3$ đơn vị súp $B$.

Mỗi lựa chọn có xác suất $0.25$, do đó ta có công thức:

$$
dfs(i, j) = 0.25 \times (dfs(i - 4, j) + dfs(i - 3, j - 1) + dfs(i - 2, j - 2) + dfs(i - 1, j - 3))
$$

Ta dùng memoization để lưu kết quả của hàm.

Ngoài ra, ta thấy khi $n=4800$, kết quả là $0.999994994426$, trong khi sai số cho phép là $10^{-5}$. Khi $n$ tăng, kết quả tiến gần đến $1$. Vì vậy, nếu $n \gt 4800$, có thể trả về $1$ ngay.

Độ phức tạp thời gian là $O(C^2)$ và độ phức tạp không gian là $O(C^2)$. Trong bài này, $C=200$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def soupServings(self, n: int) -> float:
        @cache
        def dfs(i: int, j: int) -> float:
            if i <= 0 and j <= 0:
                return 0.5
            if i <= 0:
                return 1
            if j <= 0:
                return 0
            return 0.25 * (
                dfs(i - 4, j)
                + dfs(i - 3, j - 1)
                + dfs(i - 2, j - 2)
                + dfs(i - 1, j - 3)
            )

        return 1 if n > 4800 else dfs((n + 24) // 25, (n + 24) // 25)
```

#### Java

```java
class Solution {
    private double[][] f = new double[200][200];

    public double soupServings(int n) {
        return n > 4800 ? 1 : dfs((n + 24) / 25, (n + 24) / 25);
    }

    private double dfs(int i, int j) {
        if (i <= 0 && j <= 0) {
            return 0.5;
        }
        if (i <= 0) {
            return 1.0;
        }
        if (j <= 0) {
            return 0;
        }
        if (f[i][j] > 0) {
            return f[i][j];
        }
        double ans
            = 0.25 * (dfs(i - 4, j) + dfs(i - 3, j - 1) + dfs(i - 2, j - 2) + dfs(i - 1, j - 3));
        f[i][j] = ans;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double soupServings(int n) {
        double f[200][200] = {0.0};
        auto dfs = [&](this auto&& dfs, int i, int j) -> double {
            if (i <= 0 && j <= 0) return 0.5;
            if (i <= 0) return 1;
            if (j <= 0) return 0;
            if (f[i][j] > 0) return f[i][j];
            double ans = 0.25 * (dfs(i - 4, j) + dfs(i - 3, j - 1) + dfs(i - 2, j - 2) + dfs(i - 1, j - 3));
            f[i][j] = ans;
            return ans;
        };
        return n > 4800 ? 1 : dfs((n + 24) / 25, (n + 24) / 25);
    }
};
```

#### Go

```go
func soupServings(n int) float64 {
	if n > 4800 {
		return 1
	}
	f := [200][200]float64{}
	var dfs func(i, j int) float64
	dfs = func(i, j int) float64 {
		if i <= 0 && j <= 0 {
			return 0.5
		}
		if i <= 0 {
			return 1.0
		}
		if j <= 0 {
			return 0
		}
		if f[i][j] > 0 {
			return f[i][j]
		}
		ans := 0.25 * (dfs(i-4, j) + dfs(i-3, j-1) + dfs(i-2, j-2) + dfs(i-1, j-3))
		f[i][j] = ans
		return ans
	}
	return dfs((n+24)/25, (n+24)/25)
}
```

#### TypeScript

```ts
function soupServings(n: number): number {
    const f = Array.from({ length: 200 }, () => Array(200).fill(-1));
    const dfs = (i: number, j: number): number => {
        if (i <= 0 && j <= 0) {
            return 0.5;
        }
        if (i <= 0) {
            return 1;
        }
        if (j <= 0) {
            return 0;
        }
        if (f[i][j] !== -1) {
            return f[i][j];
        }
        f[i][j] =
            0.25 * (dfs(i - 4, j) + dfs(i - 3, j - 1) + dfs(i - 2, j - 2) + dfs(i - 1, j - 3));
        return f[i][j];
    };
    return n >= 4800 ? 1 : dfs(Math.ceil(n / 25), Math.ceil(n / 25));
}
```

#### Rust

```rust
impl Solution {
    pub fn soup_servings(n: i32) -> f64 {
        if n > 4800 {
            return 1.0;
        }
        Self::dfs((n + 24) / 25, (n + 24) / 25)
    }

    fn dfs(i: i32, j: i32) -> f64 {
        static mut F: [[f64; 200]; 200] = [[0.0; 200]; 200];

        unsafe {
            if i <= 0 && j <= 0 {
                return 0.5;
            }
            if i <= 0 {
                return 1.0;
            }
            if j <= 0 {
                return 0.0;
            }
            if F[i as usize][j as usize] > 0.0 {
                return F[i as usize][j as usize];
            }

            let ans = 0.25 * (Self::dfs(i - 4, j) + Self::dfs(i - 3, j - 1) + Self::dfs(i - 2, j - 2) + Self::dfs(i - 1, j - 3));
            F[i as usize][j as usize] = ans;
            ans
        }
    }
}
```

#### C#

```cs
public class Solution {
    private double[,] f = new double[200, 200];

    public double SoupServings(int n) {
        if (n > 4800) {
            return 1.0;
        }

        return Dfs((n + 24) / 25, (n + 24) / 25);
    }

    private double Dfs(int i, int j) {
        if (i <= 0 && j <= 0) {
            return 0.5;
        }
        if (i <= 0) {
            return 1.0;
        }
        if (j <= 0) {
            return 0.0;
        }
        if (f[i, j] > 0) {
            return f[i, j];
        }

        double ans = 0.25 * (Dfs(i - 4, j) + Dfs(i - 3, j - 1) + Dfs(i - 2, j - 2) + Dfs(i - 1, j - 3));
        f[i, j] = ans;
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
