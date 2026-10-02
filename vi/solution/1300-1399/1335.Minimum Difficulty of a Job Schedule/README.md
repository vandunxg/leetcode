---
comments: true
difficulty: Hard
rating: 2034
source: Weekly Contest 173 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1335. Minimum Difficulty of a Job Schedule](https://leetcode.com/problems/minimum-difficulty-of-a-job-schedule)

[中文文档](/solution/1300-1399/1335.Minimum%20Difficulty%20of%20a%20Job%20Schedule/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn muốn xếp lịch cho một danh sách công việc trong <code>d</code> ngày. Các công việc phụ thuộc theo thứ tự: để thực hiện công việc thứ <code>i<sup>th</sup></code>, bạn phải hoàn thành tất cả công việc <code>j</code> thỏa mãn <code>0 &lt;= j &lt; i</code>.</p>

<p>Bạn phải hoàn thành <strong>ít nhất</strong> một công việc mỗi ngày. Độ khó của lịch làm việc bằng tổng độ khó của từng ngày trong <code>d</code> ngày. Độ khó của một ngày là độ khó lớn nhất trong số các công việc được làm vào ngày đó.</p>

<p>Bạn được cho mảng số nguyên <code>jobDifficulty</code> và số nguyên <code>d</code>. Độ khó của công việc thứ <code>i<sup>th</sup></code> là <code>jobDifficulty[i]</code>.</p>

<p>Trả về <em>độ khó nhỏ nhất của một lịch làm việc</em>. Nếu không thể lập lịch cho các công việc, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1335.Minimum%20Difficulty%20of%20a%20Job%20Schedule/images/untitled.png" style="width: 365px; height: 370px;" />
<pre>
<strong>Đầu vào:</strong> jobDifficulty = [6,5,4,3,2,1], d = 2
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Ngày đầu tiên, bạn có thể hoàn thành 5 công việc đầu tiên, tổng độ khó là 6.
Ngày thứ hai, bạn hoàn thành công việc cuối cùng, độ khó là 1.
Độ khó của lịch làm việc = 6 + 1 = 7 
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> jobDifficulty = [9,9,9], d = 4
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Dù mỗi ngày hoàn thành một công việc, bạn vẫn còn một ngày trống. Vì vậy, không thể lập lịch cho các công việc đã cho.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> jobDifficulty = [1,1,1], d = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Lịch làm việc hoàn thành một công việc mỗi ngày, nên tổng độ khó là 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= jobDifficulty.length &lt;= 300</code></li>
	<li><code>0 &lt;= jobDifficulty[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= d &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Chia dãy công việc thành $d$ ngày liên tiếp; độ khó của mỗi ngày là độ khó lớn nhất trong các công việc của ngày đó, và tổng độ khó cần được tối thiểu hóa. Thứ tự công việc phải được giữ nguyên. Với $n \le 300$, $d \le 10$, việc liệt kê mọi cách chia vẫn không khả thi. Cách tốt nhất để hoàn thành $i$ công việc trong $j$ ngày chỉ phụ thuộc vào ngày cuối cùng, ngày này thực hiện các công việc trong đoạn $[k..i]$.
>
> $f[i][j]$ lưu giá trị tối ưu đó. Khi duyệt ngược $k$, ta cập nhật giá trị lớn nhất của đoạn và chuyển trạng thái từ $f[k-1][j-1]$. Nếu số công việc ít hơn số ngày thì không thể lập lịch.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là độ khó nhỏ nhất để hoàn thành $i$ công việc đầu tiên trong $j$ ngày. Ban đầu, $f[0][0] = 0$ và các trạng thái $f[i][j]$ khác bằng $\infty$.

Trong ngày thứ $j$, ta có thể chọn hoàn thành các công việc từ $k$ đến $i$. Khi đó, công thức chuyển trạng thái là:

$$
f[i][j] = \min_{k \in [1,i]} \{f[k-1][j-1] + \max_{k \leq t \leq i} \{jobDifficulty[t]\}\}
$$

Đáp án cuối cùng là $f[n][d]$.

Độ phức tạp thời gian là $O(n^2 \times d)$ và độ phức tạp không gian là $O(n \times d)$. Ở đây, $n$ và $d$ lần lượt là số công việc và số ngày.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDifficulty(self, jobDifficulty: List[int], d: int) -> int:
        n = len(jobDifficulty)
        f = [[inf] * (d + 1) for _ in range(n + 1)]
        f[0][0] = 0
        for i in range(1, n + 1):
            for j in range(1, min(d + 1, i + 1)):
                mx = 0
                for k in range(i, 0, -1):
                    mx = max(mx, jobDifficulty[k - 1])
                    f[i][j] = min(f[i][j], f[k - 1][j - 1] + mx)
        return -1 if f[n][d] >= inf else f[n][d]
```

#### Java

```java
class Solution {
    public int minDifficulty(int[] jobDifficulty, int d) {
        final int inf = 1 << 30;
        int n = jobDifficulty.length;
        int[][] f = new int[n + 1][d + 1];
        for (var g : f) {
            Arrays.fill(g, inf);
        }
        f[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= Math.min(d, i); ++j) {
                int mx = 0;
                for (int k = i; k > 0; --k) {
                    mx = Math.max(mx, jobDifficulty[k - 1]);
                    f[i][j] = Math.min(f[i][j], f[k - 1][j - 1] + mx);
                }
            }
        }
        return f[n][d] >= inf ? -1 : f[n][d];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minDifficulty(vector<int>& jobDifficulty, int d) {
        int n = jobDifficulty.size();
        int f[n + 1][d + 1];
        memset(f, 0x3f, sizeof(f));
        f[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= min(d, i); ++j) {
                int mx = 0;
                for (int k = i; k; --k) {
                    mx = max(mx, jobDifficulty[k - 1]);
                    f[i][j] = min(f[i][j], f[k - 1][j - 1] + mx);
                }
            }
        }
        return f[n][d] == 0x3f3f3f3f ? -1 : f[n][d];
    }
};
```

#### Go

```go
func minDifficulty(jobDifficulty []int, d int) int {
	n := len(jobDifficulty)
	f := make([][]int, n+1)
	const inf = 1 << 30
	for i := range f {
		f[i] = make([]int, d+1)
		for j := range f[i] {
			f[i][j] = inf
		}
	}
	f[0][0] = 0
	for i := 1; i <= n; i++ {
		for j := 1; j <= min(d, i); j++ {
			mx := 0
			for k := i; k > 0; k-- {
				mx = max(mx, jobDifficulty[k-1])
				f[i][j] = min(f[i][j], f[k-1][j-1]+mx)
			}
		}
	}
	if f[n][d] == inf {
		return -1
	}
	return f[n][d]
}
```

#### TypeScript

```ts
function minDifficulty(jobDifficulty: number[], d: number): number {
    const n = jobDifficulty.length;
    const inf = 1 << 30;
    const f: number[][] = new Array(n + 1).fill(0).map(() => new Array(d + 1).fill(inf));
    f[0][0] = 0;
    for (let i = 1; i <= n; ++i) {
        for (let j = 1; j <= Math.min(d, i); ++j) {
            let mx = 0;
            for (let k = i; k > 0; --k) {
                mx = Math.max(mx, jobDifficulty[k - 1]);
                f[i][j] = Math.min(f[i][j], f[k - 1][j - 1] + mx);
            }
        }
    }
    return f[n][d] < inf ? f[n][d] : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
