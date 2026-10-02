---
comments: true
difficulty: Easy
rating: 1283
source: Weekly Contest 162 Q1
tags:
    - Array
    - Math
    - Simulation
---

<!-- problem:start -->

# [1252. Cells with Odd Values in a Matrix](https://leetcode.com/problems/cells-with-odd-values-in-a-matrix)

[中文文档](/solution/1200-1299/1252.Cells%20with%20Odd%20Values%20in%20a%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>m x n</code> ban đầu gồm toàn số <code>0</code>. Ngoài ra còn có mảng 2 chiều <code>indices</code>, trong đó mỗi <code>indices[i] = [r<sub>i</sub>, c<sub>i</sub>]</code> biểu thị một vị trí <strong>đánh số từ 0</strong> để thực hiện thao tác tăng trên ma trận.</p>

<p>Với mỗi vị trí <code>indices[i]</code>, thực hiện <strong>cả hai</strong> thao tác sau:</p>

<ol>
	<li>Tăng giá trị của <strong>mọi</strong> ô trên hàng <code>r<sub>i</sub></code>.</li>
	<li>Tăng giá trị của <strong>mọi</strong> ô trên cột <code>c<sub>i</sub></code>.</li>
</ol>

<p>Cho <code>m</code>, <code>n</code> và <code>indices</code>, hãy trả về <em><strong>số ô có giá trị lẻ</strong> trong ma trận sau khi thực hiện thao tác tăng tại mọi vị trí trong </em><code>indices</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1252.Cells%20with%20Odd%20Values%20in%20a%20Matrix/images/e1.png" style="width: 600px; height: 118px;" />
<pre>
<strong>Đầu vào:</strong> m = 2, n = 3, indices = [[0,1],[1,1]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Ma trận ban đầu = [[0,0,0],[0,0,0]].
Sau lần tăng thứ nhất, ma trận trở thành [[1,2,1],[0,1,0]].
Ma trận cuối cùng là [[1,3,1],[1,3,1]], có 6 số lẻ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1252.Cells%20with%20Odd%20Values%20in%20a%20Matrix/images/e2.png" style="width: 600px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> m = 2, n = 2, indices = [[1,1],[0,0]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Ma trận cuối cùng = [[2,2],[2,2]]. Ma trận không có số lẻ nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>1 &lt;= indices.length &lt;= 100</code></li>
	<li><code>0 &lt;= r<sub>i</sub> &lt; m</code></li>
	<li><code>0 &lt;= c<sub>i</sub> &lt; n</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài này trong thời gian <code>O(n + m + indices.length)</code> và chỉ dùng thêm <code>O(n + m)</code> không gian không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Vì $m,n \le 50$, ta có thể tạo ma trận, mỗi thao tác tăng toàn bộ một hàng và một cột, rồi đếm số lẻ. Cách mô phỏng này bám sát đề bài và là nền tảng cho các tối ưu tiếp theo.

<!-- thinking:end -->

Ta tạo ma trận $g$ để lưu kết quả các thao tác. Với mỗi cặp $(r_i, c_i)$ trong $\textit{indices}$, tăng $1$ cho mọi phần tử trên hàng thứ $r_i$ và mọi phần tử trên cột thứ $c_i$.

Sau khi mô phỏng xong, ta duyệt ma trận và đếm các số lẻ.

Độ phức tạp thời gian là $O(k \times (m + n) + m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $k$ là độ dài của $\textit{indices}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def oddCells(self, m: int, n: int, indices: List[List[int]]) -> int:
        g = [[0] * n for _ in range(m)]
        for r, c in indices:
            for i in range(m):
                g[i][c] += 1
            for j in range(n):
                g[r][j] += 1
        return sum(v % 2 for row in g for v in row)
```

#### Java

```java
class Solution {
    public int oddCells(int m, int n, int[][] indices) {
        int[][] g = new int[m][n];
        for (int[] e : indices) {
            int r = e[0], c = e[1];
            for (int i = 0; i < m; ++i) {
                g[i][c]++;
            }
            for (int j = 0; j < n; ++j) {
                g[r][j]++;
            }
        }
        int ans = 0;
        for (int[] row : g) {
            for (int v : row) {
                ans += v % 2;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int oddCells(int m, int n, vector<vector<int>>& indices) {
        vector<vector<int>> g(m, vector<int>(n));
        for (auto& e : indices) {
            int r = e[0], c = e[1];
            for (int i = 0; i < m; ++i) {
                ++g[i][c];
            }
            for (int j = 0; j < n; ++j) {
                ++g[r][j];
            }
        }
        int ans = 0;
        for (auto& row : g) {
            for (int v : row) {
                ans += v % 2;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func oddCells(m int, n int, indices [][]int) int {
	g := make([][]int, m)
	for i := range g {
		g[i] = make([]int, n)
	}
	for _, e := range indices {
		r, c := e[0], e[1]
		for i := 0; i < m; i++ {
			g[i][c]++
		}
		for j := 0; j < n; j++ {
			g[r][j]++
		}
	}
	ans := 0
	for _, row := range g {
		for _, v := range row {
			ans += v % 2
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tối ưu không gian

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 cập nhật $O(m+n)$ ô cho mỗi thao tác và lưu toàn bộ ma trận. Giá trị ô $(i,j)$ cuối cùng bằng số lần tăng hàng $i$ cộng số lần tăng cột $j$. Hai mảng lưu số lần tăng này; sau đó ta kiểm tra tính chẵn lẻ của từng ô. Không gian phụ là $O(m+n)$.

<!-- thinking:end -->

Ta dùng mảng hàng $\textit{row}$ và mảng cột $\textit{col}$ để ghi lại số lần mỗi hàng và cột được tăng. Với mỗi cặp $(r_i, c_i)$ trong $\textit{indices}$, lần lượt tăng $\textit{row}[r_i]$ và $\textit{col}[c_i]$ thêm $1$.

Sau khi hoàn tất các thao tác, giá trị tại vị trí $(i, j)$ được tính bằng $\textit{row}[i] + \textit{col}[j]$. Ta duyệt ma trận và đếm số ô có giá trị lẻ.

Độ phức tạp thời gian là $O(k + m \times n)$ và độ phức tạp không gian là $O(m + n)$, trong đó $k$ là độ dài của $\textit{indices}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def oddCells(self, m: int, n: int, indices: List[List[int]]) -> int:
        row = [0] * m
        col = [0] * n
        for r, c in indices:
            row[r] += 1
            col[c] += 1
        return sum((i + j) % 2 for i in row for j in col)
```

#### Java

```java
class Solution {
    public int oddCells(int m, int n, int[][] indices) {
        int[] row = new int[m];
        int[] col = new int[n];
        for (int[] e : indices) {
            int r = e[0], c = e[1];
            row[r]++;
            col[c]++;
        }
        int ans = 0;
        for (int i : row) {
            for (int j : col) {
                ans += (i + j) % 2;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int oddCells(int m, int n, vector<vector<int>>& indices) {
        vector<int> row(m);
        vector<int> col(n);
        for (auto& e : indices) {
            int r = e[0], c = e[1];
            row[r]++;
            col[c]++;
        }
        int ans = 0;
        for (int i : row) {
            for (int j : col) {
                ans += (i + j) % 2;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func oddCells(m int, n int, indices [][]int) int {
	row := make([]int, m)
	col := make([]int, n)
	for _, e := range indices {
		r, c := e[0], e[1]
		row[r]++
		col[c]++
	}
	ans := 0
	for _, i := range row {
		for _, j := range col {
			ans += (i + j) % 2
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Tối ưu bằng toán học

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 2 vẫn duyệt mọi ô. Ô $(i,j)$ có giá trị lẻ khi và chỉ khi số lần tăng hàng và cột có tính chẵn lẻ khác nhau. Nếu có $cnt1$ hàng lẻ và $cnt2$ cột lẻ, số ô lẻ là $cnt1(n-cnt2)+cnt2(m-cnt1)$; độ phức tạp thời gian là $O(k+m+n)$.

<!-- thinking:end -->

Ta nhận thấy phần tử ở vị trí $(i, j)$ trong ma trận chỉ lẻ khi đúng một trong hai giá trị $\textit{row}[i]$ và $\textit{col}[j]$ là lẻ, giá trị còn lại là chẵn.

Ta đếm số giá trị lẻ trong $\textit{row}$, gọi là $\textit{cnt1}$, và số giá trị lẻ trong $\textit{col}$, gọi là $\textit{cnt2}$. Vì vậy, tổng số ô lẻ là $\textit{cnt1} \times (n - \textit{cnt2}) + \textit{cnt2} \times (m - \textit{cnt1})$.

Độ phức tạp thời gian là $O(k + m + n)$ và độ phức tạp không gian là $O(m + n)$, trong đó $k$ là độ dài của $\textit{indices}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def oddCells(self, m: int, n: int, indices: List[List[int]]) -> int:
        row = [0] * m
        col = [0] * n
        for r, c in indices:
            row[r] += 1
            col[c] += 1
        cnt1 = sum(v % 2 for v in row)
        cnt2 = sum(v % 2 for v in col)
        return cnt1 * (n - cnt2) + cnt2 * (m - cnt1)
```

#### Java

```java
class Solution {
    public int oddCells(int m, int n, int[][] indices) {
        int[] row = new int[m];
        int[] col = new int[n];
        for (int[] e : indices) {
            int r = e[0], c = e[1];
            row[r]++;
            col[c]++;
        }
        int cnt1 = 0, cnt2 = 0;
        for (int v : row) {
            cnt1 += v % 2;
        }
        for (int v : col) {
            cnt2 += v % 2;
        }
        return cnt1 * (n - cnt2) + cnt2 * (m - cnt1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int oddCells(int m, int n, vector<vector<int>>& indices) {
        vector<int> row(m);
        vector<int> col(n);
        for (auto& e : indices) {
            int r = e[0], c = e[1];
            row[r]++;
            col[c]++;
        }
        int cnt1 = 0, cnt2 = 0;
        for (int v : row) {
            cnt1 += v % 2;
        }
        for (int v : col) {
            cnt2 += v % 2;
        }
        return cnt1 * (n - cnt2) + cnt2 * (m - cnt1);
    }
};
```

#### Go

```go
func oddCells(m int, n int, indices [][]int) int {
	row := make([]int, m)
	col := make([]int, n)
	for _, e := range indices {
		r, c := e[0], e[1]
		row[r]++
		col[c]++
	}
	cnt1, cnt2 := 0, 0
	for _, v := range row {
		cnt1 += v % 2
	}
	for _, v := range col {
		cnt2 += v % 2
	}
	return cnt1*(n-cnt2) + cnt2*(m-cnt1)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
