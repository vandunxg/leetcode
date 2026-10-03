---
comments: true
difficulty: Hard
rating: 2415
source: Weekly Contest 322 Q4
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
---

<!-- problem:start -->

# [2493. Divide Nodes Into the Maximum Number of Groups](https://leetcode.com/problems/divide-nodes-into-the-maximum-number-of-groups)

[中文文档](/solution/2400-2499/2493.Divide%20Nodes%20Into%20the%20Maximum%20Number%20of%20Groups/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>n</code> biểu diễn số lượng đỉnh của một đồ thị <strong>vô hướng</strong>. Các đỉnh được đánh số từ <code>1</code> đến <code>n</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>edges</code>, trong đó <code>edges[i] = [a<sub>i, </sub>b<sub>i</sub>]</code> biểu diễn có một cạnh <strong>hai chiều</strong> giữa các đỉnh <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code>. <strong>Lưu ý</strong> rằng đồ thị đã cho có thể không liên thông.</p>

<p>Chia các đỉnh của đồ thị thành <code>m</code> nhóm (được <strong>đánh số từ 1</strong>) sao cho:</p>

<ul>
	<li>Mỗi đỉnh trong đồ thị thuộc chính xác một nhóm.</li>
	<li>Với mọi cặp đỉnh trong đồ thị được nối bởi một cạnh <code>[a<sub>i, </sub>b<sub>i</sub>]</code>, nếu <code>a<sub>i</sub></code> thuộc nhóm có chỉ số <code>x</code>, còn <code>b<sub>i</sub></code> thuộc nhóm có chỉ số <code>y</code>, thì <code>|y - x| = 1</code>.</li>
</ul>

<p>Trả về <em>số lượng nhóm lớn nhất (tức là </em><code>m</code><em> lớn nhất) mà bạn có thể chia các đỉnh vào đó</em>. Trả về <code>-1</code> <em>nếu không thể chia các đỉnh thành các nhóm thỏa mãn các điều kiện đã cho</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2493.Divide%20Nodes%20Into%20the%20Maximum%20Number%20of%20Groups/images/example1.png" style="width: 352px; height: 201px;" />
<pre>
<strong>Đầu vào:</strong> n = 6, edges = [[1,2],[1,4],[1,5],[2,6],[2,3],[4,6]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Như trong hình, ta:
- Thêm đỉnh 5 vào nhóm thứ nhất.
- Thêm đỉnh 1 vào nhóm thứ hai.
- Thêm các đỉnh 2 và 4 vào nhóm thứ ba.
- Thêm các đỉnh 3 và 6 vào nhóm thứ tư.
Ta có thể thấy mọi cạnh đều thỏa mãn điều kiện.
Có thể chứng minh rằng nếu tạo nhóm thứ năm và chuyển bất kỳ đỉnh nào từ nhóm thứ ba hoặc thứ tư vào đó, thì ít nhất một cạnh sẽ không thỏa mãn điều kiện.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, edges = [[1,2],[2,3],[3,1]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Nếu thêm đỉnh 1 vào nhóm thứ nhất, đỉnh 2 vào nhóm thứ hai và đỉnh 3 vào nhóm thứ ba để thỏa mãn hai cạnh đầu tiên, ta thấy cạnh thứ ba sẽ không thỏa mãn điều kiện.
Có thể chứng minh rằng không tồn tại cách chia nhóm nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 500</code></li>
	<li><code>1 &lt;= edges.length &lt;= 10<sup>4</sup></code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>1 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>Giữa mỗi cặp đỉnh có nhiều nhất một cạnh.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ số nhóm của hai đỉnh kề nhau phải chênh lệch $1$, vì vậy đồ thị phải là đồ thị hai phía và cách chia tốt nhất của một thành phần là độ sâu BFS lớn nhất của thành phần đó. Với $n\le 500$, ta chạy BFS từ mọi đỉnh bắt đầu: nếu chênh lệch khoảng cách khác $1$ thì cách chia không hợp lệ. Ta dùng đỉnh có chỉ số nhỏ nhất làm gốc của thành phần, lưu độ sâu lớn nhất tìm được và cộng kết quả theo các gốc.

<!-- thinking:end -->

Vì đồ thị do đề bài cho có thể không liên thông, ta cần xử lý từng thành phần liên thông, tìm số nhóm lớn nhất trong mỗi thành phần liên thông rồi cộng lại để có kết quả cuối cùng.

Ta có thể lần lượt chọn mỗi đỉnh làm đỉnh của nhóm thứ nhất, sau đó dùng BFS để duyệt toàn bộ thành phần liên thông và dùng một mảng $d$ để ghi nhận số nhóm lớn nhất trong mỗi thành phần liên thông. Trong phần cài đặt, ta dùng đỉnh nhỏ nhất trong thành phần liên thông làm đỉnh gốc của thành phần đó.

Trong quá trình BFS, ta dùng một hàng đợi $q$ để lưu các đỉnh đang được duyệt, dùng một mảng $dist$ để ghi nhận khoảng cách từ mỗi đỉnh đến đỉnh bắt đầu, dùng biến $mx$ để ghi nhận độ sâu lớn nhất của thành phần liên thông hiện tại và dùng biến $root$ để ghi nhận đỉnh gốc của thành phần liên thông hiện tại.

Trong quá trình duyệt, nếu thấy $dist[b]$ của một đỉnh $b$ bằng $0$, điều đó có nghĩa là $b$ chưa được duyệt. Ta đặt khoảng cách của $b$ là $dist[a] + 1$, cập nhật $mx$ rồi thêm $b$ vào hàng đợi $q$. Nếu khoảng cách của $b$ đã được cập nhật, ta kiểm tra xem khoảng cách giữa $b$ và $a$ có bằng $1$ hay không. Nếu không, nghĩa là không thể đáp ứng các yêu cầu của đề bài, nên ta trả về $-1$ ngay lập tức.

Độ phức tạp thời gian là $O(n \times (n + m))$, độ phức tạp không gian là $O(n + m)$. Trong đó, $n$ và $m$ lần lượt là số đỉnh và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def magnificentSets(self, n: int, edges: List[List[int]]) -> int:
        g = [[] for _ in range(n)]
        for a, b in edges:
            g[a - 1].append(b - 1)
            g[b - 1].append(a - 1)
        d = defaultdict(int)
        for i in range(n):
            q = deque([i])
            dist = [0] * n
            dist[i] = mx = 1
            root = i
            while q:
                a = q.popleft()
                root = min(root, a)
                for b in g[a]:
                    if dist[b] == 0:
                        dist[b] = dist[a] + 1
                        mx = max(mx, dist[b])
                        q.append(b)
                    elif abs(dist[b] - dist[a]) != 1:
                        return -1
            d[root] = max(d[root], mx)
        return sum(d.values())
```

#### Java

```java
class Solution {
    public int magnificentSets(int n, int[][] edges) {
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0] - 1, b = e[1] - 1;
            g[a].add(b);
            g[b].add(a);
        }
        int[] d = new int[n];
        int[] dist = new int[n];
        for (int i = 0; i < n; ++i) {
            Deque<Integer> q = new ArrayDeque<>();
            q.offer(i);
            Arrays.fill(dist, 0);
            dist[i] = 1;
            int mx = 1;
            int root = i;
            while (!q.isEmpty()) {
                int a = q.poll();
                root = Math.min(root, a);
                for (int b : g[a]) {
                    if (dist[b] == 0) {
                        dist[b] = dist[a] + 1;
                        mx = Math.max(mx, dist[b]);
                        q.offer(b);
                    } else if (Math.abs(dist[b] - dist[a]) != 1) {
                        return -1;
                    }
                }
            }
            d[root] = Math.max(d[root], mx);
        }
        return Arrays.stream(d).sum();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int magnificentSets(int n, vector<vector<int>>& edges) {
        vector<int> g[n];
        for (auto& e : edges) {
            int a = e[0] - 1, b = e[1] - 1;
            g[a].push_back(b);
            g[b].push_back(a);
        }
        vector<int> d(n);
        for (int i = 0; i < n; ++i) {
            queue<int> q{{i}};
            vector<int> dist(n);
            dist[i] = 1;
            int mx = 1;
            int root = i;
            while (q.size()) {
                int a = q.front();
                q.pop();
                root = min(root, a);
                for (int b : g[a]) {
                    if (dist[b] == 0) {
                        dist[b] = dist[a] + 1;
                        mx = max(mx, dist[b]);
                        q.push(b);
                    } else if (abs(dist[b] - dist[a]) != 1) {
                        return -1;
                    }
                }
            }
            d[root] = max(d[root], mx);
        }
        return accumulate(d.begin(), d.end(), 0);
    }
};
```

#### Go

```go
func magnificentSets(n int, edges [][]int) (ans int) {
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0]-1, e[1]-1
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	d := make([]int, n)
	for i := range d {
		q := []int{i}
		dist := make([]int, n)
		dist[i] = 1
		mx := 1
		root := i
		for len(q) > 0 {
			a := q[0]
			q = q[1:]
			root = min(root, a)
			for _, b := range g[a] {
				if dist[b] == 0 {
					dist[b] = dist[a] + 1
					mx = max(mx, dist[b])
					q = append(q, b)
				} else if abs(dist[b]-dist[a]) != 1 {
					return -1
				}
			}
		}
		d[root] = max(d[root], mx)
	}
	for _, x := range d {
		ans += x
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {number}
 */
var magnificentSets = function (n, edges) {
    const g = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a - 1].push(b - 1);
        g[b - 1].push(a - 1);
    }
    const d = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        const q = [i];
        const dist = Array(n).fill(0);
        dist[i] = 1;
        let mx = 1;
        let root = i;
        while (q.length) {
            const a = q.shift();
            root = Math.min(root, a);
            for (const b of g[a]) {
                if (dist[b] === 0) {
                    dist[b] = dist[a] + 1;
                    mx = Math.max(mx, dist[b]);
                    q.push(b);
                } else if (Math.abs(dist[b] - dist[a]) !== 1) {
                    return -1;
                }
            }
        }
        d[root] = Math.max(d[root], mx);
    }
    return d.reduce((a, b) => a + b);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
