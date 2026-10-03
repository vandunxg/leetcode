---
comments: true
difficulty: Medium
tags:
    - Graph
---

<!-- problem:start -->

# [2077. Paths in Maze That Lead to Same Room 🔒](https://leetcode.com/problems/paths-in-maze-that-lead-to-same-room)

[中文文档](/solution/2000-2099/2077.Paths%20in%20Maze%20That%20Lead%20to%20Same%20Room/README.md)

## Mô tả

<!-- description:start -->

<p>Một mê cung gồm <code>n</code> căn phòng được đánh số từ <code>1</code> đến <code>n</code>, trong đó một số căn phòng được nối với nhau bằng các hành lang. Bạn được cho một mảng số nguyên 2 chiều <code>corridors</code>, trong đó <code>corridors[i] = [room1<sub>i</sub>, room2<sub>i</sub>]</code> cho biết có một hành lang nối <code>room1<sub>i</sub></code> và <code>room2<sub>i</sub></code>, cho phép một người trong mê cung đi từ <code>room1<sub>i</sub></code> đến <code>room2<sub>i</sub></code> <strong>và ngược lại</strong>.</p>

<p>Người thiết kế mê cung muốn biết mê cung này gây bối rối đến mức nào. <strong>Điểm</strong> <strong>gây bối rối</strong> của mê cung là số chu trình khác nhau có <strong>độ dài 3</strong>.</p>

<ul>
	<li>Ví dụ, <code>1 &rarr; 2 &rarr; 3 &rarr; 1</code> là một chu trình độ dài 3, nhưng <code>1 &rarr; 2 &rarr; 3 &rarr; 4</code> và <code>1 &rarr; 2 &rarr; 3 &rarr; 2 &rarr; 1</code> thì không phải.</li>
</ul>

<p>Hai chu trình được coi là <strong>khác nhau</strong> nếu một hoặc nhiều căn phòng đi qua trong chu trình thứ nhất <strong>không</strong> xuất hiện trong chu trình thứ hai.</p>

<p>Trả về <em>điểm</em> <em><strong>gây bối rối</strong><strong> của mê cung</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2077.Paths%20in%20Maze%20That%20Lead%20to%20Same%20Room/images/image-20211114164827-1.png" style="width: 440px; height: 350px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, corridors = [[1,2],[5,2],[4,1],[2,4],[3,1],[3,4]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Một chu trình độ dài 3 là 4 &rarr; 1 &rarr; 3 &rarr; 4, được đánh dấu màu đỏ.
Lưu ý rằng đây cũng là chu trình 3 &rarr; 4 &rarr; 1 &rarr; 3 hoặc 1 &rarr; 3 &rarr; 4 &rarr; 1 vì các căn phòng là như nhau.
Một chu trình độ dài 3 khác là 1 &rarr; 2 &rarr; 4 &rarr; 1, được đánh dấu màu xanh dương.
Do đó, có hai chu trình độ dài 3 khác nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2077.Paths%20in%20Maze%20That%20Lead%20to%20Same%20Room/images/image-20211114164851-2.png" style="width: 329px; height: 250px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, corridors = [[1,2],[3,4]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Không có chu trình nào có độ dài 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= corridors.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>corridors[i].length == 2</code></li>
	<li><code>1 &lt;= room1<sub>i</sub>, room2<sub>i</sub> &lt;= n</code></li>
	<li><code>room1<sub>i</sub> != room2<sub>i</sub></code></li>
	<li>Không có hành lang trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các chu trình độ dài 3. Lưu các cạnh vô hướng trong các tập kề để kiểm tra trong $O(1)$. Với mỗi đỉnh, xét từng cặp láng giềng không có thứ tự và giữ lại những cặp cũng kề nhau.
>
> Mỗi tam giác được đếm ba lần, vì vậy chia kết quả cho $3$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfPaths(self, n: int, corridors: List[List[int]]) -> int:
        g = defaultdict(set)
        for a, b in corridors:
            g[a].add(b)
            g[b].add(a)
        ans = 0
        for i in range(1, n + 1):
            for j, k in combinations(g[i], 2):
                if j in g[k]:
                    ans += 1
        return ans // 3
```

#### Java

```java
class Solution {
    public int numberOfPaths(int n, int[][] corridors) {
        Set<Integer>[] g = new Set[n + 1];
        for (int i = 0; i <= n; ++i) {
            g[i] = new HashSet<>();
        }
        for (var c : corridors) {
            int a = c[0], b = c[1];
            g[a].add(b);
            g[b].add(a);
        }
        int ans = 0;
        for (int c = 1; c <= n; ++c) {
            var nxt = new ArrayList<>(g[c]);
            int m = nxt.size();
            for (int i = 0; i < m; ++i) {
                for (int j = i + 1; j < m; ++j) {
                    int a = nxt.get(i), b = nxt.get(j);
                    if (g[b].contains(a)) {
                        ++ans;
                    }
                }
            }
        }
        return ans / 3;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfPaths(int n, vector<vector<int>>& corridors) {
        vector<unordered_set<int>> g(n + 1);
        for (auto& c : corridors) {
            int a = c[0], b = c[1];
            g[a].insert(b);
            g[b].insert(a);
        }
        int ans = 0;
        for (int c = 1; c <= n; ++c) {
            vector<int> nxt;
            nxt.assign(g[c].begin(), g[c].end());
            int m = nxt.size();
            for (int i = 0; i < m; ++i) {
                for (int j = i + 1; j < m; ++j) {
                    int a = nxt[i], b = nxt[j];
                    ans += g[b].count(a);
                }
            }
        }
        return ans / 3;
    }
};
```

#### Go

```go
func numberOfPaths(n int, corridors [][]int) int {
	g := make([]map[int]bool, n+1)
	for i := range g {
		g[i] = make(map[int]bool)
	}
	for _, c := range corridors {
		a, b := c[0], c[1]
		g[a][b] = true
		g[b][a] = true
	}
	ans := 0
	for c := 1; c <= n; c++ {
		nxt := []int{}
		for v := range g[c] {
			nxt = append(nxt, v)
		}
		m := len(nxt)
		for i := 0; i < m; i++ {
			for j := i + 1; j < m; j++ {
				a, b := nxt[i], nxt[j]
				if g[b][a] {
					ans++
				}
			}
		}
	}
	return ans / 3
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
