---
comments: true
difficulty: Hard
rating: 2300
source: Weekly Contest 220 Q4
tags:
    - Union Find
    - Graph
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [1697. Checking Existence of Edge Length Limited Paths](https://leetcode.com/problems/checking-existence-of-edge-length-limited-paths)

[中文文档](/solution/1600-1699/1697.Checking%20Existence%20of%20Edge%20Length%20Limited%20Paths/README.md)

## Mô tả

<!-- description:start -->

<p>Một đồ thị vô hướng gồm <code>n</code> nút được mô tả bởi <code>edgeList</code>, trong đó <code>edgeList[i] = [u<sub>i</sub>, v<sub>i</sub>, dis<sub>i</sub>]</code> biểu diễn một cạnh giữa hai nút <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code> có độ dài <code>dis<sub>i</sub></code>. Lưu ý rằng có thể có <strong>nhiều</strong> cạnh giữa hai nút.</p>

<p>Cho một mảng <code>queries</code>, trong đó <code>queries[j] = [p<sub>j</sub>, q<sub>j</sub>, limit<sub>j</sub>]</code>, nhiệm vụ của bạn là xác định với mỗi <code>queries[j]</code> liệu có đường đi giữa <code>p<sub>j</sub></code> và <code>q<sub>j</sub></code><sub> </sub>sao cho độ dài của mỗi cạnh trên đường đi <strong>nhỏ hơn nghiêm ngặt</strong> <code>limit<sub>j</sub></code> hay không.</p>

<p>Trả về <em>một <strong>mảng boolean</strong> </em><code>answer</code><em>, trong đó </em><code>answer.length == queries.length</code><em> và giá trị thứ </em><code>j<sup>th</sup></code><em> của </em><code>answer</code><em> là </em><code>true</code><em> nếu có đường đi cho </em><code>queries[j]</code><em> là </em><code>true</code><em>, và là </em><code>false</code><em> nếu ngược lại</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1697.Checking%20Existence%20of%20Edge%20Length%20Limited%20Paths/images/h.png" style="width: 267px; height: 262px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, edgeList = [[0,1,2],[1,2,4],[2,0,8],[1,0,16]], queries = [[0,1,2],[0,2,5]]
<strong>Đầu ra:</strong> [false,true]
<strong>Giải thích:</strong> Hình trên minh họa đồ thị đã cho. Lưu ý rằng có hai cạnh song song giữa 0 và 1 với độ dài lần lượt là 2 và 16.
Với truy vấn thứ nhất, không có đường đi giữa 0 và 1 trong đó mọi độ dài đều nhỏ hơn 2, nên ta trả về false.
Với truy vấn thứ hai, có đường đi (0 -&gt; 1 -&gt; 2) gồm hai cạnh có độ dài nhỏ hơn 5, nên ta trả về true.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1697.Checking%20Existence%20of%20Edge%20Length%20Limited%20Paths/images/q.png" style="width: 390px; height: 358px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, edgeList = [[0,1,10],[1,2,5],[2,3,9],[3,4,13]], queries = [[0,4,14],[1,4,13]]
<strong>Đầu ra:</strong> [true,false]
<strong>Giải thích:</strong> Hình trên minh họa đồ thị đã cho.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= edgeList.length, queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>edgeList[i].length == 3</code></li>
	<li><code>queries[j].length == 3</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub>, p<sub>j</sub>, q<sub>j</sub> &lt;= n - 1</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
	<li><code>p<sub>j</sub> != q<sub>j</sub></code></li>
	<li><code>1 &lt;= dis<sub>i</sub>, limit<sub>j</sub> &lt;= 10<sup>9</sup></code></li>
	<li>Có thể có <strong>nhiều</strong> cạnh giữa hai nút.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Truy vấn offline + Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn hỏi liệu hai đỉnh có được nối bởi một đường đi mà mọi cạnh đều nhỏ hơn nghiêm ngặt $\textit{limit}$ hay không. Xây dựng lại đồ thị cho từng truy vấn quá chậm; các truy vấn độc lập nên ta xử lý offline.
>
> Sắp xếp các cạnh và truy vấn theo trọng số/limit. Union-Find cùng một con trỏ sẽ thêm mọi cạnh nhẹ hơn $\textit{limit}$ hiện tại, rồi kiểm tra hai đỉnh có chung gốc hay không.
>
> Ghi nhớ chỉ số ban đầu của truy vấn khi điền vào mảng đáp án.

<!-- thinking:end -->

Theo yêu cầu bài toán, ta cần đánh giá từng truy vấn $queries[i]$, tức là xác định liệu có đường đi với trọng số cạnh nhỏ hơn hoặc bằng $limit$ giữa hai điểm $a$ và $b$ của truy vấn hiện tại hay không.

Tính liên thông của hai điểm có thể được xác định bằng Union-Find. Ngoài ra, vì thứ tự truy vấn không ảnh hưởng đến kết quả, ta có thể sắp xếp các truy vấn tăng dần theo $limit$, đồng thời sắp xếp các cạnh tăng dần theo trọng số.

Sau đó, với mỗi truy vấn, ta bắt đầu từ cạnh có trọng số nhỏ nhất, thêm tất cả các cạnh có trọng số nhỏ hơn $limit$ vào Union-Find, rồi dùng thao tác truy vấn của Union-Find để xác định hai điểm có liên thông hay không.

Độ phức tạp thời gian là $O(m \times \log m + q \times \log q)$, trong đó $m$ và $q$ lần lượt là số lượng cạnh và truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distanceLimitedPathsExist(
        self, n: int, edgeList: List[List[int]], queries: List[List[int]]
    ) -> List[bool]:
        def find(x):
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        p = list(range(n))
        edgeList.sort(key=lambda x: x[2])
        j = 0
        ans = [False] * len(queries)
        for i, (a, b, limit) in sorted(enumerate(queries), key=lambda x: x[1][2]):
            while j < len(edgeList) and edgeList[j][2] < limit:
                u, v, _ = edgeList[j]
                p[find(u)] = find(v)
                j += 1
            ans[i] = find(a) == find(b)
        return ans
```

#### Java

```java
class Solution {
    private int[] p;

    public boolean[] distanceLimitedPathsExist(int n, int[][] edgeList, int[][] queries) {
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        Arrays.sort(edgeList, (a, b) -> a[2] - b[2]);
        int m = queries.length;
        boolean[] ans = new boolean[m];
        Integer[] qid = new Integer[m];
        for (int i = 0; i < m; ++i) {
            qid[i] = i;
        }
        Arrays.sort(qid, (i, j) -> queries[i][2] - queries[j][2]);
        int j = 0;
        for (int i : qid) {
            int a = queries[i][0], b = queries[i][1], limit = queries[i][2];
            while (j < edgeList.length && edgeList[j][2] < limit) {
                int u = edgeList[j][0], v = edgeList[j][1];
                p[find(u)] = find(v);
                ++j;
            }
            ans[i] = find(a) == find(b);
        }
        return ans;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<bool> distanceLimitedPathsExist(int n, vector<vector<int>>& edgeList, vector<vector<int>>& queries) {
        vector<int> p(n);
        iota(p.begin(), p.end(), 0);
        sort(edgeList.begin(), edgeList.end(), [](auto& a, auto& b) { return a[2] < b[2]; });
        function<int(int)> find = [&](int x) -> int {
            if (p[x] != x) p[x] = find(p[x]);
            return p[x];
        };
        int m = queries.size();
        vector<bool> ans(m);
        vector<int> qid(m);
        iota(qid.begin(), qid.end(), 0);
        sort(qid.begin(), qid.end(), [&](int i, int j) { return queries[i][2] < queries[j][2]; });
        int j = 0;
        for (int i : qid) {
            int a = queries[i][0], b = queries[i][1], limit = queries[i][2];
            while (j < edgeList.size() && edgeList[j][2] < limit) {
                int u = edgeList[j][0], v = edgeList[j][1];
                p[find(u)] = find(v);
                ++j;
            }
            ans[i] = find(a) == find(b);
        }
        return ans;
    }
};
```

#### Go

```go
func distanceLimitedPathsExist(n int, edgeList [][]int, queries [][]int) []bool {
	p := make([]int, n)
	for i := range p {
		p[i] = i
	}
	sort.Slice(edgeList, func(i, j int) bool { return edgeList[i][2] < edgeList[j][2] })
	var find func(int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	m := len(queries)
	qid := make([]int, m)
	ans := make([]bool, m)
	for i := range qid {
		qid[i] = i
	}
	sort.Slice(qid, func(i, j int) bool { return queries[qid[i]][2] < queries[qid[j]][2] })
	j := 0
	for _, i := range qid {
		a, b, limit := queries[i][0], queries[i][1], queries[i][2]
		for j < len(edgeList) && edgeList[j][2] < limit {
			u, v := edgeList[j][0], edgeList[j][1]
			p[find(u)] = find(v)
			j++
		}
		ans[i] = find(a) == find(b)
	}
	return ans
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn distance_limited_paths_exist(
        n: i32,
        edge_list: Vec<Vec<i32>>,
        queries: Vec<Vec<i32>>,
    ) -> Vec<bool> {
        let mut disjoint_set: Vec<usize> = vec![0; n as usize];
        let mut ans_vec: Vec<bool> = vec![false; queries.len()];
        let mut q_vec: Vec<usize> = vec![0; queries.len()];

        // Initialize the set
        for i in 0..n {
            disjoint_set[i as usize] = i as usize;
        }

        // Initialize the q_vec
        for i in 0..queries.len() {
            q_vec[i] = i;
        }

        // Sort the q_vec based on the query limit, from the lowest to highest
        q_vec.sort_by(|i, j| queries[*i][2].cmp(&queries[*j][2]));

        // Sort the edge_list based on the edge weight, from the lowest to highest
        let mut edge_list = edge_list.clone();
        edge_list.sort_by(|i, j| i[2].cmp(&j[2]));

        let mut edge_idx: usize = 0;
        for q_idx in &q_vec {
            let s = queries[*q_idx][0] as usize;
            let d = queries[*q_idx][1] as usize;
            let limit = queries[*q_idx][2];
            // Construct the disjoint set
            while edge_idx < edge_list.len() && edge_list[edge_idx][2] < limit {
                Solution::union(
                    edge_list[edge_idx][0] as usize,
                    edge_list[edge_idx][1] as usize,
                    &mut disjoint_set,
                );
                edge_idx += 1;
            }
            // If the parents of s & d are the same, this query should be `true`
            // Otherwise, the current query is `false`
            ans_vec[*q_idx] = Solution::check_valid(s, d, &mut disjoint_set);
        }

        ans_vec
    }

    #[allow(dead_code)]
    pub fn find(x: usize, d_set: &mut Vec<usize>) -> usize {
        if d_set[x] != x {
            d_set[x] = Solution::find(d_set[x], d_set);
        }
        return d_set[x];
    }

    #[allow(dead_code)]
    pub fn union(s: usize, d: usize, d_set: &mut Vec<usize>) {
        let p_s = Solution::find(s, d_set);
        let p_d = Solution::find(d, d_set);
        d_set[p_s] = p_d;
    }

    #[allow(dead_code)]
    pub fn check_valid(s: usize, d: usize, d_set: &mut Vec<usize>) -> bool {
        let p_s = Solution::find(s, d_set);
        let p_d = Solution::find(d, d_set);
        p_s == p_d
    }
}
```

<!-- tabs:end -->

Union-Find là một cấu trúc dữ liệu dạng cây, được dùng để xử lý các bài toán **gộp** và **truy vấn** tập hợp rời nhau. Nó hỗ trợ hai thao tác:

1. Find: Xác định một phần tử thuộc tập con nào. Độ phức tạp thời gian của một thao tác là $O(\alpha(n))$.
2. Union: Gộp hai tập con thành một tập hợp. Độ phức tạp thời gian của một thao tác là $O(\alpha(n))$.

Ở đây, $\alpha$ là hàm Ackermann ngược, tăng cực kỳ chậm. Nói cách khác, thời gian chạy trung bình của một thao tác có thể xem như một hằng số rất nhỏ.

Dưới đây là một mẫu Union-Find thông dụng cần nắm vững. Trong đó:

- `n` biểu diễn số lượng node.
- `p` lưu node cha của mỗi điểm. Ban đầu, node cha của mỗi điểm là chính nó.
- `size` chỉ có ý nghĩa khi node là node tổ tiên, cho biết số lượng điểm trong tập hợp chứa node tổ tiên đó.
- Hàm `find(x)` dùng để tìm node tổ tiên của tập hợp chứa $x$.
- Hàm `union(a, b)` dùng để gộp các tập hợp chứa $a$ và $b$.

<!-- tabs:start -->

#### Python3

```python
p = list(range(n))
size = [1] * n

def find(x):
    if p[x] != x:
        p[x] = find(p[x])
    return p[x]


def union(a, b):
    pa, pb = find(a), find(b)
    if pa == pb:
        return
    p[pa] = pb
    size[pb] += size[pa]
```

#### Java

```java
int[] p = new int[n];
int[] size = new int[n];
for (int i = 0; i < n; ++i) {
    p[i] = i;
    size[i] = 1;
}

int find(int x) {
    if (p[x] != x) {
        p[x] = find(p[x]);
    }
    return p[x];
}

void union(int a, int b) {
    int pa = find(a), pb = find(b);
    if (pa == pb) {
        return;
    }
    p[pa] = pb;
    size[pb] += size[pa];
}
```

#### C++

```cpp
vector<int> p(n);
iota(p.begin(), p.end(), 0);
vector<int> size(n, 1);

int find(int x) {
    if (p[x] != x) {
        p[x] = find(p[x]);
    }
    return p[x];
}

void unite(int a, int b) {
    int pa = find(a), pb = find(b);
    if (pa == pb) return;
    p[pa] = pb;
    size[pb] += size[pa];
}
```

#### Go

```go
p := make([]int, n)
size := make([]int, n)
for i := range p {
    p[i] = i
    size[i] = 1
}

func find(x int) int {
    if p[x] != x {
        p[x] = find(p[x])
    }
    return p[x]
}

func union(a, b int) {
    pa, pb := find(a), find(b)
    if pa == pb {
        return
    }
    p[pa] = pb
    size[pb] += size[pa]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
