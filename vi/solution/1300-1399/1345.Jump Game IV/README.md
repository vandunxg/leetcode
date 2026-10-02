---
comments: true
difficulty: Hard
rating: 1809
source: Biweekly Contest 19 Q4
tags:
    - Breadth-First Search
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1345. Jump Game IV](https://leetcode.com/problems/jump-game-iv)

[中文文档](/solution/1300-1399/1345.Jump%20Game%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>, ban đầu bạn đứng ở chỉ số đầu tiên của mảng.</p>

<p>Trong một bước, bạn có thể nhảy từ chỉ số <code>i</code> đến:</p>

<ul>
	<li><code>i + 1</code> nếu <code>i + 1 &lt; arr.length</code>.</li>
	<li><code>i - 1</code> nếu <code>i - 1 &gt;= 0</code>.</li>
	<li><code>j</code> nếu <code>arr[i] == arr[j]</code> và <code>i != j</code>.</li>
</ul>

<p>Trả về <em>số bước ít nhất</em> để đến <strong>chỉ số cuối cùng</strong> của mảng.</p>

<p>Lưu ý rằng bạn không thể nhảy ra ngoài mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [100,-23,-23,404,100,23,23,23,3,404]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Cần ba bước nhảy từ chỉ số 0 --&gt; 4 --&gt; 3 --&gt; 9. Lưu ý chỉ số 9 là chỉ số cuối của mảng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [7]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Chỉ số bắt đầu cũng là chỉ số cuối, nên không cần nhảy.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [7,6,9,6,9,6,9,7]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bạn có thể nhảy trực tiếp từ chỉ số 0 đến chỉ số 7, là chỉ số cuối của mảng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>-10<sup>8</sup> &lt;= arr[i] &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước có thể nhảy sang chỉ số kề bên hoặc đến một chỉ số có cùng giá trị; mục tiêu là đến chỉ số cuối với ít bước nhất. Vì $n \le 5 \times 10^4$, duyệt lại danh sách các chỉ số có cùng giá trị sẽ quá chậm. BFS theo từng lớp đưa $i\pm 1$ và các chỉ số có cùng giá trị vào queue, sau đó $\textit{pop}$ danh sách của giá trị đó để mỗi cạnh dạng này chỉ được xét một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minJumps(self, arr: List[int]) -> int:
        g = defaultdict(list)
        for i, x in enumerate(arr):
            g[x].append(i)
        q = deque([0])
        vis = {0}
        ans = 0
        while 1:
            for _ in range(len(q)):
                i = q.popleft()
                if i == len(arr) - 1:
                    return ans
                for j in (i + 1, i - 1, *g.pop(arr[i], [])):
                    if 0 <= j < len(arr) and j not in vis:
                        q.append(j)
                        vis.add(j)
            ans += 1
```

#### Java

```java
class Solution {
    public int minJumps(int[] arr) {
        Map<Integer, List<Integer>> g = new HashMap<>();
        int n = arr.length;
        for (int i = 0; i < n; i++) {
            g.computeIfAbsent(arr[i], k -> new ArrayList<>()).add(i);
        }
        boolean[] vis = new boolean[n];
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(0);
        vis[0] = true;
        for (int ans = 0;; ++ans) {
            for (int k = q.size(); k > 0; --k) {
                int i = q.poll();
                if (i == n - 1) {
                    return ans;
                }
                for (int j : g.get(arr[i])) {
                    if (!vis[j]) {
                        vis[j] = true;
                        q.offer(j);
                    }
                }
                g.get(arr[i]).clear();
                for (int j : new int[] {i - 1, i + 1}) {
                    if (0 <= j && j < n && !vis[j]) {
                        vis[j] = true;
                        q.offer(j);
                    }
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
    int minJumps(vector<int>& arr) {
        unordered_map<int, vector<int>> g;
        int n = arr.size();
        for (int i = 0; i < n; ++i) {
            g[arr[i]].push_back(i);
        }
        vector<bool> vis(n);
        queue<int> q{{0}};
        vis[0] = true;
        for (int ans = 0;; ++ans) {
            for (int k = q.size(); k; --k) {
                int i = q.front();
                q.pop();
                if (i == n - 1) {
                    return ans;
                }
                for (int j : g[arr[i]]) {
                    if (!vis[j]) {
                        vis[j] = true;
                        q.push(j);
                    }
                }
                g[arr[i]].clear();
                for (int j : {i - 1, i + 1}) {
                    if (0 <= j && j < n && !vis[j]) {
                        vis[j] = true;
                        q.push(j);
                    }
                }
            }
        }
    }
};
```

#### Go

```go
func minJumps(arr []int) int {
	g := map[int][]int{}
	for i, x := range arr {
		g[x] = append(g[x], i)
	}
	n := len(arr)
	q := []int{0}
	vis := make([]bool, n)
	vis[0] = true
	for ans := 0; ; ans++ {
		for k := len(q); k > 0; k-- {
			i := q[0]
			q = q[1:]
			if i == n-1 {
				return ans
			}
			for _, j := range g[arr[i]] {
				if !vis[j] {
					vis[j] = true
					q = append(q, j)
				}
			}
			g[arr[i]] = nil
			for _, j := range []int{i - 1, i + 1} {
				if 0 <= j && j < n && !vis[j] {
					vis[j] = true
					q = append(q, j)
				}
			}
		}
	}
}
```

#### TypeScript

```ts
function minJumps(arr: number[]): number {
    const g: Map<number, number[]> = new Map();
    const n = arr.length;
    for (let i = 0; i < n; ++i) {
        if (!g.has(arr[i])) {
            g.set(arr[i], []);
        }
        g.get(arr[i])!.push(i);
    }
    let q: number[] = [0];
    const vis: boolean[] = Array(n).fill(false);
    vis[0] = true;
    for (let ans = 0; ; ++ans) {
        const nq: number[] = [];
        for (const i of q) {
            if (i === n - 1) {
                return ans;
            }
            for (const j of g.get(arr[i])!) {
                if (!vis[j]) {
                    vis[j] = true;
                    nq.push(j);
                }
            }
            g.get(arr[i])!.length = 0;
            for (const j of [i - 1, i + 1]) {
                if (j >= 0 && j < n && !vis[j]) {
                    vis[j] = true;
                    nq.push(j);
                }
            }
        }
        q = nq;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
