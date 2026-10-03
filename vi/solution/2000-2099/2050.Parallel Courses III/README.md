---
comments: true
difficulty: Hard
rating: 2084
source: Weekly Contest 264 Q4
tags:
    - Graph
    - Topological Sort
    - Array
    - Dynamic Programming
    - Directed Acyclic Graph
---

<!-- problem:start -->

# [2050. Parallel Courses III](https://leetcode.com/problems/parallel-courses-iii)

[中文文档](/solution/2000-2099/2050.Parallel%20Courses%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>, biểu thị có <code>n</code> khóa học được đánh số từ <code>1</code> đến <code>n</code>. Bạn cũng được cho một mảng số nguyên 2 chiều <code>relations</code>, trong đó <code>relations[j] = [prevCourse<sub>j</sub>, nextCourse<sub>j</sub>]</code> biểu thị khóa học <code>prevCourse<sub>j</sub></code> phải được hoàn thành <strong>trước</strong> khóa học <code>nextCourse<sub>j</sub></code> (mối quan hệ tiên quyết). Ngoài ra, bạn được cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>time</code>, trong đó <code>time[i]</code> biểu thị cần bao nhiêu <strong>tháng</strong> để hoàn thành khóa học thứ <code>(i+1)<sup>th</sup></code>.</p>

<p>Bạn cần tìm <strong>số tháng nhỏ nhất</strong> để hoàn thành tất cả khóa học theo các quy tắc sau:</p>

<ul>
	<li>Bạn có thể bắt đầu học một khóa học <strong>bất kỳ lúc nào</strong> nếu đã đáp ứng các điều kiện tiên quyết.</li>
	<li>Có thể học <strong>bất kỳ số lượng khóa học nào</strong> <strong>đồng thời</strong>.</li>
</ul>

<p>Hãy trả về <em><strong>số tháng nhỏ nhất cần thiết để hoàn thành tất cả khóa học</strong></em>.</p>

<p><strong>Lưu ý:</strong> Các test case được tạo sao cho có thể hoàn thành mọi khóa học (tức là đồ thị là đồ thị có hướng không chu trình).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2050.Parallel%20Courses%20III/images/ex1.png" style="width: 392px; height: 232px;" /></strong>

<pre>
<strong>Đầu vào:</strong> n = 3, relations = [[1,3],[2,3]], time = [3,2,5]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Hình trên biểu diễn đồ thị đã cho và thời gian cần thiết để hoàn thành mỗi khóa học.
Ta bắt đầu khóa học 1 và khóa học 2 cùng lúc tại tháng 0.
Khóa học 1 và khóa học 2 lần lượt mất 3 tháng và 2 tháng để hoàn thành.
Do đó, thời điểm sớm nhất có thể bắt đầu khóa học 3 là tháng 3, và tổng thời gian cần thiết là 3 + 5 = 8 tháng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2050.Parallel%20Courses%20III/images/ex2.png" style="width: 500px; height: 365px;" /></strong>

<pre>
<strong>Đầu vào:</strong> n = 5, relations = [[1,5],[2,5],[3,5],[3,4],[4,5]], time = [1,2,3,4,5]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Hình trên biểu diễn đồ thị đã cho và thời gian cần thiết để hoàn thành mỗi khóa học.
Bạn có thể bắt đầu các khóa học 1, 2 và 3 tại tháng 0.
Bạn có thể hoàn thành chúng lần lượt sau 1, 2 và 3 tháng.
Chỉ có thể học khóa học 4 sau khi hoàn thành khóa học 3, tức là sau 3 tháng. Khóa học 4 hoàn thành sau 3 + 4 = 7 tháng.
Chỉ có thể học khóa học 5 sau khi hoàn thành các khóa học 1, 2, 3 và 4, tức là sau max(1,2,3,7) = 7 tháng.
Vì vậy, thời gian nhỏ nhất cần thiết để hoàn thành tất cả khóa học là 7 + 5 = 12 tháng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= relations.length &lt;= min(n * (n - 1) / 2, 5 * 10<sup>4</sup>)</code></li>
	<li><code>relations[j].length == 2</code></li>
	<li><code>1 &lt;= prevCourse<sub>j</sub>, nextCourse<sub>j</sub> &lt;= n</code></li>
	<li><code>prevCourse<sub>j</sub> != nextCourse<sub>j</sub></code></li>
	<li>Tất cả các cặp <code>[prevCourse<sub>j</sub>, nextCourse<sub>j</sub>]</code> đều <strong>khác nhau</strong>.</li>
	<li><code>time.length == n</code></li>
	<li><code>1 &lt;= time[i] &lt;= 10<sup>4</sup></code></li>
	<li>Đồ thị đã cho là đồ thị có hướng không chu trình.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp topo + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Các khóa học có điều kiện tiên quyết và thời lượng; thời điểm hoàn thành là độ dài của đường đi nặng nhất trong DAG. Với $n \le 5 \times 10^4$, ta không thể liệt kê các đường đi. Các đỉnh nguồn bắt đầu ngay và hoàn thành sau `time` của chính chúng.
>
> Theo thứ tự topo, khi mọi đỉnh trước $i$ đã hoàn thành, ta cập nhật $f[j] = \max(f[j], f[i]+time[j])$. Đáp án là giá trị lớn nhất của $f$.
>
> Queue chứa các đỉnh có bậc vào bằng 0 và chỉ cập nhật một khóa học sau khi tất cả điều kiện tiên quyết của khóa học đó đã được xử lý.

<!-- thinking:end -->

Trước tiên, ta xây dựng một đồ thị có hướng không chu trình dựa trên các quan hệ tiên quyết đã cho, thực hiện sắp xếp topo trên đồ thị này, sau đó dùng quy hoạch động để tìm thời gian nhỏ nhất cần thiết để hoàn thành tất cả khóa học dựa trên kết quả sắp xếp topo.

Ta định nghĩa các cấu trúc dữ liệu hoặc biến sau:

- Danh sách kề $g$ lưu đồ thị có hướng không chu trình, còn mảng $indeg$ lưu bậc vào của mỗi đỉnh;
- Queue $q$ lưu tất cả các đỉnh có bậc vào bằng $0$;
- Mảng $f$ lưu thời điểm hoàn thành sớm nhất của mỗi đỉnh, ban đầu $f[i] = 0$;
- Biến $ans$ lưu đáp án cuối cùng, ban đầu $ans = 0$;

Khi $q$ không rỗng, lần lượt lấy đỉnh đầu $i$ ra, duyệt từng đỉnh $j$ trong $g[i]$, cập nhật $f[j] = \max(f[j], f[i] + time[j])$, đồng thời cập nhật $ans = \max(ans, f[j])$ và giảm bậc vào của $j$ đi $1$. Nếu lúc này bậc vào của $j$ bằng $0$, thêm $j$ vào queue $q$;

Cuối cùng, trả về $ans$.

Độ phức tạp thời gian là $O(m + n)$, và độ phức tạp không gian là $O(m + n)$. Trong đó, $m$ là độ dài của mảng $relations$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTime(self, n: int, relations: List[List[int]], time: List[int]) -> int:
        g = defaultdict(list)
        indeg = [0] * n
        for a, b in relations:
            g[a - 1].append(b - 1)
            indeg[b - 1] += 1
        q = deque()
        f = [0] * n
        ans = 0
        for i, (v, t) in enumerate(zip(indeg, time)):
            if v == 0:
                q.append(i)
                f[i] = t
                ans = max(ans, t)
        while q:
            i = q.popleft()
            for j in g[i]:
                f[j] = max(f[j], f[i] + time[j])
                ans = max(ans, f[j])
                indeg[j] -= 1
                if indeg[j] == 0:
                    q.append(j)
        return ans
```

#### Java

```java
class Solution {
    public int minimumTime(int n, int[][] relations, int[] time) {
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        int[] indeg = new int[n];
        for (int[] e : relations) {
            int a = e[0] - 1, b = e[1] - 1;
            g[a].add(b);
            ++indeg[b];
        }
        Deque<Integer> q = new ArrayDeque<>();
        int[] f = new int[n];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int v = indeg[i], t = time[i];
            if (v == 0) {
                q.offer(i);
                f[i] = t;
                ans = Math.max(ans, t);
            }
        }
        while (!q.isEmpty()) {
            int i = q.pollFirst();
            for (int j : g[i]) {
                f[j] = Math.max(f[j], f[i] + time[j]);
                ans = Math.max(ans, f[j]);
                if (--indeg[j] == 0) {
                    q.offer(j);
                }
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
    int minimumTime(int n, vector<vector<int>>& relations, vector<int>& time) {
        vector<vector<int>> g(n);
        vector<int> indeg(n);
        for (auto& e : relations) {
            int a = e[0] - 1, b = e[1] - 1;
            g[a].push_back(b);
            ++indeg[b];
        }
        queue<int> q;
        vector<int> f(n);
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int v = indeg[i], t = time[i];
            if (v == 0) {
                q.push(i);
                f[i] = t;
                ans = max(ans, t);
            }
        }
        while (!q.empty()) {
            int i = q.front();
            q.pop();
            for (int j : g[i]) {
                if (--indeg[j] == 0) {
                    q.push(j);
                }
                f[j] = max(f[j], f[i] + time[j]);
                ans = max(ans, f[j]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumTime(n int, relations [][]int, time []int) int {
	g := make([][]int, n)
	indeg := make([]int, n)
	for _, e := range relations {
		a, b := e[0]-1, e[1]-1
		g[a] = append(g[a], b)
		indeg[b]++
	}
	f := make([]int, n)
	q := []int{}
	ans := 0
	for i, v := range indeg {
		if v == 0 {
			q = append(q, i)
			f[i] = time[i]
			ans = max(ans, time[i])
		}
	}
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		for _, j := range g[i] {
			indeg[j]--
			if indeg[j] == 0 {
				q = append(q, j)
			}
			f[j] = max(f[j], f[i]+time[j])
			ans = max(ans, f[j])
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minimumTime(n: number, relations: number[][], time: number[]): number {
    const g: number[][] = Array(n)
        .fill(0)
        .map(() => []);
    const indeg: number[] = Array(n).fill(0);
    for (const [a, b] of relations) {
        g[a - 1].push(b - 1);
        ++indeg[b - 1];
    }
    const q: number[] = [];
    const f: number[] = Array(n).fill(0);
    let ans: number = 0;
    for (let i = 0; i < n; ++i) {
        if (indeg[i] === 0) {
            q.push(i);
            f[i] = time[i];
            ans = Math.max(ans, f[i]);
        }
    }
    while (q.length > 0) {
        const i = q.shift()!;
        for (const j of g[i]) {
            f[j] = Math.max(f[j], f[i] + time[j]);
            ans = Math.max(ans, f[j]);
            if (--indeg[j] === 0) {
                q.push(j);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
