---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
    - Array
---

<!-- problem:start -->

# [3383. Minimum Runes to Add to Cast Spell 🔒](https://leetcode.com/problems/minimum-runes-to-add-to-cast-spell)

[中文文档](/solution/3300-3399/3383.Minimum%20Runes%20to%20Add%20to%20Cast%20Spell/README.md)

## Mô tả

<!-- description:start -->

<p>Alice vừa tốt nghiệp trường pháp sư và muốn niệm một câu thần chú để ăn mừng. Câu thần chú chứa một số <strong>điểm tập trung</strong> tại đó ma thuật cần được tập trung, và một số điểm trong đó chứa <strong>tinh thể ma thuật</strong> đóng vai trò là nguồn năng lượng của câu thần chú. Các điểm tập trung có thể được liên kết qua các <strong>rune có hướng</strong>, giúp truyền dòng ma lực từ điểm tập trung này sang điểm khác.</p>

<p>Bạn được cho một số nguyên <code>n</code> biểu thị <em>số lượng</em> điểm tập trung và một mảng số nguyên <code>crystals</code>, trong đó <code>crystals[i]</code> cho biết một điểm tập trung chứa tinh thể ma thuật. Bạn cũng được cho hai mảng số nguyên <code>flowFrom</code> và <code>flowTo</code>, biểu diễn các <strong>rune có hướng</strong> hiện có. Rune thứ <code>i<sup>th</sup></code> cho phép dòng ma lực tự do chảy từ điểm tập trung <code>flowFrom[i]</code> đến điểm tập trung <code>flowTo[i]</code>.</p>

<p>Bạn cần tìm số rune có hướng mà Alice phải thêm vào câu thần chú sao cho <em>mọi</em> điểm tập trung đều thỏa mãn một trong các điều kiện sau:</p>

<ul>
	<li><strong>Chứa</strong> một tinh thể ma thuật.</li>
	<li><strong>Nhận</strong> dòng ma lực <em>từ</em> một điểm tập trung khác.</li>
</ul>

<p>Trả về số rune có hướng <strong>nhỏ nhất</strong> mà cô ấy cần thêm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 6, crystals = [0], flowFrom = [0,1,2,3], flowTo = [1,2,3,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong>&nbsp;</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3383.Minimum%20Runes%20to%20Add%20to%20Cast%20Spell/images/runesexample0.png" style="width: 250px; height: 252px;" /></p>

<p>Thêm hai rune có hướng:</p>

<ul>
	<li>Từ điểm tập trung 0 đến điểm tập trung 4.</li>
	<li>Từ điểm tập trung 0 đến điểm tập trung 5.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 7, crystals = [3,5], flowFrom = [0,1,2,3,5], flowTo = [1,2,0,4,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong>&nbsp;</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3383.Minimum%20Runes%20to%20Add%20to%20Cast%20Spell/images/runesexample1.png" style="width: 250px; height: 250px;" /></p>

<p>Thêm một rune có hướng từ điểm tập trung 4 đến điểm tập trung 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= crystals.length &lt;= n</code></li>
	<li><code>0 &lt;= crystals[i] &lt;= n - 1</code></li>
	<li><code>1 &lt;= flowFrom.length == flowTo.length &lt;= min(2 * 10<sup>5</sup>, (n * (n - 1)) / 2)</code></li>
	<li><code>0 &lt;= flowFrom[i], flowTo[i] &lt;= n - 1</code></li>
	<li><code>flowFrom[i] != flowTo[i]</code></li>
	<li>Tất cả các rune có hướng đã tồn tại đều <strong>phân biệt</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các tinh thể đã thắp sáng tập các điểm có thể đi tới từ chúng. Ta muốn thêm ít rune nhất từ một nút đã được thắp sáng để thắp sáng toàn bộ đồ thị có hướng. Với $n \le 10^5$, trước tiên ta dùng BFS để tìm bao đóng các điểm đã được thắp sáng.
>
> Các nút còn lại được xử lý theo thứ tự kết thúc của DFS đảo ngược, tức là các nguồn của DAG còn lại được xử lý trước.
>
> Mỗi nút vẫn chưa được thắp sáng theo thứ tự đó cần một rune mới; sau đó BFS sẽ mở rộng bao đóng. Vì vậy, mỗi cạnh được thêm vào đều bao phủ một nguồn mới của phần đồ thị còn lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minRunesToAdd(
        self, n: int, crystals: List[int], flowFrom: List[int], flowTo: List[int]
    ) -> int:
        def bfs(q: Deque[int]):
            while q:
                a = q.popleft()
                for b in g[a]:
                    if vis[b] == 1:
                        continue
                    vis[b] = 1
                    q.append(b)

        def dfs(a: int):
            vis[a] = 2
            for b in g[a]:
                if vis[b] > 0:
                    continue
                dfs(b)
            seq.append(a)

        g = [[] for _ in range(n)]
        for a, b in zip(flowFrom, flowTo):
            g[a].append(b)

        q = deque(crystals)
        vis = [0] * n
        for x in crystals:
            vis[x] = 1
        bfs(q)

        seq = []
        for i in range(n):
            if vis[i] == 0:
                dfs(i)
        seq.reverse()
        ans = 0
        for i in seq:
            if vis[i] == 2:
                q = deque([i])
                vis[i] = 1
                bfs(q)
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    private int[] vis;
    private List<Integer>[] g;
    private List<Integer> seq = new ArrayList<>();

    public int minRunesToAdd(int n, int[] crystals, int[] flowFrom, int[] flowTo) {
        g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (int i = 0; i < flowFrom.length; ++i) {
            g[flowFrom[i]].add(flowTo[i]);
        }
        Deque<Integer> q = new ArrayDeque<>();
        vis = new int[n];
        for (int i : crystals) {
            vis[i] = 1;
            q.offer(i);
        }
        bfs(q);
        for (int i = 0; i < n; ++i) {
            if (vis[i] == 0) {
                dfs(i);
            }
        }
        int ans = 0;
        for (int i = seq.size() - 1; i >= 0; --i) {
            int a = seq.get(i);
            if (vis[a] == 2) {
                vis[a] = 1;
                q.clear();
                q.offer(a);
                bfs(q);
                ++ans;
            }
        }
        return ans;
    }

    private void bfs(Deque<Integer> q) {
        while (!q.isEmpty()) {
            int a = q.poll();
            for (int b : g[a]) {
                if (vis[b] == 1) {
                    continue;
                }
                vis[b] = 1;
                q.offer(b);
            }
        }
    }

    private void dfs(int a) {
        vis[a] = 2;
        for (int b : g[a]) {
            if (vis[b] > 0) {
                continue;
            }
            dfs(b);
        }
        seq.add(a);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> vis;
    vector<vector<int>> g;
    vector<int> seq;

    int minRunesToAdd(int n, vector<int>& crystals, vector<int>& flowFrom, vector<int>& flowTo) {
        g.resize(n);
        for (int i = 0; i < flowFrom.size(); ++i) {
            g[flowFrom[i]].push_back(flowTo[i]);
        }

        deque<int> q;
        vis.resize(n, 0);
        for (int i : crystals) {
            vis[i] = 1;
            q.push_back(i);
        }
        bfs(q);

        for (int i = 0; i < n; ++i) {
            if (vis[i] == 0) {
                dfs(i);
            }
        }

        int ans = 0;
        for (int i = seq.size() - 1; i >= 0; --i) {
            int a = seq[i];
            if (vis[a] == 2) {
                vis[a] = 1;
                q.clear();
                q.push_back(a);
                bfs(q);
                ++ans;
            }
        }
        return ans;
    }

private:
    void bfs(deque<int>& q) {
        while (!q.empty()) {
            int a = q.front();
            q.pop_front();
            for (int b : g[a]) {
                if (vis[b] == 1) {
                    continue;
                }
                vis[b] = 1;
                q.push_back(b);
            }
        }
    }

    void dfs(int a) {
        vis[a] = 2;
        for (int b : g[a]) {
            if (vis[b] > 0) {
                continue;
            }
            dfs(b);
        }
        seq.push_back(a);
    }
};
```

#### Go

```go
func minRunesToAdd(n int, crystals []int, flowFrom []int, flowTo []int) (ans int) {
	g := make([][]int, n)
	for i := 0; i < len(flowFrom); i++ {
		a, b := flowFrom[i], flowTo[i]
		g[a] = append(g[a], b)
	}

	vis := make([]int, n)
	for _, x := range crystals {
		vis[x] = 1
	}

	bfs := func(q []int) {
		for len(q) > 0 {
			a := q[0]
			q = q[1:]
			for _, b := range g[a] {
				if vis[b] == 1 {
					continue
				}
				vis[b] = 1
				q = append(q, b)
			}
		}
	}

	seq := []int{}
	var dfs func(a int)
	dfs = func(a int) {
		vis[a] = 2
		for _, b := range g[a] {
			if vis[b] > 0 {
				continue
			}
			dfs(b)
		}
		seq = append(seq, a)
	}

	q := crystals
	bfs(q)

	for i := 0; i < n; i++ {
		if vis[i] == 0 {
			dfs(i)
		}
	}

	for i, j := 0, len(seq)-1; i < j; i, j = i+1, j-1 {
		seq[i], seq[j] = seq[j], seq[i]
	}
	for _, i := range seq {
		if vis[i] == 2 {
			q = []int{i}
			vis[i] = 1
			bfs(q)
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function minRunesToAdd(
    n: number,
    crystals: number[],
    flowFrom: number[],
    flowTo: number[],
): number {
    const g: number[][] = Array.from({ length: n }, () => []);
    for (let i = 0; i < flowFrom.length; i++) {
        const a = flowFrom[i],
            b = flowTo[i];
        g[a].push(b);
    }

    const vis: number[] = Array(n).fill(0);
    for (const x of crystals) {
        vis[x] = 1;
    }

    const bfs = (q: number[]) => {
        while (q.length > 0) {
            const a = q.shift()!;
            for (const b of g[a]) {
                if (vis[b] === 1) continue;
                vis[b] = 1;
                q.push(b);
            }
        }
    };

    const seq: number[] = [];
    const dfs = (a: number) => {
        vis[a] = 2;
        for (const b of g[a]) {
            if (vis[b] > 0) continue;
            dfs(b);
        }
        seq.push(a);
    };

    bfs(crystals);

    for (let i = 0; i < n; i++) {
        if (vis[i] === 0) {
            dfs(i);
        }
    }

    seq.reverse();

    let ans = 0;
    for (const i of seq) {
        if (vis[i] === 2) {
            bfs([i]);
            vis[i] = 1;
            ans++;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
