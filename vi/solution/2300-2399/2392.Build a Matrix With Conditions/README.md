---
comments: true
difficulty: Hard
rating: 1960
source: Weekly Contest 308 Q4
tags:
    - Graph
    - Topological Sort
    - Array
    - Directed Acyclic Graph
    - Matrix
---

<!-- problem:start -->

# [2392. Build a Matrix With Conditions](https://leetcode.com/problems/build-a-matrix-with-conditions)

[中文文档](/solution/2300-2399/2392.Build%20a%20Matrix%20With%20Conditions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một số nguyên <strong>dương</strong> <code>k</code>. Bạn cũng được cung cấp:</p>

<ul>
	<li>một mảng số nguyên 2D <code>rowConditions</code> có kích thước <code>n</code>, trong đó <code>rowConditions[i] = [above<sub>i</sub>, below<sub>i</sub>]</code>, và</li>
	<li>một mảng số nguyên 2D <code>colConditions</code> có kích thước <code>m</code>, trong đó <code>colConditions[i] = [left<sub>i</sub>, right<sub>i</sub>]</code>.</li>
</ul>

<p>Hai mảng chứa các số nguyên từ <code>1</code> đến <code>k</code>.</p>

<p>Bạn phải xây dựng một ma trận <code>k x k</code> chứa mỗi số từ <code>1</code> đến <code>k</code> <strong>đúng một lần</strong>. Các ô còn lại phải có giá trị <code>0</code>.</p>

<p>Ma trận cũng phải thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Số <code>above<sub>i</sub></code> phải xuất hiện trong một <strong>hàng</strong> nằm <strong>phía trên</strong> hàng chứa số <code>below<sub>i</sub></code> với mọi <code>i</code> từ <code>0</code> đến <code>n - 1</code>.</li>
	<li>Số <code>left<sub>i</sub></code> phải xuất hiện trong một <strong>cột</strong> nằm <strong>bên trái</strong> cột chứa số <code>right<sub>i</sub></code> với mọi <code>i</code> từ <code>0</code> đến <code>m - 1</code>.</li>
</ul>

<p>Trả về <em><strong>bất kỳ</strong> ma trận nào thỏa mãn các điều kiện</em>. Nếu không tồn tại đáp án, trả về một ma trận rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2392.Build%20a%20Matrix%20With%20Conditions/images/gridosdrawio.png" style="width: 211px; height: 211px;" />
<pre>
<strong>Đầu vào:</strong> k = 3, rowConditions = [[1,2],[3,2]], colConditions = [[2,1],[3,2]]
<strong>Đầu ra:</strong> [[3,0,0],[0,0,1],[0,2,0]]
<strong>Giải thích:</strong> Sơ đồ bên trên cho thấy một ví dụ hợp lệ của ma trận thỏa mãn tất cả các điều kiện.
Các điều kiện về hàng là:
- Số 1 nằm ở hàng <u>1</u>, còn số 2 nằm ở hàng <u>2</u>, nên 1 nằm phía trên 2 trong ma trận.
- Số 3 nằm ở hàng <u>0</u>, còn số 2 nằm ở hàng <u>2</u>, nên 3 nằm phía trên 2 trong ma trận.
Các điều kiện về cột là:
- Số 2 nằm ở cột <u>1</u>, còn số 1 nằm ở cột <u>2</u>, nên 2 nằm bên trái 1 trong ma trận.
- Số 3 nằm ở cột <u>0</u>, còn số 2 nằm ở cột <u>1</u>, nên 3 nằm bên trái 2 trong ma trận.
Lưu ý rằng có thể có nhiều đáp án đúng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 3, rowConditions = [[1,2],[2,3],[3,1],[2,3]], colConditions = [[2,1]]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Từ hai điều kiện đầu tiên, 3 phải nằm phía dưới 1, nhưng điều kiện thứ ba yêu cầu 3 nằm phía trên 1 để được thỏa mãn.
Không có ma trận nào có thể thỏa mãn tất cả các điều kiện, nên ta trả về ma trận rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= k &lt;= 400</code></li>
	<li><code>1 &lt;= rowConditions.length, colConditions.length &lt;= 10<sup>4</sup></code></li>
	<li><code>rowConditions[i].length == colConditions[i].length == 2</code></li>
	<li><code>1 &lt;= above<sub>i</sub>, below<sub>i</sub>, left<sub>i</sub>, right<sub>i</sub> &lt;= k</code></li>
	<li><code>above<sub>i</sub> != below<sub>i</sub></code></li>
	<li><code>left<sub>i</sub> != right<sub>i</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi số từ $1..k$ xuất hiện đúng một lần; các điều kiện về hàng và cột là những ràng buộc về thứ tự. Vì $k \le 400$, ta biểu diễn chúng bằng các cạnh có hướng và thực hiện sắp xếp topo. Một chu trình có nghĩa là không thể tạo ma trận.
>
> Sắp xếp topo các hàng và cột một cách riêng biệt, sau đó đặt mỗi giá trị tại cặp chỉ số tương ứng. Nếu một trong hai thứ tự có độ dài nhỏ hơn $k$, trả về một ma trận rỗng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def buildMatrix(
        self, k: int, rowConditions: List[List[int]], colConditions: List[List[int]]
    ) -> List[List[int]]:
        def f(cond):
            g = defaultdict(list)
            indeg = [0] * (k + 1)
            for a, b in cond:
                g[a].append(b)
                indeg[b] += 1
            q = deque([i for i, v in enumerate(indeg[1:], 1) if v == 0])
            res = []
            while q:
                for _ in range(len(q)):
                    i = q.popleft()
                    res.append(i)
                    for j in g[i]:
                        indeg[j] -= 1
                        if indeg[j] == 0:
                            q.append(j)
            return None if len(res) != k else res

        row = f(rowConditions)
        col = f(colConditions)
        if row is None or col is None:
            return []
        ans = [[0] * k for _ in range(k)]
        m = [0] * (k + 1)
        for i, v in enumerate(col):
            m[v] = i
        for i, v in enumerate(row):
            ans[i][m[v]] = v
        return ans
```

#### Java

```java
class Solution {
    private int k;

    public int[][] buildMatrix(int k, int[][] rowConditions, int[][] colConditions) {
        this.k = k;
        List<Integer> row = f(rowConditions);
        List<Integer> col = f(colConditions);
        if (row == null || col == null) {
            return new int[0][0];
        }
        int[][] ans = new int[k][k];
        int[] m = new int[k + 1];
        for (int i = 0; i < k; ++i) {
            m[col.get(i)] = i;
        }
        for (int i = 0; i < k; ++i) {
            ans[i][m[row.get(i)]] = row.get(i);
        }
        return ans;
    }

    private List<Integer> f(int[][] cond) {
        List<Integer>[] g = new List[k + 1];
        Arrays.setAll(g, key -> new ArrayList<>());
        int[] indeg = new int[k + 1];
        for (var e : cond) {
            int a = e[0], b = e[1];
            g[a].add(b);
            ++indeg[b];
        }
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = 1; i < indeg.length; ++i) {
            if (indeg[i] == 0) {
                q.offer(i);
            }
        }
        List<Integer> res = new ArrayList<>();
        while (!q.isEmpty()) {
            for (int n = q.size(); n > 0; --n) {
                int i = q.pollFirst();
                res.add(i);
                for (int j : g[i]) {
                    if (--indeg[j] == 0) {
                        q.offer(j);
                    }
                }
            }
        }
        return res.size() == k ? res : null;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int k;

    vector<vector<int>> buildMatrix(int k, vector<vector<int>>& rowConditions, vector<vector<int>>& colConditions) {
        this->k = k;
        auto row = f(rowConditions);
        auto col = f(colConditions);
        if (row.empty() || col.empty()) return {};
        vector<vector<int>> ans(k, vector<int>(k));
        vector<int> m(k + 1);
        for (int i = 0; i < k; ++i) {
            m[col[i]] = i;
        }
        for (int i = 0; i < k; ++i) {
            ans[i][m[row[i]]] = row[i];
        }
        return ans;
    }

    vector<int> f(vector<vector<int>>& cond) {
        vector<vector<int>> g(k + 1);
        vector<int> indeg(k + 1);
        for (auto& e : cond) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            ++indeg[b];
        }
        queue<int> q;
        for (int i = 1; i < k + 1; ++i) {
            if (!indeg[i]) {
                q.push(i);
            }
        }
        vector<int> res;
        while (!q.empty()) {
            for (int n = q.size(); n; --n) {
                int i = q.front();
                res.push_back(i);
                q.pop();
                for (int j : g[i]) {
                    if (--indeg[j] == 0) {
                        q.push(j);
                    }
                }
            }
        }
        return res.size() == k ? res : vector<int>();
    }
};
```

#### Go

```go
func buildMatrix(k int, rowConditions [][]int, colConditions [][]int) [][]int {
	f := func(cond [][]int) []int {
		g := make([][]int, k+1)
		indeg := make([]int, k+1)
		for _, e := range cond {
			a, b := e[0], e[1]
			g[a] = append(g[a], b)
			indeg[b]++
		}
		q := []int{}
		for i, v := range indeg[1:] {
			if v == 0 {
				q = append(q, i+1)
			}
		}
		res := []int{}
		for len(q) > 0 {
			for n := len(q); n > 0; n-- {
				i := q[0]
				q = q[1:]
				res = append(res, i)
				for _, j := range g[i] {
					indeg[j]--
					if indeg[j] == 0 {
						q = append(q, j)
					}
				}
			}
		}
		if len(res) == k {
			return res
		}
		return []int{}
	}

	row := f(rowConditions)
	col := f(colConditions)
	if len(row) == 0 || len(col) == 0 {
		return [][]int{}
	}
	m := make([]int, k+1)
	for i, v := range col {
		m[v] = i
	}
	ans := make([][]int, k)
	for i := range ans {
		ans[i] = make([]int, k)
	}
	for i, v := range row {
		ans[i][m[v]] = v
	}
	return ans
}
```

#### TypeScript

```ts
function buildMatrix(k: number, rowConditions: number[][], colConditions: number[][]): number[][] {
    function f(cond) {
        const g = Array.from({ length: k + 1 }, () => []);
        const indeg = new Array(k + 1).fill(0);
        for (const [a, b] of cond) {
            g[a].push(b);
            ++indeg[b];
        }
        const q = [];
        for (let i = 1; i < indeg.length; ++i) {
            if (indeg[i] == 0) {
                q.push(i);
            }
        }
        const res = [];
        while (q.length) {
            for (let n = q.length; n; --n) {
                const i = q.shift();
                res.push(i);
                for (const j of g[i]) {
                    if (--indeg[j] == 0) {
                        q.push(j);
                    }
                }
            }
        }
        return res.length == k ? res : [];
    }

    const row = f(rowConditions);
    const col = f(colConditions);
    if (!row.length || !col.length) return [];
    const ans = Array.from({ length: k }, () => new Array(k).fill(0));
    const m = new Array(k + 1).fill(0);
    for (let i = 0; i < k; ++i) {
        m[col[i]] = i;
    }
    for (let i = 0; i < k; ++i) {
        ans[i][m[row[i]]] = row[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
