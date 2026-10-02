---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [864. Shortest Path to Get All Keys](https://leetcode.com/problems/shortest-path-to-get-all-keys)

[中文文档](/solution/0800-0899/0864.Shortest%20Path%20to%20Get%20All%20Keys/README.md)

## Mô tả

<!-- description:start -->

<p>Cho lưới <code>grid</code> kích thước <code>m x n</code>, trong đó:</p>

<ul>
	<li><code>&#39;.&#39;</code> là ô trống.</li>
	<li><code>&#39;#&#39;</code> là tường.</li>
	<li><code>&#39;@&#39;</code> là vị trí bắt đầu.</li>
	<li>Chữ cái viết thường biểu diễn chìa khóa.</li>
	<li>Chữ cái viết hoa biểu diễn ổ khóa.</li>
</ul>

<p>Bạn bắt đầu tại vị trí xuất phát; mỗi bước đi là di chuyển một ô theo một trong bốn hướng. Bạn không thể đi ra ngoài lưới hoặc đi vào tường.</p>

<p>Khi đi qua chìa khóa, bạn có thể nhặt nó. Bạn không thể đi qua ổ khóa nếu chưa có chìa khóa tương ứng.</p>

<p>Với một giá trị <code><font face="monospace">1 &lt;= k &lt;= 6</font></code>, lưới có đúng một chữ cái viết thường và một chữ cái viết hoa tương ứng trong <code>k</code> chữ cái đầu tiên của bảng chữ cái tiếng Anh. Nghĩa là mỗi ổ khóa có đúng một chìa khóa tương ứng và mỗi chìa khóa có đúng một ổ khóa; các chữ cái dùng làm chìa khóa và ổ khóa cũng được chọn theo đúng thứ tự bảng chữ cái tiếng Anh.</p>

<p>Trả về <em>số bước ít nhất để lấy được tất cả chìa khóa</em>. Nếu không thể, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0864.Shortest%20Path%20to%20Get%20All%20Keys/images/lc-keys2.jpg" style="width: 404px; height: 245px;" />
<pre>
<strong>Đầu vào:</strong> grid = [&quot;@.a..&quot;,&quot;###.#&quot;,&quot;b.A.B&quot;]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Lưu ý, mục tiêu là lấy tất cả chìa khóa chứ không phải mở mọi ổ khóa.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0864.Shortest%20Path%20to%20Get%20All%20Keys/images/lc-key2.jpg" style="width: 404px; height: 245px;" />
<pre>
<strong>Đầu vào:</strong> grid = [&quot;@..aA&quot;,&quot;..B#.&quot;,&quot;....b&quot;]
<strong>Đầu ra:</strong> 6
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0864.Shortest%20Path%20to%20Get%20All%20Keys/images/lc-keys3.jpg" style="width: 244px; height: 85px;" />
<pre>
<strong>Đầu vào:</strong> grid = [&quot;@Aa&quot;]
<strong>Đầu ra:</strong> -1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 30</code></li>
	<li><code>grid[i][j]</code> là chữ cái tiếng Anh hoặc một trong các ký tự <code>&#39;.&#39;</code>, <code>&#39;#&#39;</code>, <code>&#39;@&#39;</code>.&nbsp;</li>
	<li>Trong lưới có đúng một&nbsp;<code>&#39;@&#39;</code>.</li>
	<li>Số chìa khóa trong lưới nằm trong khoảng <code>[1, 6]</code>.</li>
	<li>Mỗi chìa khóa trong lưới là <strong>duy nhất</strong>.</li>
	<li>Mỗi chìa khóa trong lưới đều có ổ khóa tương ứng.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nén trạng thái + BFS

<!-- thinking:start -->

> **Tư duy**
>
> Cần nhặt mọi chìa khóa; ổ khóa sẽ chặn đường nếu chưa có chìa tương ứng. Lưới tối đa $30\times 30$ và có không quá $6$ chìa, nên có thể dùng (vị trí, mask chìa khóa) làm trạng thái BFS.
>
> $(i,j,\textit{mask})$ biểu diễn ô hiện tại và các chìa khóa đang có. Bỏ qua tường và ổ khóa chưa mở; khi nhặt chìa khóa thì bật bit tương ứng. Lần đầu đạt mask đầy đủ là đường đi ngắn nhất.

<!-- thinking:end -->

Theo đề bài, ta bắt đầu ở vị trí xuất phát, di chuyển theo bốn hướng (lên, xuống, trái, phải) để nhặt tất cả chìa khóa, rồi trả về số bước ít nhất cần thiết. Nếu không thể nhặt đủ chìa khóa thì trả về $-1$.

Đầu tiên, duyệt lưới 2D để tìm vị trí bắt đầu $(si, sj)$ và đếm số chìa khóa $k$.

Sau đó, dùng Breadth-First Search (BFS) để giải bài toán. Vì số chìa khóa nằm trong khoảng từ $1$ đến $6$, ta có thể dùng một số nhị phân biểu diễn trạng thái chìa khóa: bit thứ $i$ bằng $1$ nghĩa là đã nhặt chìa khóa thứ $i$, còn bằng $0$ nghĩa là chưa nhặt.

For example, in the following case, there are $4$ bits set to $1$, indicating that keys `'b', 'c', 'd', 'f'` have been collected.

```
1 0 1 1 1 0
^   ^ ^ ^
f   d c b
```

Ta dùng queue $q$ để lưu vị trí hiện tại và trạng thái các chìa khóa đã nhặt, tức $(i, j, \textit{state})$. Trong đó $(i, j)$ là vị trí hiện tại, còn $\textit{state}$ biểu diễn trạng thái chìa khóa. Bit thứ $i$ của $\textit{state}$ bằng $1$ nghĩa là đã nhặt chìa khóa thứ $i$; ngược lại là chưa nhặt.

Ngoài ra, dùng hash table hoặc mảng $vis$ để ghi nhận liệu tổ hợp vị trí hiện tại và trạng thái chìa khóa đã được thăm hay chưa. Nếu đã thăm thì không cần duyệt lại. $vis[i][j][\textit{state}]$ cho biết vị trí $(i, j)$ với trạng thái chìa khóa $state$ đã được thăm hay chưa.

Bắt đầu từ vị trí $(si, sj)$, thêm trạng thái này vào queue $q$ và đặt $vis[si][sj][0]$ thành $true$ để đánh dấu vị trí ban đầu cùng trạng thái chưa nhặt chìa khóa đã được thăm.

Trong quá trình BFS, lấy trạng thái $(i, j, \textit{state})$ ở đầu queue và kiểm tra xem đã đạt đích chưa, tức là đã nhặt đủ chìa khóa hay chưa. Điều này tương đương với việc biểu diễn nhị phân của $state$ có $k$ bit $1$. Nếu đúng, trả về số bước hiện tại.

Nếu chưa đạt đích, ta thử di chuyển từ vị trí hiện tại theo bốn hướng (lên, xuống, trái, phải). Nếu có thể đến vị trí tiếp theo $(x, y)$ thì thêm $(x, y, nxt)$ vào queue $q$, trong đó $nxt$ là trạng thái chìa khóa sau khi đến vị trí đó.

Trước hết, $(x, y)$ phải nằm trong phạm vi lưới, tức $0 \leq x < m$ và $0 \leq y < n$. Nếu vị trí $(x, y)$ là tường, tức `grid[x][y] == '#'`, hoặc là ổ khóa nhưng ta chưa có chìa tương ứng, tức `grid[x][y] >= 'A' && grid[x][y] <= 'F' && (state >> (grid[x][y] - 'A') & 1) == 0)`, thì không thể đi đến đó. Nếu không, ta có thể di chuyển đến $(x, y)$.

Nếu quá trình tìm kiếm kết thúc mà vẫn chưa nhặt đủ chìa khóa thì trả về $-1$.

Độ phức tạp thời gian là $O(m \times n \times 2^k)$ và độ phức tạp không gian là $O(m \times n \times 2^k)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của lưới, còn $k$ là số chìa khóa.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestPathAllKeys(self, grid: List[str]) -> int:
        m, n = len(grid), len(grid[0])
        # Find the starting point (si, sj)
        si, sj = next((i, j) for i in range(m) for j in range(n) if grid[i][j] == '@')
        # Count the number of keys
        k = sum(v.islower() for row in grid for v in row)
        dirs = (-1, 0, 1, 0, -1)
        q = deque([(si, sj, 0)])
        vis = {(si, sj, 0)}
        ans = 0
        while q:
            for _ in range(len(q)):
                i, j, state = q.popleft()
                # If all keys are found, return the current step count
                if state == (1 << k) - 1:
                    return ans

                # Search in the four directions
                for a, b in pairwise(dirs):
                    x, y = i + a, j + b
                    nxt = state
                    # Within boundary limits
                    if 0 <= x < m and 0 <= y < n:
                        c = grid[x][y]
                        # It's a wall, or it's a lock but we don't have the key for it
                        if (
                            c == '#'
                            or c.isupper()
                            and (state & (1 << (ord(c) - ord('A')))) == 0
                        ):
                            continue
                        # It's a key
                        if c.islower():
                            # Update the state
                            nxt |= 1 << (ord(c) - ord('a'))
                        # If this state has not been visited, enqueue it
                        if (x, y, nxt) not in vis:
                            vis.add((x, y, nxt))
                            q.append((x, y, nxt))
            # Increment the step count
            ans += 1
        return -1
```

#### Java

```java
class Solution {
    private int[] dirs = {-1, 0, 1, 0, -1};

    public int shortestPathAllKeys(String[] grid) {
        int m = grid.length, n = grid[0].length();
        int k = 0;
        int si = 0, sj = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                char c = grid[i].charAt(j);
                if (Character.isLowerCase(c)) {
                    // Count the number of keys
                    ++k;
                } else if (c == '@') {
                    // Starting point
                    si = i;
                    sj = j;
                }
            }
        }
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {si, sj, 0});
        boolean[][][] vis = new boolean[m][n][1 << k];
        vis[si][sj][0] = true;
        int ans = 0;
        while (!q.isEmpty()) {
            for (int t = q.size(); t > 0; --t) {
                var p = q.poll();
                int i = p[0], j = p[1], state = p[2];
                // If all keys are found, return the current step count
                if (state == (1 << k) - 1) {
                    return ans;
                }
                // Search in the four directions
                for (int h = 0; h < 4; ++h) {
                    int x = i + dirs[h], y = j + dirs[h + 1];
                    // Within boundary limits
                    if (x >= 0 && x < m && y >= 0 && y < n) {
                        char c = grid[x].charAt(y);
                        // It's a wall, or it's a lock without the corresponding key
                        if (c == '#'
                            || (Character.isUpperCase(c) && ((state >> (c - 'A')) & 1) == 0)) {
                            continue;
                        }
                        int nxt = state;
                        // If it's a key
                        if (Character.isLowerCase(c)) {
                            // Update the state
                            nxt |= 1 << (c - 'a');
                        }
                        // If this state has not been visited, enqueue it
                        if (!vis[x][y][nxt]) {
                            vis[x][y][nxt] = true;
                            q.offer(new int[] {x, y, nxt});
                        }
                    }
                }
            }
            // Increment the step count
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
    const static inline vector<int> dirs = {-1, 0, 1, 0, -1};

    int shortestPathAllKeys(vector<string>& grid) {
        int m = grid.size(), n = grid[0].size();
        int k = 0;
        int si = 0, sj = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                char c = grid[i][j];
                // Count the number of keys
                if (islower(c)) ++k;
                // Starting point
                else if (c == '@')
                    si = i, sj = j;
            }
        }
        queue<tuple<int, int, int>> q{{{si, sj, 0}}};
        vector<vector<vector<bool>>> vis(m, vector<vector<bool>>(n, vector<bool>(1 << k)));
        vis[si][sj][0] = true;
        int ans = 0;
        while (!q.empty()) {
            for (int t = q.size(); t; --t) {
                auto [i, j, state] = q.front();
                q.pop();
                // If all keys are found, return the current step count
                if (state == (1 << k) - 1) return ans;
                // Search in the four directions
                for (int h = 0; h < 4; ++h) {
                    int x = i + dirs[h], y = j + dirs[h + 1];
                    // Within boundary limits
                    if (x >= 0 && x < m && y >= 0 && y < n) {
                        char c = grid[x][y];
                        // It's a wall, or it's a lock without the corresponding key
                        if (c == '#' || (isupper(c) && (state >> (c - 'A') & 1) == 0)) continue;
                        int nxt = state;
                        // If it's a key, update the state
                        if (islower(c)) nxt |= 1 << (c - 'a');
                        // If this state has not been visited, enqueue it
                        if (!vis[x][y][nxt]) {
                            vis[x][y][nxt] = true;
                            q.push({x, y, nxt});
                        }
                    }
                }
            }
            // Increment the step count
            ++ans;
        }
        return -1;
    }
};
```

#### Go

```go
func shortestPathAllKeys(grid []string) int {
	m, n := len(grid), len(grid[0])
	var k, si, sj int
	for i, row := range grid {
		for j, c := range row {
			if c >= 'a' && c <= 'z' {
				// Count the number of keys
				k++
			} else if c == '@' {
				// Starting point
				si, sj = i, j
			}
		}
	}
	type tuple struct{ i, j, state int }
	q := []tuple{tuple{si, sj, 0}}
	vis := map[tuple]bool{tuple{si, sj, 0}: true}
	dirs := []int{-1, 0, 1, 0, -1}
	ans := 0
	for len(q) > 0 {
		for t := len(q); t > 0; t-- {
			p := q[0]
			q = q[1:]
			i, j, state := p.i, p.j, p.state
			// If all keys are found, return the current step count
			if state == 1<<k-1 {
				return ans
			}
			// Search in the four directions
			for h := 0; h < 4; h++ {
				x, y := i+dirs[h], j+dirs[h+1]
				// Within boundary limits
				if x >= 0 && x < m && y >= 0 && y < n {
					c := grid[x][y]
					// It's a wall, or it's a lock without the corresponding key
					if c == '#' || (c >= 'A' && c <= 'Z' && (state>>(c-'A')&1 == 0)) {
						continue
					}
					nxt := state
					// If it's a key, update the state
					if c >= 'a' && c <= 'z' {
						nxt |= 1 << (c - 'a')
					}
					// If this state has not been visited, enqueue it
					if !vis[tuple{x, y, nxt}] {
						vis[tuple{x, y, nxt}] = true
						q = append(q, tuple{x, y, nxt})
					}
				}
			}
		}
		// Increment the step count
		ans++
	}
	return -1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
