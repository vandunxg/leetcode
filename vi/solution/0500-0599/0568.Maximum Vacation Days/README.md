---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [568. Maximum Vacation Days 🔒](https://leetcode.com/problems/maximum-vacation-days)

[中文文档](/solution/0500-0599/0568.Maximum%20Vacation%20Days/README.md)

## Mô tả

<!-- description:start -->

<p>LeetCode muốn cho một trong những nhân viên xuất sắc nhất cơ hội đi qua <code>n</code> thành phố để thu thập các bài toán thuật toán. Tuy nhiên, làm việc mãi mà không nghỉ sẽ khiến ai cũng trở nên nhàm chán; bạn có thể nghỉ ở một số thành phố trong một số tuần. Nhiệm vụ của bạn là lên lịch di chuyển để tối đa hóa số ngày nghỉ, đồng thời tuân theo một số quy tắc và giới hạn.</p>

<p>Quy tắc và giới hạn:</p>

<ol>
	<li>Bạn chỉ có thể di chuyển giữa <code>n</code> thành phố, được đánh chỉ số từ <code>0</code> đến <code>n - 1</code>. Ban đầu, bạn ở thành phố có chỉ số <code>0</code> vào <strong>thứ Hai</strong>.</li>
	<li>Các thành phố được kết nối bằng các chuyến bay. Lịch bay được biểu diễn bằng ma trận <code>n x n</code> (không nhất thiết đối xứng) tên là <code>flights</code>, mô tả các chuyến bay từ thành phố <code>i</code> đến thành phố <code>j</code>. Nếu không có chuyến bay từ <code>i</code> đến <code>j</code>, thì <code>flights[i][j] == 0</code>; ngược lại, <code>flights[i][j] == 1</code>. Ngoài ra, <code>flights[i][i] == 0</code> với mọi <code>i</code>.</li>
	<li>Bạn có tổng cộng <code>k</code> tuần (mỗi tuần có <strong>bảy ngày</strong>) để đi lại. Mỗi ngày bạn chỉ có thể bay nhiều nhất một lần và chỉ được bay vào sáng thứ Hai của mỗi tuần. Vì thời gian bay rất ngắn, ta bỏ qua ảnh hưởng của thời gian bay.</li>
	<li>Số ngày nghỉ được phép ở mỗi thành phố thay đổi theo từng tuần và được biểu diễn bằng ma trận <code>n x k</code> tên là <code>days</code>. Giá trị <code>days[i][j]</code> là số ngày nghỉ tối đa bạn có thể dành ở thành phố <code>i</code> trong tuần <code>j</code>.</li>
	<li>Bạn có thể ở lại một thành phố lâu hơn số ngày được nghỉ, nhưng những ngày vượt quá sẽ phải đi làm và không được tính là ngày nghỉ.</li>
	<li>Nếu bạn bay từ thành phố <code>A</code> đến thành phố <code>B</code> và nghỉ trong ngày đó, ngày nghỉ này được tính vào số ngày nghỉ của thành phố <code>B</code> trong tuần đó.</li>
	<li>Ta bỏ qua ảnh hưởng của thời lượng chuyến bay khi tính số ngày nghỉ.</li>
</ol>

<p>Cho hai ma trận <code>flights</code> và <code>days</code>, hãy trả về <em>số ngày nghỉ tối đa bạn có thể có trong </em><code>k</code><em> tuần</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> flights = [[0,1,1],[1,0,1],[1,1,0]], days = [[1,3,1],[6,0,3],[3,3,3]]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong>
Một trong những lịch trình tối ưu là:
Tuần 1: bay từ thành phố 0 đến thành phố 1 vào thứ Hai, nghỉ 6 ngày và làm việc 1 ngày.
(Dù ban đầu bạn ở thành phố 0, bạn cũng có thể bay đến và bắt đầu ở thành phố khác vì hôm đó là thứ Hai.)
Tuần 2: bay từ thành phố 1 đến thành phố 2 vào thứ Hai, nghỉ 3 ngày và làm việc 4 ngày.
Tuần 3: ở lại thành phố 2, nghỉ 3 ngày và làm việc 4 ngày.
Đáp án = 6 + 3 + 3 = 12.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> flights = [[0,0,0],[0,0,0],[0,0,0]], days = [[1,1,1],[7,7,7],[7,7,7]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Vì không có chuyến bay nào giúp bạn đến thành phố khác, bạn phải ở thành phố 0 suốt 3 tuần.
Mỗi tuần, bạn chỉ có một ngày nghỉ và phải làm việc sáu ngày.
Vì vậy, số ngày nghỉ tối đa là 3.
Đáp án = 1 + 1 + 1 = 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> flights = [[0,1,1],[1,0,1],[1,1,0]], days = [[7,0,0],[0,7,0],[0,0,7]]
<strong>Đầu ra:</strong> 21
<strong>Giải thích:</strong>
Một trong những lịch trình tối ưu là:
Tuần 1: ở lại thành phố 0 và nghỉ 7 ngày.
Tuần 2: bay từ thành phố 0 đến thành phố 1 vào thứ Hai và nghỉ 7 ngày.
Tuần 3: bay từ thành phố 1 đến thành phố 2 vào thứ Hai và nghỉ 7 ngày.
Đáp án = 7 + 7 + 7 = 21
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == flights.length</code></li>
	<li><code>n == flights[i].length</code></li>
	<li><code>n == days.length</code></li>
	<li><code>k == days[i].length</code></li>
	<li><code>1 &lt;= n, k &lt;= 100</code></li>
	<li><code>flights[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>0 &lt;= days[i][j] &lt;= 7</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi tuần được dành ở một thành phố; bạn chỉ có thể di chuyển theo các chuyến bay hoặc ở lại. Có quá nhiều lịch trình nếu xét hết $n^K$ khả năng.
>
> $f[k][j]$ là tổng số ngày nghỉ tối đa sau $k$ tuần nếu kết thúc ở thành phố $j$. Ta có thể ở lại $j$ hoặc bay từ thành phố $i$ đến, rồi cộng $days[j][k-1]$. Tuần $0$ chỉ bắt đầu ở thành phố $0$. Đáp án là giá trị lớn nhất trong các thành phố ở tuần $K$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxVacationDays(self, flights: List[List[int]], days: List[List[int]]) -> int:
        n = len(flights)
        K = len(days[0])
        f = [[-inf] * n for _ in range(K + 1)]
        f[0][0] = 0
        for k in range(1, K + 1):
            for j in range(n):
                f[k][j] = f[k - 1][j]
                for i in range(n):
                    if flights[i][j]:
                        f[k][j] = max(f[k][j], f[k - 1][i])
                f[k][j] += days[j][k - 1]
        return max(f[-1][j] for j in range(n))
```

#### Java

```java
class Solution {
    public int maxVacationDays(int[][] flights, int[][] days) {
        int n = flights.length;
        int K = days[0].length;
        final int inf = 1 << 30;
        int[][] f = new int[K + 1][n];
        for (var g : f) {
            Arrays.fill(g, -inf);
        }
        f[0][0] = 0;
        for (int k = 1; k <= K; ++k) {
            for (int j = 0; j < n; ++j) {
                f[k][j] = f[k - 1][j];
                for (int i = 0; i < n; ++i) {
                    if (flights[i][j] == 1) {
                        f[k][j] = Math.max(f[k][j], f[k - 1][i]);
                    }
                }
                f[k][j] += days[j][k - 1];
            }
        }
        int ans = 0;
        for (int j = 0; j < n; ++j) {
            ans = Math.max(ans, f[K][j]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxVacationDays(vector<vector<int>>& flights, vector<vector<int>>& days) {
        int n = flights.size();
        int K = days[0].size();
        int f[K + 1][n];
        memset(f, -0x3f, sizeof(f));
        f[0][0] = 0;
        for (int k = 1; k <= K; ++k) {
            for (int j = 0; j < n; ++j) {
                f[k][j] = f[k - 1][j];
                for (int i = 0; i < n; ++i) {
                    if (flights[i][j] == 1) {
                        f[k][j] = max(f[k][j], f[k - 1][i]);
                    }
                }
                f[k][j] += days[j][k - 1];
            }
        }
        int ans = 0;
        for (int j = 0; j < n; ++j) {
            ans = max(ans, f[K][j]);
        }
        return ans;
    }
};
```

#### Go

```go
func maxVacationDays(flights [][]int, days [][]int) (ans int) {
	n, K := len(flights), len(days[0])
	f := make([][]int, K+1)
	for i := range f {
		f[i] = make([]int, n)
		for j := range f[i] {
			f[i][j] = -(1 << 30)
		}
	}
	f[0][0] = 0
	for k := 1; k <= K; k++ {
		for j := 0; j < n; j++ {
			f[k][j] = f[k-1][j]
			for i := 0; i < n; i++ {
				if flights[i][j] == 1 {
					f[k][j] = max(f[k][j], f[k-1][i])
				}
			}
			f[k][j] += days[j][k-1]
		}
	}
	for j := 0; j < n; j++ {
		ans = max(ans, f[K][j])
	}
	return
}
```

#### TypeScript

```ts
function maxVacationDays(flights: number[][], days: number[][]): number {
    const n = flights.length;
    const K = days[0].length;
    const inf = Number.NEGATIVE_INFINITY;
    const f: number[][] = Array.from({ length: K + 1 }, () => Array(n).fill(inf));
    f[0][0] = 0;
    for (let k = 1; k <= K; k++) {
        for (let j = 0; j < n; j++) {
            f[k][j] = f[k - 1][j];
            for (let i = 0; i < n; i++) {
                if (flights[i][j]) {
                    f[k][j] = Math.max(f[k][j], f[k - 1][i]);
                }
            }
            f[k][j] += days[j][k - 1];
        }
    }
    return Math.max(...f[K]);
}
```

#### Rust

```rust
impl Solution {
    pub fn max_vacation_days(flights: Vec<Vec<i32>>, days: Vec<Vec<i32>>) -> i32 {
        let n = flights.len();
        let k = days[0].len();
        let inf = i32::MIN;

        let mut f = vec![vec![inf; n]; k + 1];
        f[0][0] = 0;

        for step in 1..=k {
            for j in 0..n {
                f[step][j] = f[step - 1][j];
                for i in 0..n {
                    if flights[i][j] == 1 {
                        f[step][j] = f[step][j].max(f[step - 1][i]);
                    }
                }
                f[step][j] += days[j][step - 1];
            }
        }

        *f[k].iter().max().unwrap()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
