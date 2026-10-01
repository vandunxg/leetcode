---
comments: true
difficulty: Hard
tags:
    - Array
    - Math
    - Matrix
    - Sorting
---

<!-- problem:start -->

# [296. Best Meeting Point 🔒](https://leetcode.com/problems/best-meeting-point)

[中文文档](/solution/0200-0299/0296.Best%20Meeting%20Point/README.md)

## Mô tả

<!-- description:start -->

<p>Cho lưới nhị phân <code>m x n</code> <code>grid</code>, trong đó mỗi ô có giá trị <code>1</code> đánh dấu nhà của một người bạn. Hãy trả về <em><strong>tổng quãng đường di chuyển</strong> nhỏ nhất</em>.</p>

<p><strong>Tổng quãng đường di chuyển</strong> là tổng khoảng cách từ nhà của mỗi người bạn đến điểm gặp mặt.</p>

<p>Khoảng cách được tính theo <a href="http://en.wikipedia.org/wiki/Taxicab_geometry" target="_blank">khoảng cách Manhattan</a>, với <code>distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0296.Best%20Meeting%20Point/images/meetingpoint-grid.jpg" style="width: 413px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,0,0,0,1],[0,0,0,0,0],[0,0,1,0,0]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có ba người bạn sống tại (0,0), (0,4) và (2,2).
Điểm (0,2) là vị trí gặp mặt phù hợp, vì tổng quãng đường 2 + 2 + 2 = 6 là nhỏ nhất.
Vậy trả về 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1]]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>grid[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
	<li>Trong <code>grid</code> có <strong>ít nhất hai</strong> người bạn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng cách Manhattan tách thành tổng độ lệch theo trục $x$ và tổng độ lệch theo trục $y$. Trong một chiều, tổng khoảng cách nhỏ nhất đạt được tại trung vị.
>
> Thu thập chỉ số hàng của mọi ô có giá trị $1$ (đã được sắp xếp), rồi thu thập chỉ số cột và sắp xếp chúng. Chọn trung vị của hai dãy làm điểm gặp mặt, sau đó tính tổng độ lệch tuyệt đối.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minTotalDistance(self, grid: List[List[int]]) -> int:
        def f(arr, x):
            return sum(abs(v - x) for v in arr)

        rows, cols = [], []
        for i, row in enumerate(grid):
            for j, v in enumerate(row):
                if v:
                    rows.append(i)
                    cols.append(j)
        cols.sort()
        i = rows[len(rows) >> 1]
        j = cols[len(cols) >> 1]
        return f(rows, i) + f(cols, j)
```

#### Java

```java
class Solution {
    public int minTotalDistance(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        List<Integer> rows = new ArrayList<>();
        List<Integer> cols = new ArrayList<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    rows.add(i);
                    cols.add(j);
                }
            }
        }
        Collections.sort(cols);
        int i = rows.get(rows.size() >> 1);
        int j = cols.get(cols.size() >> 1);
        return f(rows, i) + f(cols, j);
    }

    private int f(List<Integer> arr, int x) {
        int s = 0;
        for (int v : arr) {
            s += Math.abs(v - x);
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minTotalDistance(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<int> rows;
        vector<int> cols;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j]) {
                    rows.emplace_back(i);
                    cols.emplace_back(j);
                }
            }
        }
        sort(cols.begin(), cols.end());
        int i = rows[rows.size() / 2];
        int j = cols[cols.size() / 2];
        auto f = [](vector<int>& arr, int x) {
            int s = 0;
            for (int v : arr) {
                s += abs(v - x);
            }
            return s;
        };
        return f(rows, i) + f(cols, j);
    }
};
```

#### Go

```go
func minTotalDistance(grid [][]int) int {
	rows, cols := []int{}, []int{}
	for i, row := range grid {
		for j, v := range row {
			if v == 1 {
				rows = append(rows, i)
				cols = append(cols, j)
			}
		}
	}
	sort.Ints(cols)
	i := rows[len(rows)>>1]
	j := cols[len(cols)>>1]
	f := func(arr []int, x int) int {
		s := 0
		for _, v := range arr {
			s += abs(v - x)
		}
		return s
	}
	return f(rows, i) + f(cols, j)
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn min_total_distance(grid: Vec<Vec<i32>>) -> i32 {
        let n = grid.len();
        let m = grid[0].len();

        let mut row_vec = Vec::new();
        let mut col_vec = Vec::new();

        // Initialize the two vector
        for i in 0..n {
            for j in 0..m {
                if grid[i][j] == 1 {
                    row_vec.push(i as i32);
                    col_vec.push(j as i32);
                }
            }
        }

        // Since the row vector is originally sorted, we only need to sort the col vector here
        col_vec.sort();

        Self::compute_manhattan_dis(&row_vec, row_vec[row_vec.len() / 2])
            + Self::compute_manhattan_dis(&col_vec, col_vec[col_vec.len() / 2])
    }

    #[allow(dead_code)]
    fn compute_manhattan_dis(v: &Vec<i32>, e: i32) -> i32 {
        let mut ret = 0;

        for num in v {
            ret += (num - e).abs();
        }

        ret
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
