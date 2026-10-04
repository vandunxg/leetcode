---
comments: true
difficulty: Hard
rating: 2424
source: Weekly Contest 350 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2742. Painting the Walls](https://leetcode.com/problems/painting-the-walls)

[中文文档](/solution/2700-2799/2742.Painting%20the%20Walls/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <strong>được đánh chỉ số từ 0</strong>, <code>cost</code> và <code>time</code>, cùng có kích thước <code>n</code>, lần lượt biểu diễn chi phí và thời gian cần để sơn <code>n</code> bức tường khác nhau. Có hai thợ sơn:</p>

<ul>
	<li>Một <strong>&nbsp;thợ sơn có trả phí</strong>&nbsp;sơn bức tường thứ <code>i<sup>th</sup></code> trong <code>time[i]</code> đơn vị thời gian và tốn <code>cost[i]</code> đơn vị tiền.</li>
	<li>Một <strong>thợ sơn miễn phí</strong> có thể sơn <strong>bất kỳ</strong> bức tường nào trong <code>1</code> đơn vị thời gian với chi phí <code>0</code>. Tuy nhiên, thợ sơn miễn phí chỉ được sử dụng khi thợ sơn có trả phí đang <strong>bận</strong>.</li>
</ul>

<p>Hãy trả về <em>số tiền nhỏ nhất cần thiết để sơn </em><code>n</code><em>&nbsp;bức tường.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> cost = [1,2,3,2], time = [1,2,3,2]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các bức tường ở chỉ số 0 và 1 sẽ được sơn bởi thợ sơn có trả phí, mất 3 đơn vị thời gian; trong lúc đó, thợ sơn miễn phí sẽ sơn các bức tường ở chỉ số 2 và 3 mà không tốn chi phí trong 2 đơn vị thời gian. Vì vậy, tổng chi phí là 1 + 2 = 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> cost = [2,3,4,2], time = [1,1,1,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các bức tường ở chỉ số 0 và 3 sẽ được sơn bởi thợ sơn có trả phí, mất 2 đơn vị thời gian; trong lúc đó, thợ sơn miễn phí sẽ sơn các bức tường ở chỉ số 1 và 2 mà không tốn chi phí trong 2 đơn vị thời gian. Vì vậy, tổng chi phí là 2 + 2 = 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= cost.length &lt;= 500</code></li>
	<li><code>cost.length == time.length</code></li>
	<li><code>1 &lt;= cost[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= time[i] &lt;= 500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Thợ sơn có trả phí tốn $cost[i]$ và mất $time[i]$ để sơn bức tường $i$, trong khoảng thời gian đó thợ sơn miễn phí có thể hoàn thành cùng số lượng bức tường. Liệt kê tập các bức tường được sơn bởi thợ sơn có trả phí sẽ có $2^n$ trường hợp, trong khi $n\le 500$.
>
> Xét bức tường $i$ từ trái sang phải: chọn thợ sơn có trả phí làm tăng ngân sách thời gian miễn phí thêm $time[i]$, còn dùng thợ sơn miễn phí làm giảm ngân sách đó đi $1$. Trạng thái $(i,j)$ là chi phí nhỏ nhất để sơn từ bức tường $i$ khi còn $j$ đơn vị thời gian miễn phí; nếu số bức tường còn lại không vượt quá $j$ thì chi phí là $0$. Ghi nhớ các trạng thái có độ phức tạp bậc hai theo $n$.

<!-- thinking:end -->

Ta có thể xét xem mỗi bức tường được sơn bởi thợ sơn có trả phí hay thợ sơn miễn phí. Xây dựng hàm $dfs(i, j)$, biểu diễn chi phí nhỏ nhất để sơn tất cả các bức tường còn lại, bắt đầu từ bức tường thứ $i$ khi thời gian làm việc miễn phí còn lại là $j$. Khi đó, đáp án là $dfs(0, 0)$.

Quá trình tính hàm $dfs(i, j)$ như sau:

- Nếu $n - i \le j$, nghĩa là số bức tường còn lại không nhiều hơn thời gian làm việc của thợ sơn miễn phí, thì các bức tường còn lại được sơn bởi thợ sơn miễn phí và chi phí là $0$;
- Nếu $i \ge n$, trả về $+\infty$;
- Ngược lại, nếu bức tường thứ $i$ được sơn bởi thợ sơn có trả phí, chi phí là $cost[i]$, khi đó $dfs(i, j) = dfs(i + 1, j + time[i]) + cost[i]$; nếu bức tường thứ $i$ được sơn bởi thợ sơn miễn phí, chi phí là $0$, khi đó $dfs(i, j) = dfs(i + 1, j - 1)$.

Lưu ý rằng tham số $j$ có thể nhỏ hơn $0$. Vì vậy, trong quá trình cài đặt thực tế, ngoại trừ ngôn ngữ $Python$, ta cộng thêm $n$ vào $j$ để miền giá trị của $j$ nằm trong $[0, 2n]$.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def paintWalls(self, cost: List[int], time: List[int]) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if n - i <= j:
                return 0
            if i >= n:
                return inf
            return min(dfs(i + 1, j + time[i]) + cost[i], dfs(i + 1, j - 1))

        n = len(cost)
        return dfs(0, 0)
```

#### Java

```java
class Solution {
    private int n;
    private int[] cost;
    private int[] time;
    private Integer[][] f;

    public int paintWalls(int[] cost, int[] time) {
        n = cost.length;
        this.cost = cost;
        this.time = time;
        f = new Integer[n][n << 1 | 1];
        return dfs(0, n);
    }

    private int dfs(int i, int j) {
        if (n - i <= j - n) {
            return 0;
        }
        if (i >= n) {
            return 1 << 30;
        }
        if (f[i][j] == null) {
            f[i][j] = Math.min(dfs(i + 1, j + time[i]) + cost[i], dfs(i + 1, j - 1));
        }
        return f[i][j];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int paintWalls(vector<int>& cost, vector<int>& time) {
        int n = cost.size();
        int f[n][n << 1 | 1];
        memset(f, -1, sizeof(f));
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            if (n - i <= j - n) {
                return 0;
            }
            if (i >= n) {
                return 1 << 30;
            }
            if (f[i][j] == -1) {
                f[i][j] = min(dfs(i + 1, j + time[i]) + cost[i], dfs(i + 1, j - 1));
            }
            return f[i][j];
        };
        return dfs(0, n);
    }
};
```

#### Go

```go
func paintWalls(cost []int, time []int) int {
	n := len(cost)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n<<1|1)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if n-i <= j-n {
			return 0
		}
		if i >= n {
			return 1 << 30
		}
		if f[i][j] == -1 {
			f[i][j] = min(dfs(i+1, j+time[i])+cost[i], dfs(i+1, j-1))
		}
		return f[i][j]
	}
	return dfs(0, n)
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn paint_walls(cost: Vec<i32>, time: Vec<i32>) -> i32 {
        let n = cost.len();
        let mut record_vec: Vec<Vec<i32>> = vec![vec![-1; n << 1 | 1]; n];
        Self::dfs(&mut record_vec, 0, n as i32, n as i32, &time, &cost)
    }

    #[allow(dead_code)]
    fn dfs(
        record_vec: &mut Vec<Vec<i32>>,
        i: i32,
        j: i32,
        n: i32,
        time: &Vec<i32>,
        cost: &Vec<i32>,
    ) -> i32 {
        if n - i <= j - n {
            // All the remaining walls can be printed at no cost
            // Just return 0
            return 0;
        }
        if i >= n {
            // No way this case can be achieved
            // Just return +INF
            return 1 << 30;
        }
        if record_vec[i as usize][j as usize] == -1 {
            // This record hasn't been written
            record_vec[i as usize][j as usize] = std::cmp::min(
                Self::dfs(record_vec, i + 1, j + time[i as usize], n, time, cost)
                    + cost[i as usize],
                Self::dfs(record_vec, i + 1, j - 1, n, time, cost),
            );
        }
        record_vec[i as usize][j as usize]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
