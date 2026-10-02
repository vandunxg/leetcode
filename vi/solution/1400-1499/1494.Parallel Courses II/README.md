---
comments: true
difficulty: Hard
rating: 2081
source: Biweekly Contest 29 Q4
tags:
    - Bit Manipulation
    - Graph
    - Dynamic Programming
    - Bitmask
    - Directed Acyclic Graph
---

<!-- problem:start -->

# [1494. Parallel Courses II](https://leetcode.com/problems/parallel-courses-ii)

[Tài liệu tiếng Trung](/solution/1400-1499/1494.Parallel%20Courses%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>, biểu thị có <code>n</code> khóa học được đánh số từ <code>1</code> đến <code>n</code>. Bạn cũng được cho một mảng <code>relations</code>, trong đó <code>relations[i] = [prevCourse<sub>i</sub>, nextCourse<sub>i</sub>]</code> biểu thị mối quan hệ tiên quyết giữa khóa học <code>prevCourse<sub>i</sub></code> và khóa học <code>nextCourse<sub>i</sub></code>: khóa học <code>prevCourse<sub>i</sub></code> phải được học trước khóa học <code>nextCourse<sub>i</sub></code>. Ngoài ra, bạn được cho số nguyên <code>k</code>.</p>

<p>Trong một học kỳ, bạn có thể học <strong>tối đa</strong> <code>k</code> khóa học, miễn là bạn đã học tất cả các khóa học tiên quyết trong những học kỳ <strong>trước đó</strong> của các khóa học mà bạn đăng ký.</p>

<p>Trả về <em><strong>số học kỳ tối thiểu</strong> cần thiết để học tất cả các khóa học</em>. Dữ liệu kiểm thử được tạo sao cho có thể học mọi khóa học.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1494.Parallel%20Courses%20II/images/leetcode_parallel_courses_1.png" style="width: 269px; height: 147px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, relations = [[2,1],[3,1],[1,4]], k = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Hình trên biểu diễn đồ thị đã cho.
Trong học kỳ đầu tiên, bạn có thể học các khóa học 2 và 3.
Trong học kỳ thứ hai, bạn có thể học khóa học 1.
Trong học kỳ thứ ba, bạn có thể học khóa học 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1494.Parallel%20Courses%20II/images/leetcode_parallel_courses_2.png" style="width: 271px; height: 211px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, relations = [[2,1],[3,1],[4,1],[1,5]], k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Hình trên biểu diễn đồ thị đã cho.
Trong học kỳ đầu tiên, bạn chỉ có thể học các khóa học 2 và 3 vì mỗi học kỳ không thể học quá hai khóa.
Trong học kỳ thứ hai, bạn có thể học khóa học 4.
Trong học kỳ thứ ba, bạn có thể học khóa học 1.
Trong học kỳ thứ tư, bạn có thể học khóa học 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 15</code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
	<li><code>0 &lt;= relations.length &lt;= n * (n-1) / 2</code></li>
	<li><code>relations[i].length == 2</code></li>
	<li><code>1 &lt;= prevCourse<sub>i</sub>, nextCourse<sub>i</sub> &lt;= n</code></li>
	<li><code>prevCourse<sub>i</sub> != nextCourse<sub>i</sub></code></li>
	<li>Tất cả các cặp <code>[prevCourse<sub>i</sub>, nextCourse<sub>i</sub>]</code> là <strong>duy nhất</strong>.</li>
	<li>Đồ thị đã cho là đồ thị có hướng không chu trình.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 15$. Mỗi học kỳ học tối đa $k$ khóa, đồng thời phải tuân thủ các khóa tiên quyết. Một bit mask biểu diễn tập các khóa học đã hoàn thành; BFS cho số học kỳ ít nhất.
>
> Một khóa học sẵn sàng khi tất cả các bit khóa tiên quyết của nó đều được bật. Nếu có không quá $k$ khóa sẵn sàng, học tất cả chúng; nếu không, đưa vào queue mọi tập con gồm $k$ khóa. Đích là khi các bit của tất cả khóa học đều được bật.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minNumberOfSemesters(self, n: int, relations: List[List[int]], k: int) -> int:
        d = [0] * (n + 1)
        for x, y in relations:
            d[y] |= 1 << x
        q = deque([(0, 0)])
        vis = {0}
        while q:
            cur, t = q.popleft()
            if cur == (1 << (n + 1)) - 2:
                return t
            nxt = 0
            for i in range(1, n + 1):
                if (cur & d[i]) == d[i]:
                    nxt |= 1 << i
            nxt ^= cur
            if nxt.bit_count() <= k:
                if (nxt | cur) not in vis:
                    vis.add(nxt | cur)
                    q.append((nxt | cur, t + 1))
            else:
                x = nxt
                while nxt:
                    if nxt.bit_count() == k and (nxt | cur) not in vis:
                        vis.add(nxt | cur)
                        q.append((nxt | cur, t + 1))
                    nxt = (nxt - 1) & x
```

#### Java

```java
class Solution {
    public int minNumberOfSemesters(int n, int[][] relations, int k) {
        int[] d = new int[n + 1];
        for (var e : relations) {
            d[e[1]] |= 1 << e[0];
        }
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {0, 0});
        Set<Integer> vis = new HashSet<>();
        vis.add(0);
        while (!q.isEmpty()) {
            var p = q.pollFirst();
            int cur = p[0], t = p[1];
            if (cur == (1 << (n + 1)) - 2) {
                return t;
            }
            int nxt = 0;
            for (int i = 1; i <= n; ++i) {
                if ((cur & d[i]) == d[i]) {
                    nxt |= 1 << i;
                }
            }
            nxt ^= cur;
            if (Integer.bitCount(nxt) <= k) {
                if (vis.add(nxt | cur)) {
                    q.offer(new int[] {nxt | cur, t + 1});
                }
            } else {
                int x = nxt;
                while (nxt > 0) {
                    if (Integer.bitCount(nxt) == k && vis.add(nxt | cur)) {
                        q.offer(new int[] {nxt | cur, t + 1});
                    }
                    nxt = (nxt - 1) & x;
                }
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minNumberOfSemesters(int n, vector<vector<int>>& relations, int k) {
        vector<int> d(n + 1);
        for (auto& e : relations) {
            d[e[1]] |= 1 << e[0];
        }
        queue<pair<int, int>> q;
        q.push({0, 0});
        unordered_set<int> vis{{0}};
        while (!q.empty()) {
            auto [cur, t] = q.front();
            q.pop();
            if (cur == (1 << (n + 1)) - 2) {
                return t;
            }
            int nxt = 0;
            for (int i = 1; i <= n; ++i) {
                if ((cur & d[i]) == d[i]) {
                    nxt |= 1 << i;
                }
            }
            nxt ^= cur;
            if (__builtin_popcount(nxt) <= k) {
                if (!vis.count(nxt | cur)) {
                    vis.insert(nxt | cur);
                    q.push({nxt | cur, t + 1});
                }
            } else {
                int x = nxt;
                while (nxt) {
                    if (__builtin_popcount(nxt) == k && !vis.count(nxt | cur)) {
                        vis.insert(nxt | cur);
                        q.push({nxt | cur, t + 1});
                    }
                    nxt = (nxt - 1) & x;
                }
            }
        }
        return 0;
    }
};
```

#### Go

```go
func minNumberOfSemesters(n int, relations [][]int, k int) int {
	d := make([]int, n+1)
	for _, e := range relations {
		d[e[1]] |= 1 << e[0]
	}
	type pair struct{ v, t int }
	q := []pair{pair{0, 0}}
	vis := map[int]bool{0: true}
	for len(q) > 0 {
		p := q[0]
		q = q[1:]
		cur, t := p.v, p.t
		if cur == (1<<(n+1))-2 {
			return t
		}
		nxt := 0
		for i := 1; i <= n; i++ {
			if (cur & d[i]) == d[i] {
				nxt |= 1 << i
			}
		}
		nxt ^= cur
		if bits.OnesCount(uint(nxt)) <= k {
			if !vis[nxt|cur] {
				vis[nxt|cur] = true
				q = append(q, pair{nxt | cur, t + 1})
			}
		} else {
			x := nxt
			for nxt > 0 {
				if bits.OnesCount(uint(nxt)) == k && !vis[nxt|cur] {
					vis[nxt|cur] = true
					q = append(q, pair{nxt | cur, t + 1})
				}
				nxt = (nxt - 1) & x
			}
		}
	}
	return 0
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
