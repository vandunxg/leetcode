---
comments: true
difficulty: Medium
rating: 1710
source: Biweekly Contest 5 Q4
tags:
    - Graph
    - Topological Sort
    - Directed Acyclic Graph
---

<!-- problem:start -->

# [1136. Parallel Courses 🔒](https://leetcode.com/problems/parallel-courses)

[中文文档](/solution/1100-1199/1136.Parallel%20Courses/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, biểu thị có <code>n</code> khóa học được đánh số từ <code>1</code> đến <code>n</code>. Ngoài ra, cho mảng <code>relations</code>, trong đó <code>relations[i] = [prevCourse<sub>i</sub>, nextCourse<sub>i</sub>]</code> biểu thị quan hệ tiên quyết giữa khóa học <code>prevCourse<sub>i</sub></code> và khóa học <code>nextCourse<sub>i</sub></code>: phải học <code>prevCourse<sub>i</sub></code> trước <code>nextCourse<sub>i</sub></code>.</p>

<p>Trong một học kỳ, bạn có thể học <strong>bất kỳ số lượng</strong> khóa học nào, miễn là các môn tiên quyết của những khóa học đó đã được hoàn thành ở học kỳ <strong>trước</strong>.</p>

<p>Hãy trả về <em>số học kỳ <strong>ít nhất</strong> cần thiết để hoàn thành tất cả khóa học</em>. Nếu không thể hoàn thành tất cả khóa học, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1136.Parallel%20Courses/images/course1graph.jpg" style="width: 222px; height: 222px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, relations = [[1,3],[2,3]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hình trên biểu diễn đồ thị đã cho.
Ở học kỳ thứ nhất, bạn có thể học khóa 1 và 2.
Ở học kỳ thứ hai, bạn có thể học khóa 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1136.Parallel%20Courses/images/course2graph.jpg" style="width: 222px; height: 222px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, relations = [[1,2],[2,3],[3,1]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể học khóa nào vì chúng là môn tiên quyết của nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5000</code></li>
	<li><code>1 &lt;= relations.length &lt;= 5000</code></li>
	<li><code>relations[i].length == 2</code></li>
	<li><code>1 &lt;= prevCourse<sub>i</sub>, nextCourse<sub>i</sub> &lt;= n</code></li>
	<li><code>prevCourse<sub>i</sub> != nextCourse<sub>i</sub></code></li>
	<li>Tất cả các cặp <code>[prevCourse<sub>i</sub>, nextCourse<sub>i</sub>]</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Topological Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Số học kỳ cần thiết bằng độ dài chuỗi môn tiên quyết dài nhất; trong mỗi học kỳ, có thể học mọi khóa hiện có indegree bằng $0$. Dùng thuật toán topo của Kahn theo từng lớp, mỗi lớp tương ứng một học kỳ. Nếu còn khóa học chưa xử lý thì đồ thị có chu trình, nên trả về $-1$.

<!-- thinking:end -->

Trước tiên, ta xây dựng đồ thị $g$ biểu diễn quan hệ tiên quyết giữa các khóa học, đồng thời đếm bậc vào $indeg$ của từng khóa.

Sau đó, ta đưa các khóa có bậc vào bằng $0$ vào queue và bắt đầu sắp xếp topo. Mỗi lần, lấy một khóa khỏi queue, giảm bậc vào của các khóa mà nó trỏ tới đi $1$; nếu bậc vào giảm về $0$, đưa khóa đó vào queue. Khi queue rỗng, nếu vẫn còn khóa chưa hoàn thành thì không thể hoàn thành tất cả khóa học, vì vậy trả về $-1$. Ngược lại, trả về số học kỳ cần thiết để hoàn thành tất cả khóa.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(n + m)$. Trong đó, $n$ và $m$ lần lượt là số khóa học và số quan hệ tiên quyết.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSemesters(self, n: int, relations: List[List[int]]) -> int:
        g = defaultdict(list)
        indeg = [0] * n
        for prev, nxt in relations:
            prev, nxt = prev - 1, nxt - 1
            g[prev].append(nxt)
            indeg[nxt] += 1
        q = deque(i for i, v in enumerate(indeg) if v == 0)
        ans = 0
        while q:
            ans += 1
            for _ in range(len(q)):
                i = q.popleft()
                n -= 1
                for j in g[i]:
                    indeg[j] -= 1
                    if indeg[j] == 0:
                        q.append(j)
        return -1 if n else ans
```

#### Java

```java
class Solution {
    public int minimumSemesters(int n, int[][] relations) {
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        int[] indeg = new int[n];
        for (var r : relations) {
            int prev = r[0] - 1, nxt = r[1] - 1;
            g[prev].add(nxt);
            ++indeg[nxt];
        }
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            if (indeg[i] == 0) {
                q.offer(i);
            }
        }
        int ans = 0;
        while (!q.isEmpty()) {
            ++ans;
            for (int k = q.size(); k > 0; --k) {
                int i = q.poll();
                --n;
                for (int j : g[i]) {
                    if (--indeg[j] == 0) {
                        q.offer(j);
                    }
                }
            }
        }
        return n == 0 ? ans : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSemesters(int n, vector<vector<int>>& relations) {
        vector<vector<int>> g(n);
        vector<int> indeg(n);
        for (auto& r : relations) {
            int prev = r[0] - 1, nxt = r[1] - 1;
            g[prev].push_back(nxt);
            ++indeg[nxt];
        }
        queue<int> q;
        for (int i = 0; i < n; ++i) {
            if (indeg[i] == 0) {
                q.push(i);
            }
        }
        int ans = 0;
        while (!q.empty()) {
            ++ans;
            for (int k = q.size(); k; --k) {
                int i = q.front();
                q.pop();
                --n;
                for (int& j : g[i]) {
                    if (--indeg[j] == 0) {
                        q.push(j);
                    }
                }
            }
        }
        return n == 0 ? ans : -1;
    }
};
```

#### Go

```go
func minimumSemesters(n int, relations [][]int) (ans int) {
	g := make([][]int, n)
	indeg := make([]int, n)
	for _, r := range relations {
		prev, nxt := r[0]-1, r[1]-1
		g[prev] = append(g[prev], nxt)
		indeg[nxt]++
	}
	q := []int{}
	for i, v := range indeg {
		if v == 0 {
			q = append(q, i)
		}
	}
	for len(q) > 0 {
		ans++
		for k := len(q); k > 0; k-- {
			i := q[0]
			q = q[1:]
			n--
			for _, j := range g[i] {
				indeg[j]--
				if indeg[j] == 0 {
					q = append(q, j)
				}
			}
		}
	}
	if n == 0 {
		return
	}
	return -1
}
```

#### TypeScript

```ts
function minimumSemesters(n: number, relations: number[][]): number {
    const g: number[][] = Array.from({ length: n }, () => []);
    const indeg = new Array(n).fill(0);
    for (const [prev, nxt] of relations) {
        g[prev - 1].push(nxt - 1);
        indeg[nxt - 1]++;
    }
    const q: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (indeg[i] === 0) {
            q.push(i);
        }
    }
    let ans = 0;
    while (q.length) {
        ++ans;
        for (let k = q.length; k; --k) {
            const i = q.shift()!;
            --n;
            for (const j of g[i]) {
                if (--indeg[j] === 0) {
                    q.push(j);
                }
            }
        }
    }
    return n === 0 ? ans : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
