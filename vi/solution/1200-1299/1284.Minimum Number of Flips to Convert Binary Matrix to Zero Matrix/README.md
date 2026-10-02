---
comments: true
difficulty: Hard
rating: 1810
source: Weekly Contest 166 Q4
tags:
    - Bit Manipulation
    - Breadth-First Search
    - Array
    - Hash Table
    - Matrix
---

<!-- problem:start -->

# [1284. Minimum Number of Flips to Convert Binary Matrix to Zero Matrix](https://leetcode.com/problems/minimum-number-of-flips-to-convert-binary-matrix-to-zero-matrix)

[中文文档](/solution/1200-1299/1284.Minimum%20Number%20of%20Flips%20to%20Convert%20Binary%20Matrix%20to%20Zero%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận nhị phân <code>m x n</code> <code>mat</code>. Trong một bước, bạn có thể chọn một ô và lật giá trị của nó cùng bốn ô láng giềng nếu chúng tồn tại (lật nghĩa là đổi <code>1</code> thành <code>0</code> và <code>0</code> thành <code>1</code>). Hai ô được gọi là láng giềng nếu chúng chung một cạnh.</p>

<p>Trả về <em>số bước ít nhất</em> cần để biến <code>mat</code> thành ma trận toàn số 0; nếu không thể thì trả về <code>-1</code>.</p>

<p><strong>Ma trận nhị phân</strong> là ma trận mà mọi ô chỉ có thể bằng <code>0</code> hoặc <code>1</code>.</p>

<p><strong>Ma trận toàn số 0</strong> là ma trận mà mọi ô đều bằng <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1284.Minimum%20Number%20of%20Flips%20to%20Convert%20Binary%20Matrix%20to%20Zero%20Matrix/images/matrix.png" style="width: 409px; height: 86px;" />
<pre>
<strong>Input:</strong> mat = [[0,0],[0,1]]
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Một cách thực hiện là lần lượt lật các ô (1, 0), (0, 1), rồi (1, 1) như hình minh họa.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> mat = [[0]]
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Ma trận đã cho là ma trận toàn số 0 nên không cần thay đổi.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> mat = [[1,0,0],[1,0,0]]
<strong>Output:</strong> -1
<strong>Giải thích:</strong> Không thể biến ma trận đã cho thành ma trận toàn số 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n == mat[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 3</code></li>
	<li><code>mat[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Lật một ô cũng làm lật các ô láng giềng. Vì $m,n \le 3$, có tối đa $2^9$ state. Tìm số lần lật ít nhất tương đương tìm đường đi ngắn nhất. Ta mã hóa ma trận thành một số nguyên; mỗi lượt đi XOR trạng thái của một ô cùng các ô láng giềng. Tầng BFS chạm tới $0$ chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minFlips(self, mat: List[List[int]]) -> int:
        m, n = len(mat), len(mat[0])
        state = sum(1 << (i * n + j) for i in range(m) for j in range(n) if mat[i][j])
        q = deque([state])
        vis = {state}
        ans = 0
        dirs = [0, -1, 0, 1, 0, 0]
        while q:
            for _ in range(len(q)):
                state = q.popleft()
                if state == 0:
                    return ans
                for i in range(m):
                    for j in range(n):
                        nxt = state
                        for k in range(5):
                            x, y = i + dirs[k], j + dirs[k + 1]
                            if not 0 <= x < m or not 0 <= y < n:
                                continue
                            if nxt & (1 << (x * n + y)):
                                nxt -= 1 << (x * n + y)
                            else:
                                nxt |= 1 << (x * n + y)
                        if nxt not in vis:
                            vis.add(nxt)
                            q.append(nxt)
            ans += 1
        return -1
```

#### Java

```java
class Solution {
    public int minFlips(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        int state = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (mat[i][j] == 1) {
                    state |= 1 << (i * n + j);
                }
            }
        }
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(state);
        Set<Integer> vis = new HashSet<>();
        vis.add(state);
        int ans = 0;
        int[] dirs = {0, -1, 0, 1, 0, 0};
        while (!q.isEmpty()) {
            for (int t = q.size(); t > 0; --t) {
                state = q.poll();
                if (state == 0) {
                    return ans;
                }
                for (int i = 0; i < m; ++i) {
                    for (int j = 0; j < n; ++j) {
                        int nxt = state;
                        for (int k = 0; k < 5; ++k) {
                            int x = i + dirs[k], y = j + dirs[k + 1];
                            if (x < 0 || x >= m || y < 0 || y >= n) {
                                continue;
                            }
                            if ((nxt & (1 << (x * n + y))) != 0) {
                                nxt -= 1 << (x * n + y);
                            } else {
                                nxt |= 1 << (x * n + y);
                            }
                        }
                        if (!vis.contains(nxt)) {
                            vis.add(nxt);
                            q.offer(nxt);
                        }
                    }
                }
            }
            ++ans;
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minFlips(vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size();
        int state = 0;
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                if (mat[i][j])
                    state |= (1 << (i * n + j));
        queue<int> q{{state}};
        unordered_set<int> vis{{state}};
        int ans = 0;
        vector<int> dirs = {0, -1, 0, 1, 0, 0};
        while (!q.empty()) {
            for (int t = q.size(); t; --t) {
                state = q.front();
                if (state == 0) return ans;
                q.pop();
                for (int i = 0; i < m; ++i) {
                    for (int j = 0; j < n; ++j) {
                        int nxt = state;
                        for (int k = 0; k < 5; ++k) {
                            int x = i + dirs[k], y = j + dirs[k + 1];
                            if (x < 0 || x >= m || y < 0 || y >= n) continue;
                            if ((nxt & (1 << (x * n + y))) != 0)
                                nxt -= 1 << (x * n + y);
                            else
                                nxt |= 1 << (x * n + y);
                        }
                        if (!vis.count(nxt)) {
                            vis.insert(nxt);
                            q.push(nxt);
                        }
                    }
                }
            }
            ++ans;
        }
        return -1;
    }
};
```

#### Go

```go
func minFlips(mat [][]int) int {
	m, n := len(mat), len(mat[0])
	state := 0
	for i, row := range mat {
		for j, v := range row {
			if v == 1 {
				state |= 1 << (i*n + j)
			}
		}
	}
	q := []int{state}
	vis := map[int]bool{state: true}
	ans := 0
	dirs := []int{0, -1, 0, 1, 0, 0}
	for len(q) > 0 {
		for t := len(q); t > 0; t-- {
			state = q[0]
			if state == 0 {
				return ans
			}
			q = q[1:]
			for i := 0; i < m; i++ {
				for j := 0; j < n; j++ {
					nxt := state
					for k := 0; k < 5; k++ {
						x, y := i+dirs[k], j+dirs[k+1]
						if x < 0 || x >= m || y < 0 || y >= n {
							continue
						}
						if (nxt & (1 << (x*n + y))) != 0 {
							nxt -= 1 << (x*n + y)
						} else {
							nxt |= 1 << (x*n + y)
						}
					}
					if !vis[nxt] {
						vis[nxt] = true
						q = append(q, nxt)
					}
				}
			}
		}
		ans++
	}
	return -1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
