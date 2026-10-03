---
comments: true
difficulty: Medium
rating: 1704
source: Weekly Contest 251 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Backtracking
    - Bitmask
    - Hungarian Algorithm
    - Bipartite Graph
    - Graph Matching
    - Min-Cost Flow
    - SSP
    - Perfect Matching
    - Network Flow
---

<!-- problem:start -->

# [1947. Maximum Compatibility Score Sum](https://leetcode.com/problems/maximum-compatibility-score-sum)

[中文文档](/solution/1900-1999/1947.Maximum%20Compatibility%20Score%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Có một bảng khảo sát gồm <code>n</code> câu hỏi, mỗi câu trả lời là <code>0</code> (không) hoặc <code>1</code> (có).</p>

<p>Bảng khảo sát được đưa cho <code>m</code> sinh viên, được đánh số từ <code>0</code> đến <code>m - 1</code>, và <code>m</code> mentor, cũng được đánh số từ <code>0</code> đến <code>m - 1</code>. Câu trả lời của sinh viên được biểu diễn bằng một mảng số nguyên hai chiều <code>students</code>, trong đó <code>students[i]</code> là mảng số nguyên chứa câu trả lời của sinh viên <code>i<sup>th</sup></code> (<strong>đánh số từ 0</strong>). Câu trả lời của mentor được biểu diễn bằng một mảng số nguyên hai chiều <code>mentors</code>, trong đó <code>mentors[j]</code> là mảng số nguyên chứa câu trả lời của mentor <code>j<sup>th</sup></code> (<strong>đánh số từ 0</strong>).</p>

<p>Mỗi sinh viên sẽ được ghép với <strong>một</strong> mentor, và mỗi mentor sẽ được ghép với <strong>một</strong> sinh viên. <strong>Điểm tương thích</strong> của một cặp sinh viên-mentor là số câu trả lời giống nhau giữa hai người.</p>

<ul>
	<li>Ví dụ, nếu câu trả lời của sinh viên là <code>[1, <u>0</u>, <u>1</u>]</code> và câu trả lời của mentor là <code>[0, <u>0</u>, <u>1</u>]</code>, thì điểm tương thích của họ là 2 vì chỉ câu trả lời thứ hai và thứ ba giống nhau.</li>
</ul>

<p>Nhiệm vụ của bạn là tìm cách ghép cặp sinh viên-mentor tối ưu để <strong>tối đa hóa</strong> <strong>tổng điểm tương thích</strong>.</p>

<p>Cho hai mảng <code>students</code> và <code>mentors</code>, hãy trả về <em><strong>tổng điểm tương thích lớn nhất</strong> có thể đạt được.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> students = [[1,1,0],[1,0,1],[0,0,1]], mentors = [[1,0,0],[0,0,1],[1,1,0]]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong>&nbsp;Ta ghép sinh viên với mentor như sau:
- sinh viên 0 với mentor 2, có điểm tương thích là 3.
- sinh viên 1 với mentor 0, có điểm tương thích là 2.
- sinh viên 2 với mentor 1, có điểm tương thích là 3.
Tổng điểm tương thích là 3 + 2 + 3 = 8.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> students = [[0,0],[0,0],[0,0]], mentors = [[1,1],[1,1],[1,1]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Điểm tương thích của mọi cặp sinh viên-mentor đều là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == students.length == mentors.length</code></li>
	<li><code>n == students[i].length == mentors[j].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 8</code></li>
	<li><code>students[i][k]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>mentors[j][k]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Quay lui

<!-- thinking:start -->

> **Tư duy**
>
> Sinh viên và mentor tạo thành một phép ghép song ánh sao cho tổng điểm là lớn nhất. Vì $m\le 8$ nên có thể duyệt hết $8!$ hoán vị.
>
> Trước tiên, ta tính số câu trả lời giống nhau $g[i][j]$, sau đó quay lui qua các mentor chưa được sử dụng và duy trì tổng điểm lớn nhất.
>
> Mảng $\textit{vis}$ ngăn việc ghép trùng mentor; khi đã xử lý xong $m$ sinh viên thì cập nhật đáp án.

<!-- thinking:end -->

Trước tiên, ta có thể tiền xử lý điểm tương thích $g[i][j]$ giữa mỗi sinh viên $i$ và mentor $j$, sau đó dùng thuật toán quay lui để giải bài toán.

Định nghĩa hàm $\textit{dfs}(i, s)$, trong đó $i$ là sinh viên hiện đang được xử lý, còn $s$ là tổng điểm tương thích hiện tại.

Trong $\textit{dfs}(i, s)$, nếu $i \geq m$, nghĩa là tất cả sinh viên đã được ghép, khi đó ta cập nhật đáp án thành $\max(\textit{ans}, s)$. Nếu không, ta duyệt mentor có thể được ghép với sinh viên thứ $i$, rồi đệ quy xử lý sinh viên tiếp theo. Trong quá trình này, ta dùng một mảng $\textit{vis}$ để ghi nhận những mentor đã được ghép nhằm tránh ghép trùng.

Ta gọi $\textit{dfs}(0, 0)$ để tìm tổng điểm tương thích lớn nhất.

Độ phức tạp thời gian là $O(m!)$, độ phức tạp không gian là $O(m^2)$. Trong đó, $m$ là số sinh viên và mentor.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxCompatibilitySum(
        self, students: List[List[int]], mentors: List[List[int]]
    ) -> int:
        def dfs(i: int, s: int):
            if i >= m:
                nonlocal ans
                ans = max(ans, s)
                return
            for j in range(m):
                if not vis[j]:
                    vis[j] = True
                    dfs(i + 1, s + g[i][j])
                    vis[j] = False

        ans = 0
        m = len(students)
        vis = [False] * m
        g = [[0] * m for _ in range(m)]
        for i, x in enumerate(students):
            for j, y in enumerate(mentors):
                g[i][j] = sum(a == b for a, b in zip(x, y))
        dfs(0, 0)
        return ans
```

#### Java

```java
class Solution {
    private int m;
    private int ans;
    private int[][] g;
    private boolean[] vis;

    public int maxCompatibilitySum(int[][] students, int[][] mentors) {
        m = students.length;
        g = new int[m][m];
        vis = new boolean[m];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < m; ++j) {
                for (int k = 0; k < students[i].length; ++k) {
                    if (students[i][k] == mentors[j][k]) {
                        ++g[i][j];
                    }
                }
            }
        }
        dfs(0, 0);
        return ans;
    }

    private void dfs(int i, int s) {
        if (i >= m) {
            ans = Math.max(ans, s);
            return;
        }
        for (int j = 0; j < m; ++j) {
            if (!vis[j]) {
                vis[j] = true;
                dfs(i + 1, s + g[i][j]);
                vis[j] = false;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxCompatibilitySum(vector<vector<int>>& students, vector<vector<int>>& mentors) {
        int m = students.size();
        int n = students[0].size();
        vector<vector<int>> g(m, vector<int>(m));
        vector<bool> vis(m);
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < m; ++j) {
                for (int k = 0; k < n; ++k) {
                    g[i][j] += students[i][k] == mentors[j][k];
                }
            }
        }
        int ans = 0;
        auto dfs = [&](this auto&& dfs, int i, int s) {
            if (i >= m) {
                ans = max(ans, s);
                return;
            }
            for (int j = 0; j < m; ++j) {
                if (!vis[j]) {
                    vis[j] = true;
                    dfs(i + 1, s + g[i][j]);
                    vis[j] = false;
                }
            }
        };
        dfs(0, 0);
        return ans;
    }
};
```

#### Go

```go
func maxCompatibilitySum(students [][]int, mentors [][]int) (ans int) {
	m, n := len(students), len(students[0])
	g := make([][]int, m)
	vis := make([]bool, m)
	for i, x := range students {
		g[i] = make([]int, m)
		for j, y := range mentors {
			for k := 0; k < n; k++ {
				if x[k] == y[k] {
					g[i][j]++
				}
			}
		}
	}
	var dfs func(int, int)
	dfs = func(i, s int) {
		if i == m {
			ans = max(ans, s)
			return
		}
		for j := 0; j < m; j++ {
			if !vis[j] {
				vis[j] = true
				dfs(i+1, s+g[i][j])
				vis[j] = false
			}
		}
	}
	dfs(0, 0)
	return
}
```

#### TypeScript

```ts
function maxCompatibilitySum(students: number[][], mentors: number[][]): number {
    let ans = 0;
    const m = students.length;
    const vis: boolean[] = Array(m).fill(false);
    const g: number[][] = Array.from({ length: m }, () => Array(m).fill(0));
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < m; ++j) {
            for (let k = 0; k < students[i].length; ++k) {
                if (students[i][k] === mentors[j][k]) {
                    g[i][j]++;
                }
            }
        }
    }
    const dfs = (i: number, s: number): void => {
        if (i >= m) {
            ans = Math.max(ans, s);
            return;
        }
        for (let j = 0; j < m; ++j) {
            if (!vis[j]) {
                vis[j] = true;
                dfs(i + 1, s + g[i][j]);
                vis[j] = false;
            }
        }
    };
    dfs(0, 0);
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_compatibility_sum(students: Vec<Vec<i32>>, mentors: Vec<Vec<i32>>) -> i32 {
        let mut ans = 0;
        let m = students.len();
        let mut vis = vec![false; m];
        let mut g = vec![vec![0; m]; m];

        for i in 0..m {
            for j in 0..m {
                for k in 0..students[i].len() {
                    if students[i][k] == mentors[j][k] {
                        g[i][j] += 1;
                    }
                }
            }
        }

        fn dfs(i: usize, s: i32, m: usize, g: &Vec<Vec<i32>>, vis: &mut Vec<bool>, ans: &mut i32) {
            if i >= m {
                *ans = (*ans).max(s);
                return;
            }
            for j in 0..m {
                if !vis[j] {
                    vis[j] = true;
                    dfs(i + 1, s + g[i][j], m, g, vis, ans);
                    vis[j] = false;
                }
            }
        }

        dfs(0, 0, m, &g, &mut vis, &mut ans);
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} students
 * @param {number[][]} mentors
 * @return {number}
 */
var maxCompatibilitySum = function (students, mentors) {
    let ans = 0;
    const m = students.length;
    const vis = Array(m).fill(false);
    const g = Array.from({ length: m }, () => Array(m).fill(0));

    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < m; ++j) {
            for (let k = 0; k < students[i].length; ++k) {
                if (students[i][k] === mentors[j][k]) {
                    g[i][j]++;
                }
            }
        }
    }

    const dfs = function (i, s) {
        if (i >= m) {
            ans = Math.max(ans, s);
            return;
        }
        for (let j = 0; j < m; ++j) {
            if (!vis[j]) {
                vis[j] = true;
                dfs(i + 1, s + g[i][j]);
                vis[j] = false;
            }
        }
    };

    dfs(0, 0);
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
