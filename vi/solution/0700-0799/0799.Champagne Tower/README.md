---
comments: true
difficulty: Medium
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [799. Champagne Tower](https://leetcode.com/problems/champagne-tower)

[中文文档](/solution/0700-0799/0799.Champagne%20Tower/README.md)

## Mô tả

<!-- description:start -->

<p>Ta xếp các ly thành hình kim tự tháp: hàng <strong>thứ nhất</strong> có <code>1</code> ly, hàng <strong>thứ hai</strong> có <code>2</code> ly, cứ tiếp tục như vậy đến hàng thứ 100.&nbsp; Mỗi ly chứa được một cốc champagne.</p>

<p>Sau đó, ta rót một lượng champagne vào chiếc ly trên cùng.&nbsp; Khi ly trên cùng đầy, phần chất lỏng dư sẽ chảy đều sang hai ly ngay bên trái và bên phải.&nbsp; Khi các ly đó đầy, champagne dư tiếp tục chảy đều sang các ly bên trái và bên phải của chúng, cứ như vậy.&nbsp; (Champagne dư từ các ly ở hàng cuối cùng sẽ chảy xuống sàn.)</p>

<p>Ví dụ, sau khi rót một cốc champagne, ly trên cùng sẽ đầy.&nbsp; Sau khi rót hai cốc, hai ly ở hàng thứ hai đầy một nửa.&nbsp; Sau khi rót ba cốc, hai ly đó sẽ đầy, tổng cộng có 3 ly đầy.&nbsp; Sau khi rót bốn cốc, ly giữa ở hàng thứ ba đầy một nửa, còn hai ly ngoài cùng đầy một phần tư, như hình bên dưới.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0799.Champagne%20Tower/images/tower.png" style="height: 241px; width: 350px;" /></p>

<p>Sau khi rót một số nguyên không âm cup champagne, hãy trả về mức độ đầy của ly thứ <code>j<sup>th</sup></code> ở hàng thứ <code>i<sup>th</sup></code> (cả <code>i</code> và <code>j</code> đều được đánh chỉ số từ 0).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> poured = 1, query_row = 1, query_glass = 1
<strong>Đầu ra:</strong> 0.00000
<strong>Giải thích:</strong> Ta rót 1 cốc champagne vào ly trên cùng của tháp (có chỉ số (0, 0)). Không có chất lỏng dư nên tất cả ly bên dưới vẫn rỗng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> poured = 2, query_row = 1, query_glass = 1
<strong>Đầu ra:</strong> 0.50000
<strong>Giải thích:</strong> Ta rót 2 cốc champagne vào ly trên cùng của tháp (có chỉ số (0, 0)). Có một cốc chất lỏng dư. Ly có chỉ số (1, 0) và ly có chỉ số (1, 1) chia đều phần chất lỏng dư, mỗi ly nhận được nửa cốc champagne.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> poured = 100000009, query_row = 33, query_glass = 17
<strong>Đầu ra:</strong> 1.00000
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;=&nbsp;poured &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= query_glass &lt;= query_row&nbsp;&lt; 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Rót $poured$ cốc từ trên cùng rồi đọc lượng champagne trong ly cần truy vấn (tối đa là $1$). Có nhiều nhất $99$ hàng cần xét: mô phỏng lượng tràn.
>
> Lượng vượt quá $1$ được chia đều cho hai ly ở hàng bên dưới. Làm đầy ly rồi phân phối phần tràn cho đến hàng cần truy vấn.

<!-- thinking:end -->

Ta mô phỏng trực tiếp quá trình rót champagne.

Định nghĩa mảng 2 chiều $f$, trong đó $f[i][j]$ là lượng champagne trong ly thứ $j$ ở tầng thứ $i$. Ban đầu, $f[0][0] = poured$.

Với mỗi tầng, nếu lượng champagne trong ly hiện tại $f[i][j]$ lớn hơn $1$, phần dư sẽ chảy xuống hai ly ở tầng tiếp theo. Mỗi ly nhận được $\frac{f[i][j]-1}{2}$, tức lượng champagne trong ly hiện tại trừ $1$ rồi chia cho $2$. Sau đó, cập nhật lượng champagne trong ly hiện tại thành $1$.

Sau khi mô phỏng xong, trả về $f[query\_row][query\_glass]$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số tầng, tức là $\text{query\_row}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def champagneTower(self, poured: int, query_row: int, query_glass: int) -> float:
        f = [[0] * 101 for _ in range(101)]
        f[0][0] = poured
        for i in range(query_row + 1):
            for j in range(i + 1):
                if f[i][j] > 1:
                    half = (f[i][j] - 1) / 2
                    f[i][j] = 1
                    f[i + 1][j] += half
                    f[i + 1][j + 1] += half
        return f[query_row][query_glass]
```

#### Java

```java
class Solution {
    public double champagneTower(int poured, int query_row, int query_glass) {
        double[][] f = new double[101][101];
        f[0][0] = poured;
        for (int i = 0; i <= query_row; ++i) {
            for (int j = 0; j <= i; ++j) {
                if (f[i][j] > 1) {
                    double half = (f[i][j] - 1) / 2.0;
                    f[i][j] = 1;
                    f[i + 1][j] += half;
                    f[i + 1][j + 1] += half;
                }
            }
        }
        return f[query_row][query_glass];
    }
}
```

#### C++

```cpp
class Solution {
public:
    double champagneTower(int poured, int query_row, int query_glass) {
        double f[101][101] = {0.0};
        f[0][0] = poured;
        for (int i = 0; i <= query_row; ++i) {
            for (int j = 0; j <= i; ++j) {
                if (f[i][j] > 1) {
                    double half = (f[i][j] - 1) / 2.0;
                    f[i][j] = 1;
                    f[i + 1][j] += half;
                    f[i + 1][j + 1] += half;
                }
            }
        }
        return f[query_row][query_glass];
    }
};
```

#### Go

```go
func champagneTower(poured int, query_row int, query_glass int) float64 {
	f := [101][101]float64{}
	f[0][0] = float64(poured)
	for i := 0; i <= query_row; i++ {
		for j := 0; j <= i; j++ {
			if f[i][j] > 1 {
				half := (f[i][j] - 1) / 2.0
				f[i][j] = 1
				f[i+1][j] += half
				f[i+1][j+1] += half
			}
		}
	}
	return f[query_row][query_glass]
}
```

#### TypeScript

```ts
function champagneTower(poured: number, query_row: number, query_glass: number): number {
    const f: number[][] = Array.from({ length: 101 }, () => Array(101).fill(0));
    f[0][0] = poured;
    for (let i = 0; i <= query_row; ++i) {
        for (let j = 0; j <= i; ++j) {
            if (f[i][j] > 1) {
                const half = (f[i][j] - 1) / 2.0;
                f[i][j] = 1;
                f[i + 1][j] += half;
                f[i + 1][j + 1] += half;
            }
        }
    }
    return f[query_row][query_glass];
}
```

#### Rust

```rust
impl Solution {
    pub fn champagne_tower(poured: i32, query_row: i32, query_glass: i32) -> f64 {
        let mut f = vec![vec![0.0; 101]; 101];
        f[0][0] = poured as f64;
        for i in 0..=query_row as usize {
            for j in 0..=i {
                if f[i][j] > 1.0 {
                    let half = (f[i][j] - 1.0) / 2.0;
                    f[i][j] = 1.0;
                    f[i + 1][j] += half;
                    f[i + 1][j + 1] += half;
                }
            }
        }
        f[query_row as usize][query_glass as usize]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Mô phỏng (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Hàng $i$ chỉ phụ thuộc vào hàng $i-1$. Dùng rolling array và ghi lượng tràn sang hàng kế tiếp. Giới hạn lượng champagne trong ly cần truy vấn ở mức $1$.

<!-- thinking:end -->

Vì lượng champagne ở mỗi tầng chỉ phụ thuộc vào tầng trước đó, ta có thể dùng rolling array để tối ưu không gian, chuyển mảng 2 chiều thành mảng 1 chiều.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số tầng, tức là $\text{query\_row}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def champagneTower(self, poured: int, query_row: int, query_glass: int) -> float:
        f = [poured]
        for i in range(1, query_row + 1):
            g = [0] * (i + 1)
            for j, v in enumerate(f):
                if v > 1:
                    half = (v - 1) / 2
                    g[j] += half
                    g[j + 1] += half
            f = g
        return min(1, f[query_glass])
```

#### Java

```java
class Solution {
    public double champagneTower(int poured, int query_row, int query_glass) {
        double[] f = {poured};
        for (int i = 1; i <= query_row; ++i) {
            double[] g = new double[i + 1];
            for (int j = 0; j < i; ++j) {
                if (f[j] > 1) {
                    double half = (f[j] - 1) / 2.0;
                    g[j] += half;
                    g[j + 1] += half;
                }
            }
            f = g;
        }
        return Math.min(1, f[query_glass]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    double champagneTower(int poured, int query_row, int query_glass) {
        double f[101] = {(double) poured};
        double g[101];
        for (int i = 1; i <= query_row; ++i) {
            memset(g, 0, sizeof g);
            for (int j = 0; j < i; ++j) {
                if (f[j] > 1) {
                    double half = (f[j] - 1) / 2.0;
                    g[j] += half;
                    g[j + 1] += half;
                }
            }
            memcpy(f, g, sizeof g);
        }
        return min(1.0, f[query_glass]);
    }
};
```

#### Go

```go
func champagneTower(poured int, query_row int, query_glass int) float64 {
	f := []float64{float64(poured)}
	for i := 1; i <= query_row; i++ {
		g := make([]float64, i+1)
		for j, v := range f {
			if v > 1 {
				half := (v - 1) / 2.0
				g[j] += half
				g[j+1] += half
			}
		}
		f = g
	}
	return math.Min(1, f[query_glass])
}
```

#### TypeScript

```ts
function champagneTower(poured: number, query_row: number, query_glass: number): number {
    let f: number[] = [poured];
    for (let i = 1; i <= query_row; ++i) {
        const g: number[] = new Array(i + 1).fill(0);
        for (let j = 0; j < i; ++j) {
            if (f[j] > 1) {
                const half = (f[j] - 1) / 2.0;
                g[j] += half;
                g[j + 1] += half;
            }
        }
        f = g;
    }
    return Math.min(1, f[query_glass]);
}
```

#### Rust

```rust
impl Solution {
    pub fn champagne_tower(poured: i32, query_row: i32, query_glass: i32) -> f64 {
        let mut f = vec![poured as f64];
        for i in 1..=query_row {
            let mut g = vec![0.0; (i + 1) as usize];
            for j in 0..i as usize {
                if f[j] > 1.0 {
                    let half = (f[j] - 1.0) / 2.0;
                    g[j] += half;
                    g[j + 1] += half;
                }
            }
            f = g;
        }
        f[query_glass as usize].min(1.0)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
