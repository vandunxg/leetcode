---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [749. Contain Virus](https://leetcode.com/problems/contain-virus)

[中文文档](/solution/0700-0799/0749.Contain%20Virus/README.md)

## Mô tả

<!-- description:start -->

<p>Virus đang lây lan nhanh chóng; nhiệm vụ của bạn là cách ly khu vực bị nhiễm bằng cách dựng tường.</p>

<p>Thế giới được mô hình hóa thành lưới nhị phân <code>m x n</code> <code>isInfected</code>, trong đó <code>isInfected[i][j] == 0</code> biểu thị ô chưa nhiễm và <code>isInfected[i][j] == 1</code> biểu thị ô đã nhiễm virus. Có thể dựng một bức tường (và chỉ một bức tường) trên cạnh chung giữa hai ô kề nhau theo <strong>4 hướng</strong>.</p>

<p>Mỗi đêm, virus lây sang mọi ô lân cận theo cả bốn hướng, trừ khi bị tường chặn. Nguồn lực có hạn, nên mỗi ngày bạn chỉ có thể dựng tường quanh một vùng (tức vùng bị ảnh hưởng gồm các ô nhiễm liên thông) đang đe dọa nhiều ô chưa nhiễm nhất trong đêm tiếp theo. Đảm bảo <strong>không bao giờ có trường hợp hòa</strong>.</p>

<p>Trả về <em>số bức tường cần dùng để cách ly tất cả các vùng nhiễm</em>. Nếu cuối cùng toàn bộ thế giới bị nhiễm, trả về tổng số tường đã dựng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0749.Contain%20Virus/images/virus11-grid.jpg" style="width: 500px; height: 255px;" />
<pre>
<strong>Đầu vào:</strong> isInfected = [[0,1,0,0,0,0,0,1],[0,1,0,0,0,0,0,1],[0,0,0,0,0,0,0,1],[0,0,0,0,0,0,0,0]]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Có 2 vùng bị nhiễm.
Ngày thứ nhất, dựng 5 bức tường để cách ly vùng nhiễm bên trái. Bảng sau khi virus lây lan như sau:
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0749.Contain%20Virus/images/virus12edited-grid.jpg" style="width: 500px; height: 257px;" />
Ngày thứ hai, dựng 5 bức tường để cách ly vùng nhiễm bên phải. Virus đã được khống chế hoàn toàn.
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0749.Contain%20Virus/images/virus13edited-grid.jpg" style="width: 500px; height: 261px;" />
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0749.Contain%20Virus/images/virus2-grid.jpg" style="width: 653px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> isInfected = [[1,1,1],[1,0,1],[1,1,1]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Dù chỉ cứu được một ô, vẫn cần dựng 4 bức tường.
Lưu ý, tường chỉ được dựng trên cạnh chung giữa hai ô khác nhau.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> isInfected = [[1,1,1,0,0,0,0,0,0],[1,0,1,0,1,1,1,1,1],[1,1,1,0,0,0,0,0,0]]
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Vùng bên trái chỉ cần dựng thêm hai bức tường mới.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m ==&nbsp;isInfected.length</code></li>
	<li><code>n ==&nbsp;isInfected[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>isInfected[i][j]</code> bằng <code>0</code> hoặc <code>1</code>.</li>
	<li>Trong suốt quá trình, luôn có một vùng virus liên thông sẽ <strong>lây nhiễm nhiều ô chưa bị nhiễm hơn hẳn</strong> ở lượt tiếp theo.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đêm, virus chưa bị cách ly lan sang các ô lân cận; ta có thể dựng tường quanh vùng đe dọa nhiều ô nhất. Lưới đủ nhỏ để mô phỏng trực tiếp: tìm các vùng, cách ly một vùng rồi lan truyền virus.
>
> Mức độ đe dọa là số ô chưa nhiễm khác nhau ở biên; số tường cần dựng là độ dài biên. DFS ghi nhận các ô trong vùng, tập ô ở biên và số tường tương ứng.
>
> Cách ly vùng có biên lớn nhất (đánh dấu các ô trong vùng đã được khống chế và cộng số tường của vùng đó), cho virus lây sang các ô biên của những vùng còn lại, rồi lặp lại đến khi không còn virus hoạt động.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def containVirus(self, isInfected: List[List[int]]) -> int:
        def dfs(i, j):
            vis[i][j] = True
            areas[-1].append((i, j))
            for a, b in [[0, -1], [0, 1], [-1, 0], [1, 0]]:
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n:
                    if isInfected[x][y] == 1 and not vis[x][y]:
                        dfs(x, y)
                    elif isInfected[x][y] == 0:
                        c[-1] += 1
                        boundaries[-1].add((x, y))

        m, n = len(isInfected), len(isInfected[0])
        ans = 0
        while 1:
            vis = [[False] * n for _ in range(m)]
            areas = []
            c = []
            boundaries = []
            for i, row in enumerate(isInfected):
                for j, v in enumerate(row):
                    if v == 1 and not vis[i][j]:
                        areas.append([])
                        boundaries.append(set())
                        c.append(0)
                        dfs(i, j)
            if not areas:
                break
            idx = boundaries.index(max(boundaries, key=len))
            ans += c[idx]
            for k, area in enumerate(areas):
                if k == idx:
                    for i, j in area:
                        isInfected[i][j] = -1
                else:
                    for i, j in area:
                        for a, b in [[0, -1], [0, 1], [-1, 0], [1, 0]]:
                            x, y = i + a, j + b
                            if 0 <= x < m and 0 <= y < n and isInfected[x][y] == 0:
                                isInfected[x][y] = 1
        return ans
```

#### Java

```java
class Solution {
    private static final int[] DIRS = {-1, 0, 1, 0, -1};
    private List<Integer> c = new ArrayList<>();
    private List<List<Integer>> areas = new ArrayList<>();
    private List<Set<Integer>> boundaries = new ArrayList<>();
    private int[][] infected;
    private boolean[][] vis;
    private int m;
    private int n;

    public int containVirus(int[][] isInfected) {
        infected = isInfected;
        m = infected.length;
        n = infected[0].length;
        vis = new boolean[m][n];
        int ans = 0;
        while (true) {
            for (boolean[] row : vis) {
                Arrays.fill(row, false);
            }
            c.clear();
            areas.clear();
            boundaries.clear();
            for (int i = 0; i < m; ++i) {
                for (int j = 0; j < n; ++j) {
                    if (infected[i][j] == 1 && !vis[i][j]) {
                        c.add(0);
                        areas.add(new ArrayList<>());
                        boundaries.add(new HashSet<>());
                        dfs(i, j);
                    }
                }
            }
            if (areas.isEmpty()) {
                break;
            }
            int idx = max(boundaries);
            ans += c.get(idx);
            for (int t = 0; t < areas.size(); ++t) {
                if (t == idx) {
                    for (int v : areas.get(t)) {
                        int i = v / n, j = v % n;
                        infected[i][j] = -1;
                    }
                } else {
                    for (int v : areas.get(t)) {
                        int i = v / n, j = v % n;
                        for (int k = 0; k < 4; ++k) {
                            int x = i + DIRS[k], y = j + DIRS[k + 1];
                            if (x >= 0 && x < m && y >= 0 && y < n && infected[x][y] == 0) {
                                infected[x][y] = 1;
                            }
                        }
                    }
                }
            }
        }
        return ans;
    }

    private int max(List<Set<Integer>> boundaries) {
        int idx = 0;
        int mx = boundaries.get(0).size();
        for (int i = 1; i < boundaries.size(); ++i) {
            int t = boundaries.get(i).size();
            if (mx < t) {
                mx = t;
                idx = i;
            }
        }
        return idx;
    }

    private void dfs(int i, int j) {
        vis[i][j] = true;
        int idx = areas.size() - 1;
        areas.get(idx).add(i * n + j);
        for (int k = 0; k < 4; ++k) {
            int x = i + DIRS[k], y = j + DIRS[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n) {
                if (infected[x][y] == 1 && !vis[x][y]) {
                    dfs(x, y);
                } else if (infected[x][y] == 0) {
                    c.set(idx, c.get(idx) + 1);
                    boundaries.get(idx).add(x * n + y);
                }
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    const vector<int> dirs = {-1, 0, 1, 0, -1};
    vector<int> c;
    vector<vector<int>> areas;
    vector<unordered_set<int>> boundaries;
    vector<vector<int>> infected;
    vector<vector<bool>> vis;
    int m;
    int n;

    int containVirus(vector<vector<int>>& isInfected) {
        infected = isInfected;
        m = infected.size();
        n = infected[0].size();
        vis.assign(m, vector<bool>(n));
        int ans = 0;
        while (1) {
            for (int i = 0; i < m; ++i)
                for (int j = 0; j < n; ++j) vis[i][j] = false;
            c.clear();
            areas.clear();
            boundaries.clear();
            for (int i = 0; i < m; ++i) {
                for (int j = 0; j < n; ++j) {
                    if (infected[i][j] == 1 && !vis[i][j]) {
                        c.push_back(0);
                        areas.push_back({});
                        boundaries.push_back({});
                        dfs(i, j);
                    }
                }
            }
            if (areas.empty()) break;
            int idx = getMax();
            ans += c[idx];
            for (int t = 0; t < areas.size(); ++t) {
                if (t == idx) {
                    for (int v : areas[t]) {
                        int i = v / n, j = v % n;
                        infected[i][j] = -1;
                    }
                } else {
                    for (int v : areas[t]) {
                        int i = v / n, j = v % n;
                        for (int k = 0; k < 4; ++k) {
                            int x = i + dirs[k], y = j + dirs[k + 1];
                            if (x >= 0 && x < m && y >= 0 && y < n && infected[x][y] == 0) infected[x][y] = 1;
                        }
                    }
                }
            }
        }
        return ans;
    }

    int getMax() {
        int idx = 0;
        int mx = boundaries[0].size();
        for (int i = 1; i < boundaries.size(); ++i) {
            int t = boundaries[i].size();
            if (mx < t) {
                mx = t;
                idx = i;
            }
        }
        return idx;
    }

    void dfs(int i, int j) {
        vis[i][j] = true;
        areas.back().push_back(i * n + j);
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n) {
                if (infected[x][y] == 1 && !vis[x][y])
                    dfs(x, y);
                else if (infected[x][y] == 0) {
                    c.back() += 1;
                    boundaries.back().insert(x * n + y);
                }
            }
        }
    }
};
```

#### Go

```go
func containVirus(isInfected [][]int) int {
	m, n := len(isInfected), len(isInfected[0])
	ans := 0
	dirs := []int{-1, 0, 1, 0, -1}
	max := func(boundaries []map[int]bool) int {
		idx := 0
		mx := len(boundaries[0])
		for i, v := range boundaries {
			t := len(v)
			if mx < t {
				mx = t
				idx = i
			}
		}
		return idx
	}

	for {
		vis := make([][]bool, m)
		for i := range vis {
			vis[i] = make([]bool, n)
		}
		areas := []map[int]bool{}
		boundaries := []map[int]bool{}
		c := []int{}

		var dfs func(i, j int)
		dfs = func(i, j int) {
			vis[i][j] = true
			idx := len(areas) - 1
			areas[idx][i*n+j] = true
			for k := 0; k < 4; k++ {
				x, y := i+dirs[k], j+dirs[k+1]
				if x >= 0 && x < m && y >= 0 && y < n {
					if isInfected[x][y] == 1 && !vis[x][y] {
						dfs(x, y)
					} else if isInfected[x][y] == 0 {
						c[idx]++
						boundaries[idx][x*n+y] = true
					}
				}
			}
		}

		for i, row := range isInfected {
			for j, v := range row {
				if v == 1 && !vis[i][j] {
					areas = append(areas, map[int]bool{})
					boundaries = append(boundaries, map[int]bool{})
					c = append(c, 0)
					dfs(i, j)
				}
			}
		}
		if len(areas) == 0 {
			break
		}
		idx := max(boundaries)
		ans += c[idx]
		for t, area := range areas {
			if t == idx {
				for v := range area {
					i, j := v/n, v%n
					isInfected[i][j] = -1
				}
			} else {
				for v := range area {
					i, j := v/n, v%n
					for k := 0; k < 4; k++ {
						x, y := i+dirs[k], j+dirs[k+1]
						if x >= 0 && x < m && y >= 0 && y < n && isInfected[x][y] == 0 {
							isInfected[x][y] = 1
						}
					}
				}
			}
		}

	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
