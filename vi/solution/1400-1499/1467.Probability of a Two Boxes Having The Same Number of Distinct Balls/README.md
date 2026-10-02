---
comments: true
difficulty: Hard
rating: 2356
source: Weekly Contest 191 Q4
tags:
    - Array
    - Math
    - Dynamic Programming
    - Backtracking
    - Combinatorics
    - Probability and Statistics
---

<!-- problem:start -->

# [1467. Probability of a Two Boxes Having The Same Number of Distinct Balls](https://leetcode.com/problems/probability-of-a-two-boxes-having-the-same-number-of-distinct-balls)

[中文文档](/solution/1400-1499/1467.Probability%20of%20a%20Two%20Boxes%20Having%20The%20Same%20Number%20of%20Distinct%20Balls/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>2n</code> quả bóng với <code>k</code> màu khác nhau. Bạn được cung cấp một mảng số nguyên <code>balls</code> có kích thước <code>k</code>, trong đó <code>balls[i]</code> là số quả bóng có màu <code>i</code>.</p>

<p>Tất cả các quả bóng sẽ được <strong>xáo trộn đồng đều ngẫu nhiên</strong>, sau đó chúng ta phân phối <code>n</code> quả bóng đầu tiên vào hộp thứ nhất và <code>n</code> quả bóng còn lại vào hộp kia (Hãy đọc kỹ phần giải thích của ví dụ thứ hai).</p>

<p>Lưu ý rằng hai hộp được xem là khác nhau. Ví dụ, nếu có hai quả bóng màu <code>a</code> và <code>b</code>, cùng hai hộp <code>[]</code> và <code>()</code>, thì cách phân phối <code>[a] (b)</code> được xem là khác với cách phân phối <code>[b] (a) </code>(Hãy đọc kỹ phần giải thích của ví dụ thứ nhất).</p>

<p>Trả về <em>xác suất</em> để hai hộp có cùng số lượng màu bóng khác nhau. Các đáp án có sai số không quá <code>10<sup>-5</sup></code> so với giá trị thực sẽ được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> balls = [1,1]
<strong>Đầu ra:</strong> 1.00000
<strong>Giải thích:</strong> Chỉ có 2 cách chia đều các quả bóng:
- Một quả bóng màu 1 vào hộp 1 và một quả bóng màu 2 vào hộp 2
- Một quả bóng màu 2 vào hộp 1 và một quả bóng màu 1 vào hộp 2
Trong cả hai cách, số màu khác nhau trong mỗi hộp đều bằng nhau. Xác suất là 2/2 = 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> balls = [2,1,1]
<strong>Đầu ra:</strong> 0.66667
<strong>Giải thích:</strong> Ta có tập các quả bóng [1, 1, 2, 3]
Tập các quả bóng này sẽ được xáo trộn ngẫu nhiên và ta có thể nhận được một trong 12 cách xáo trộn khác nhau với xác suất bằng nhau (tức là 1/12):
[1,1 / 2,3], [1,1 / 3,2], [1,2 / 1,3], [1,2 / 3,1], [1,3 / 1,2], [1,3 / 2,1], [2,1 / 1,3], [2,1 / 3,1], [2,3 / 1,1], [3,1 / 1,2], [3,1 / 2,1], [3,2 / 1,1]
Sau đó, ta đưa hai quả bóng đầu tiên vào hộp thứ nhất và hai quả bóng tiếp theo vào hộp thứ hai.
Có thể thấy rằng 8 trong 12 cách phân phối ngẫu nhiên có cùng số màu khác nhau của các quả bóng trong mỗi hộp.
Xác suất là 8/12 = 0.66667
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> balls = [1,2,1,2]
<strong>Đầu ra:</strong> 0.60000
<strong>Giải thích:</strong> Tập các quả bóng là [1, 2, 2, 3, 4, 4]. Khó có thể liệt kê cả 180 cách xáo trộn ngẫu nhiên của tập này, nhưng dễ dàng kiểm tra rằng 108 cách trong số đó có cùng số màu khác nhau trong mỗi hộp.
Xác suất = 108 / 180 = 0.6
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= balls.length &lt;= 8</code></li>
	<li><code>1 &lt;= balls[i] &lt;= 6</code></li>
	<li><code>sum(balls)</code> là số chẵn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có tất cả $2n$ quả bóng được chia đều, nên mẫu số là $C_{2n}^n$. Có nhiều nhất $8$ màu và mỗi màu có $6$ quả bóng, vì vậy ta liệt kê số quả bóng màu $i$ được đưa vào hộp thứ nhất.
>
> $dfs(i,j,\textit{diff})$: đang xét màu $i$, còn $j$ vị trí trống trong hộp 1, và hiệu giữa số lượng màu khác nhau. Nếu đưa toàn bộ một màu vào một phía thì $diff$ thay đổi; nếu không thì giữ nguyên. Nhân với $\mathrm{comb}(balls[i],x)$. Cuối cùng chia số trạng thái được chấp nhận cho tổng số trạng thái.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getProbability(self, balls: List[int]) -> float:
        @cache
        def dfs(i: int, j: int, diff: int) -> float:
            if i >= k:
                return 1 if j == 0 and diff == 0 else 0
            if j < 0:
                return 0
            ans = 0
            for x in range(balls[i] + 1):
                y = 1 if x == balls[i] else (-1 if x == 0 else 0)
                ans += dfs(i + 1, j - x, diff + y) * comb(balls[i], x)
            return ans

        n = sum(balls) >> 1
        k = len(balls)
        return dfs(0, n, 0) / comb(n << 1, n)
```

#### Java

```java
class Solution {
    private int n;
    private long[][] c;
    private int[] balls;
    private Map<List<Integer>, Long> f = new HashMap<>();

    public double getProbability(int[] balls) {
        int mx = 0;
        for (int x : balls) {
            n += x;
            mx = Math.max(mx, x);
        }
        n >>= 1;
        this.balls = balls;
        int m = Math.max(mx, n << 1);
        c = new long[m + 1][m + 1];
        for (int i = 0; i <= m; ++i) {
            c[i][0] = 1;
            for (int j = 1; j <= i; ++j) {
                c[i][j] = c[i - 1][j - 1] + c[i - 1][j];
            }
        }
        return dfs(0, n, 0) * 1.0 / c[n << 1][n];
    }

    private long dfs(int i, int j, int diff) {
        if (i >= balls.length) {
            return j == 0 && diff == 0 ? 1 : 0;
        }
        if (j < 0) {
            return 0;
        }
        List<Integer> key = List.of(i, j, diff);
        if (f.containsKey(key)) {
            return f.get(key);
        }
        long ans = 0;
        for (int x = 0; x <= balls[i]; ++x) {
            int y = x == balls[i] ? 1 : (x == 0 ? -1 : 0);
            ans += dfs(i + 1, j - x, diff + y) * c[balls[i]][x];
        }
        f.put(key, ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double getProbability(vector<int>& balls) {
        int n = accumulate(balls.begin(), balls.end(), 0) / 2;
        int mx = *max_element(balls.begin(), balls.end());
        int m = max(mx, n << 1);
        long long c[m + 1][m + 1];
        memset(c, 0, sizeof(c));
        for (int i = 0; i <= m; ++i) {
            c[i][0] = 1;
            for (int j = 1; j <= i; ++j) {
                c[i][j] = c[i - 1][j - 1] + c[i - 1][j];
            }
        }
        int k = balls.size();
        long long f[k][n + 1][k << 1 | 1];
        memset(f, -1, sizeof(f));
        function<long long(int, int, int)> dfs = [&](int i, int j, int diff) -> long long {
            if (i >= k) {
                return j == 0 && diff == k ? 1 : 0;
            }
            if (j < 0) {
                return 0;
            }
            if (f[i][j][diff] != -1) {
                return f[i][j][diff];
            }
            long long ans = 0;
            for (int x = 0; x <= balls[i]; ++x) {
                int y = x == balls[i] ? 1 : (x == 0 ? -1 : 0);
                ans += dfs(i + 1, j - x, diff + y) * c[balls[i]][x];
            }
            return f[i][j][diff] = ans;
        };
        return dfs(0, n, k) * 1.0 / c[n << 1][n];
    }
};
```

#### Go

```go
func getProbability(balls []int) float64 {
	n, mx := 0, 0
	for _, x := range balls {
		n += x
		mx = max(mx, x)
	}
	n >>= 1
	m := max(mx, n<<1)
	c := make([][]int, m+1)
	for i := range c {
		c[i] = make([]int, m+1)
	}
	for i := 0; i <= m; i++ {
		c[i][0] = 1
		for j := 1; j <= i; j++ {
			c[i][j] = c[i-1][j-1] + c[i-1][j]
		}
	}
	k := len(balls)
	f := make([][][]int, k)
	for i := range f {
		f[i] = make([][]int, n+1)
		for j := range f[i] {
			f[i][j] = make([]int, k<<1|1)
			for h := range f[i][j] {
				f[i][j][h] = -1
			}
		}
	}
	var dfs func(int, int, int) int
	dfs = func(i, j, diff int) int {
		if i >= k {
			if j == 0 && diff == k {
				return 1
			}
			return 0
		}
		if j < 0 {
			return 0
		}
		if f[i][j][diff] != -1 {
			return f[i][j][diff]
		}
		ans := 0
		for x := 0; x <= balls[i]; x++ {
			y := 1
			if x != balls[i] {
				if x == 0 {
					y = -1
				} else {
					y = 0
				}
			}
			ans += dfs(i+1, j-x, diff+y) * c[balls[i]][x]
		}
		f[i][j][diff] = ans
		return ans
	}
	return float64(dfs(0, n, k)) / float64(c[n<<1][n])
}
```

#### TypeScript

```ts
function getProbability(balls: number[]): number {
    const n = balls.reduce((a, b) => a + b, 0) >> 1;
    const mx = Math.max(...balls);
    const m = Math.max(mx, n << 1);
    const c: number[][] = Array(m + 1)
        .fill(0)
        .map(() => Array(m + 1).fill(0));
    for (let i = 0; i <= m; ++i) {
        c[i][0] = 1;
        for (let j = 1; j <= i; ++j) {
            c[i][j] = c[i - 1][j - 1] + c[i - 1][j];
        }
    }
    const k = balls.length;
    const f: number[][][] = Array(k)
        .fill(0)
        .map(() =>
            Array(n + 1)
                .fill(0)
                .map(() => Array((k << 1) | 1).fill(-1)),
        );
    const dfs = (i: number, j: number, diff: number): number => {
        if (i >= k) {
            return j === 0 && diff === k ? 1 : 0;
        }
        if (j < 0) {
            return 0;
        }
        if (f[i][j][diff] !== -1) {
            return f[i][j][diff];
        }
        let ans = 0;
        for (let x = 0; x <= balls[i]; ++x) {
            const y = x === balls[i] ? 1 : x === 0 ? -1 : 0;
            ans += dfs(i + 1, j - x, diff + y) * c[balls[i]][x];
        }
        return (f[i][j][diff] = ans);
    };
    return dfs(0, n, k) / c[n << 1][n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
