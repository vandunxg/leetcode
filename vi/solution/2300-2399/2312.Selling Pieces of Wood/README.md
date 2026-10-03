---
comments: true
difficulty: Hard
rating: 2363
source: Weekly Contest 298 Q4
tags:
    - Memoization
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2312. Selling Pieces of Wood](https://leetcode.com/problems/selling-pieces-of-wood)

[中文文档](/solution/2300-2399/2312.Selling%20Pieces%20of%20Wood/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>m</code> và <code>n</code>, lần lượt biểu thị chiều cao và chiều rộng của một miếng gỗ hình chữ nhật. Bạn cũng được cho một mảng số nguyên 2 chiều <code>prices</code>, trong đó <code>prices[i] = [h<sub>i</sub>, w<sub>i</sub>, price<sub>i</sub>]</code> cho biết bạn có thể bán một miếng gỗ hình chữ nhật có chiều cao <code>h<sub>i</sub></code> và chiều rộng <code>w<sub>i</sub></code> với giá <code>price<sub>i</sub></code> đô la.</p>

<p>Để cắt một miếng gỗ, bạn phải thực hiện một đường cắt dọc hoặc ngang xuyên suốt <strong>toàn bộ</strong> chiều cao hoặc chiều rộng của miếng gỗ để tách nó thành hai miếng nhỏ hơn. Sau khi cắt một miếng gỗ thành một số miếng nhỏ hơn, bạn có thể bán các miếng theo <code>prices</code>. Bạn có thể bán nhiều miếng có cùng hình dạng và không cần phải bán tất cả các hình dạng. Do thớ gỗ tạo ra sự khác biệt, <strong>không thể xoay</strong> một miếng để đổi chiều cao và chiều rộng cho nhau.</p>

<p><em>Trả về số tiền <strong>tối đa</strong> bạn có thể kiếm được sau khi cắt một miếng gỗ kích thước </em><code>m x n</code><em>.</em></p>

<p>Lưu ý rằng bạn có thể cắt miếng gỗ bao nhiêu lần tùy ý.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2312.Selling%20Pieces%20of%20Wood/images/ex1.png" style="width: 239px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> m = 3, n = 5, prices = [[1,4,2],[2,2,7],[2,1,3]]
<strong>Đầu ra:</strong> 19
<strong>Giải thích:</strong> Hình minh họa phía trên cho thấy một phương án có thể thực hiện. Phương án này gồm:
- 2 miếng gỗ có hình dạng 2 x 2, bán với giá 2 * 7 = 14.
- 1 miếng gỗ có hình dạng 2 x 1, bán với giá 1 * 3 = 3.
- 1 miếng gỗ có hình dạng 1 x 4, bán với giá 1 * 2 = 2.
Tổng số tiền kiếm được là 14 + 3 + 2 = 19.
Có thể chứng minh rằng 19 là số tiền lớn nhất có thể kiếm được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2312.Selling%20Pieces%20of%20Wood/images/ex2new.png" style="width: 250px; height: 175px;" />
<pre>
<strong>Đầu vào:</strong> m = 4, n = 6, prices = [[3,2,10],[1,4,2],[4,1,3]]
<strong>Đầu ra:</strong> 32
<strong>Giải thích:</strong> Hình minh họa phía trên cho thấy một phương án có thể thực hiện. Phương án này gồm:
- 3 miếng gỗ có hình dạng 3 x 2, bán với giá 3 * 10 = 30.
- 1 miếng gỗ có hình dạng 1 x 4, bán với giá 1 * 2 = 2.
Tổng số tiền kiếm được là 30 + 2 = 32.
Có thể chứng minh rằng 32 là số tiền lớn nhất có thể kiếm được.
Lưu ý rằng không thể xoay miếng gỗ 1 x 4 để có được miếng gỗ 4 x 1.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>1 &lt;= prices.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>prices[i].length == 3</code></li>
	<li><code>1 &lt;= h<sub>i</sub> &lt;= m</code></li>
	<li><code>1 &lt;= w<sub>i</sub> &lt;= n</code></li>
	<li><code>1 &lt;= price<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
	<li>Tất cả các hình dạng gỗ <code>(h<sub>i</sub>, w<sub>i</sub>)</code> đều <strong>đôi một khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm với Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Một tấm gỗ có thể được cắt ngang hoặc dọc nhiều lần, hoặc bán nguyên tấm. Việc duyệt các chuỗi cắt sẽ tính lặp lại cùng một kích thước. Vì $m, n \le 200$, chỉ có $O(mn)$ hình chữ nhật khác nhau.
>
> Bài toán con chỉ phụ thuộc vào chiều cao và chiều rộng: chọn giá trị lớn hơn giữa bán nguyên tấm với chia một lần rồi cộng kết quả tối ưu của hai phần. Ta lưu các mức giá đã cho, sau đó dùng memoization cho $dfs(h,w)$; chỉ cần cắt đến nửa chiều cao hoặc chiều rộng để bỏ qua các cách chia đối xứng.

<!-- thinking:end -->

Trước hết, ta định nghĩa một mảng 2 chiều $d$, trong đó $d[i][j]$ biểu thị giá của một khối gỗ có chiều cao $i$ và chiều rộng $j$.

Sau đó, ta xây dựng hàm $dfs(h, w)$ biểu thị số tiền tối đa thu được khi cắt một khối gỗ có chiều cao $h$ và chiều rộng $w$. Đáp án là $dfs(m, n)$.

Quy trình của hàm $dfs(h, w)$ như sau:

- Nếu $(h, w)$ đã được tính trước đó, trả về ngay kết quả.
- Nếu chưa, khởi tạo đáp án bằng $d[h][w]$, sau đó liệt kê các vị trí cắt, tính số tiền tối đa thu được khi cắt khối gỗ thành hai miếng và lấy giá trị lớn nhất.

Độ phức tạp thời gian là $O(m \times n \times (m + n) + p)$, và độ phức tạp không gian là $O(m \times n)$. Ở đây, $p$ là độ dài của mảng giá, còn $m$ và $n$ lần lượt là chiều cao và chiều rộng của khối gỗ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sellingWood(self, m: int, n: int, prices: List[List[int]]) -> int:
        @cache
        def dfs(h: int, w: int) -> int:
            ans = d[h].get(w, 0)
            for i in range(1, h // 2 + 1):
                ans = max(ans, dfs(i, w) + dfs(h - i, w))
            for i in range(1, w // 2 + 1):
                ans = max(ans, dfs(h, i) + dfs(h, w - i))
            return ans

        d = defaultdict(dict)
        for h, w, p in prices:
            d[h][w] = p
        return dfs(m, n)
```

#### Java

```java
class Solution {
    private int[][] d;
    private Long[][] f;

    public long sellingWood(int m, int n, int[][] prices) {
        d = new int[m + 1][n + 1];
        f = new Long[m + 1][n + 1];
        for (var p : prices) {
            d[p[0]][p[1]] = p[2];
        }
        return dfs(m, n);
    }

    private long dfs(int h, int w) {
        if (f[h][w] != null) {
            return f[h][w];
        }
        long ans = d[h][w];
        for (int i = 1; i < h / 2 + 1; ++i) {
            ans = Math.max(ans, dfs(i, w) + dfs(h - i, w));
        }
        for (int i = 1; i < w / 2 + 1; ++i) {
            ans = Math.max(ans, dfs(h, i) + dfs(h, w - i));
        }
        return f[h][w] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long sellingWood(int m, int n, vector<vector<int>>& prices) {
        using ll = long long;
        ll f[m + 1][n + 1];
        int d[m + 1][n + 1];
        memset(f, -1, sizeof(f));
        memset(d, 0, sizeof(d));
        for (auto& p : prices) {
            d[p[0]][p[1]] = p[2];
        }
        function<ll(int, int)> dfs = [&](int h, int w) -> ll {
            if (f[h][w] != -1) {
                return f[h][w];
            }
            ll ans = d[h][w];
            for (int i = 1; i < h / 2 + 1; ++i) {
                ans = max(ans, dfs(i, w) + dfs(h - i, w));
            }
            for (int i = 1; i < w / 2 + 1; ++i) {
                ans = max(ans, dfs(h, i) + dfs(h, w - i));
            }
            return f[h][w] = ans;
        };
        return dfs(m, n);
    }
};
```

#### Go

```go
func sellingWood(m int, n int, prices [][]int) int64 {
	f := make([][]int64, m+1)
	d := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int64, n+1)
		for j := range f[i] {
			f[i][j] = -1
		}
		d[i] = make([]int, n+1)
	}
	for _, p := range prices {
		d[p[0]][p[1]] = p[2]
	}
	var dfs func(int, int) int64
	dfs = func(h, w int) int64 {
		if f[h][w] != -1 {
			return f[h][w]
		}
		ans := int64(d[h][w])
		for i := 1; i < h/2+1; i++ {
			ans = max(ans, dfs(i, w)+dfs(h-i, w))
		}
		for i := 1; i < w/2+1; i++ {
			ans = max(ans, dfs(h, i)+dfs(h, w-i))
		}
		f[h][w] = ans
		return ans
	}
	return dfs(m, n)
}
```

#### TypeScript

```ts
function sellingWood(m: number, n: number, prices: number[][]): number {
    const f: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(-1));
    const d: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (const [h, w, p] of prices) {
        d[h][w] = p;
    }

    const dfs = (h: number, w: number): number => {
        if (f[h][w] !== -1) {
            return f[h][w];
        }

        let ans = d[h][w];
        for (let i = 1; i <= Math.floor(h / 2); i++) {
            ans = Math.max(ans, dfs(i, w) + dfs(h - i, w));
        }
        for (let i = 1; i <= Math.floor(w / 2); i++) {
            ans = Math.max(ans, dfs(h, i) + dfs(h, w - i));
        }
        return (f[h][w] = ans);
    };

    return dfs(m, n);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 phải chịu overhead của đệ quy và cache. Việc điền bảng theo thứ tự kích thước tăng dần thực hiện cùng các chuyển trạng thái bằng các vòng lặp lồng nhau, loại bỏ call stack và dễ roll hoặc chạy song song hơn.

<!-- thinking:end -->

Ta có thể chuyển cách tìm kiếm với memoization trong Lời giải 1 thành quy hoạch động.

Tương tự Lời giải 1, ta định nghĩa một mảng 2 chiều $d$, trong đó $d[i][j]$ biểu thị giá của một khối gỗ có chiều cao $i$ và chiều rộng $j$. Ban đầu, ta duyệt qua mảng giá $prices$ và lưu giá $p$ của mỗi khối gỗ $(h, w, p)$ vào $d[h][w]$, còn các mức giá khác được đặt bằng $0$.

Sau đó, ta định nghĩa một mảng 2 chiều khác là $f$, trong đó $f[i][j]$ biểu thị số tiền tối đa thu được khi cắt một khối gỗ có chiều cao $i$ và chiều rộng $j$. Đáp án là $f[m][n]$.

Xét cách chuyển trạng thái của $f[i][j]$, ban đầu $f[i][j] = d[i][j]$. Ta liệt kê các vị trí cắt, tính số tiền tối đa thu được khi cắt khối gỗ thành hai miếng và lấy giá trị lớn nhất.

Độ phức tạp thời gian là $O(m \times n \times (m + n) + p)$, và độ phức tạp không gian là $O(m \times n)$. Ở đây, $p$ là độ dài của mảng giá, còn $m$ và $n$ lần lượt là chiều cao và chiều rộng của khối gỗ.

Bài toán tương tự:

- [1444. Number of Ways of Cutting a Pizza](https://github.com/doocs/leetcode/blob/main/solution/1400-1499/1444.Number%20of%20Ways%20of%20Cutting%20a%20Pizza/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sellingWood(self, m: int, n: int, prices: List[List[int]]) -> int:
        d = defaultdict(dict)
        for h, w, p in prices:
            d[h][w] = p
        f = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                f[i][j] = d[i].get(j, 0)
                for k in range(1, i):
                    f[i][j] = max(f[i][j], f[k][j] + f[i - k][j])
                for k in range(1, j):
                    f[i][j] = max(f[i][j], f[i][k] + f[i][j - k])
        return f[m][n]
```

#### Java

```java
class Solution {
    public long sellingWood(int m, int n, int[][] prices) {
        int[][] d = new int[m + 1][n + 1];
        long[][] f = new long[m + 1][n + 1];
        for (int[] p : prices) {
            d[p[0]][p[1]] = p[2];
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                f[i][j] = d[i][j];
                for (int k = 1; k < i; ++k) {
                    f[i][j] = Math.max(f[i][j], f[k][j] + f[i - k][j]);
                }
                for (int k = 1; k < j; ++k) {
                    f[i][j] = Math.max(f[i][j], f[i][k] + f[i][j - k]);
                }
            }
        }
        return f[m][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long sellingWood(int m, int n, vector<vector<int>>& prices) {
        long long f[m + 1][n + 1];
        int d[m + 1][n + 1];
        memset(f, -1, sizeof(f));
        memset(d, 0, sizeof(d));
        for (auto& p : prices) {
            d[p[0]][p[1]] = p[2];
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                f[i][j] = d[i][j];
                for (int k = 1; k < i; ++k) {
                    f[i][j] = max(f[i][j], f[k][j] + f[i - k][j]);
                }
                for (int k = 1; k < j; ++k) {
                    f[i][j] = max(f[i][j], f[i][k] + f[i][j - k]);
                }
            }
        }
        return f[m][n];
    }
};
```

#### Go

```go
func sellingWood(m int, n int, prices [][]int) int64 {
	d := make([][]int, m+1)
	f := make([][]int64, m+1)
	for i := range d {
		d[i] = make([]int, n+1)
		f[i] = make([]int64, n+1)
	}
	for _, p := range prices {
		d[p[0]][p[1]] = p[2]
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			f[i][j] = int64(d[i][j])
			for k := 1; k < i; k++ {
				f[i][j] = max(f[i][j], f[k][j]+f[i-k][j])
			}
			for k := 1; k < j; k++ {
				f[i][j] = max(f[i][j], f[i][k]+f[i][j-k])
			}
		}
	}
	return f[m][n]
}
```

#### TypeScript

```ts
function sellingWood(m: number, n: number, prices: number[][]): number {
    const f: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    const d: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (const [h, w, p] of prices) {
        d[h][w] = p;
    }

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            f[i][j] = d[i][j];
            for (let k = 1; k < i; k++) {
                f[i][j] = Math.max(f[i][j], f[k][j] + f[i - k][j]);
            }
            for (let k = 1; k < j; k++) {
                f[i][j] = Math.max(f[i][j], f[i][k] + f[i][j - k]);
            }
        }
    }

    return f[m][n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
