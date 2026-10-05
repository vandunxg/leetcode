---
comments: true
difficulty: Medium
rating: 1698
source: Biweekly Contest 185 Q3
tags:
    - Tree
    - Depth-First Search
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3965. Finish Time of Tasks I](https://leetcode.com/problems/finish-time-of-tasks-i)

[中文文档](/solution/3900-3999/3965.Finish%20Time%20of%20Tasks%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị số lượng task trong một project, được đánh số từ 0 đến <code>n - 1</code>. Các task này được liên kết thành một <strong>cây</strong> có gốc là task 0. Cây được biểu diễn bằng mảng số nguyên hai chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết task <code>u<sub>i</sub></code> là cha của task <code>v<sub>i</sub></code>.</p>

<p>Bạn cũng được cho một mảng <code>baseTime</code> có độ dài <code>n</code>, trong đó <code>baseTime[i]</code> là thời gian cần để hoàn thành task <code>i</code>.</p>

<p><strong>Thời gian hoàn thành</strong> của mỗi task được tính như sau:</p>

<ul>
	<li>Task lá: Thời gian hoàn thành là <code>baseTime[i]</code>.</li>
	<li>Task không phải lá:
	<ul>
		<li>Gọi <code>earliest</code> là thời gian hoàn thành <strong>nhỏ nhất</strong> trong các task con, và <code>latest</code> là thời gian hoàn thành <strong>lớn nhất</strong> trong các task con.</li>
		<li>Gọi <code>ownDuration</code> là <code>(latest - earliest) + baseTime[i]</code>.</li>
		<li>Thời gian hoàn thành của task <code>i</code> là <code>latest + ownDuration</code>.</li>
	</ul>
	</li>
</ul>

<p>Trả về thời gian hoàn thành của task gốc 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[1,2]], baseTime = [9,5,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">17</span></p>

<p><strong>Giải thích:</strong></p>
<svg height="100" viewbox="0 0 420 140" width="300" xmlns="http://www.w3.org/2000/svg"> <rect fill="white" height="100%" width="100%"></rect> <line stroke="black" stroke-width="2" x1="80" x2="210" y1="60" y2="60"></line> <line stroke="black" stroke-width="2" x1="210" x2="340" y1="60" y2="60"></line> <circle cx="80" cy="60" fill="white" r="24" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="16" text-anchor="middle" x="80" y="65">0</text> <text fill="black" font-size="14" text-anchor="middle" x="80" y="100">9</text> <circle cx="210" cy="60" fill="white" r="24" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="16" text-anchor="middle" x="210" y="65">1</text> <text fill="black" font-size="14" text-anchor="middle" x="210" y="100">5</text> <circle cx="340" cy="60" fill="white" r="24" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="16" text-anchor="middle" x="340" y="65">2</text> <text fill="black" font-size="14" text-anchor="middle" x="340" y="100">3</text> </svg>

<ul>
	<li>Task 2 là task lá, nên thời gian hoàn thành là <code>baseTime[2] = 3</code>.</li>
	<li>Task 1 có một task con là task 2:
	<ul>
		<li><code>earliest = latest = 3</code></li>
		<li><code>ownDuration = (latest - earliest) + baseTime[1] = 5</code></li>
		<li>Thời gian hoàn thành của task 1 là <code>3 + 5 = 8</code></li>
	</ul>
	</li>
	<li>Task 0 có một task con với thời gian hoàn thành bằng 8:
	<ul>
		<li><code>earliest = latest = 8</code></li>
		<li><code>ownDuration = (latest - earliest) + baseTime[0] = 9</code></li>
		<li>Thời gian hoàn thành của task 0 là <code>8 + 9 = 17</code></li>
	</ul>
	</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[0,2]], baseTime = [4,7,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>
<svg height="130" viewbox="0 0 420 180" width="300" xmlns="http://www.w3.org/2000/svg"> <rect fill="white" height="100%" width="100%"></rect> <line stroke="black" stroke-width="2" x1="210" x2="110" y1="60" y2="130"></line> <line stroke="black" stroke-width="2" x1="210" x2="310" y1="60" y2="130"></line> <circle cx="210" cy="60" fill="white" r="24" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="16" text-anchor="middle" x="210" y="65">0</text> <text fill="black" font-size="14" text-anchor="middle" x="210" y="100">4</text> <circle cx="110" cy="130" fill="white" r="24" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="16" text-anchor="middle" x="110" y="135">1</text> <text fill="black" font-size="14" text-anchor="middle" x="110" y="170">7</text> <circle cx="310" cy="130" fill="white" r="24" stroke="black" stroke-width="2"></circle> <text fill="black" font-size="16" text-anchor="middle" x="310" y="135">2</text> <text fill="black" font-size="14" text-anchor="middle" x="310" y="170">6</text> </svg>

<ul>
	<li>Task 1 là task lá, nên thời gian hoàn thành là <code>baseTime[1] = 7</code>.</li>
	<li>Task 2 là task lá, nên thời gian hoàn thành là <code>baseTime[2] = 6</code>.</li>
	<li>Task 0 có hai task con với thời gian hoàn thành lần lượt là 7 và 6:
	<ul>
		<li><code>earliest = 6</code>, <code>latest = 7</code></li>
		<li><code>ownDuration = (latest - earliest) + baseTime[0] = (7 - 6) + 4 = 5</code></li>
		<li>Thời gian hoàn thành của task 0 là <code>latest + ownDuration = 7 + 5 = 12</code></li>
	</ul>
	</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1],[0,2],[2,3]], baseTime = [5,8,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">18</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Task 1 là task lá, nên thời gian hoàn thành là <code>baseTime[1] = 8</code>.</li>
	<li>Task 3 là task lá, nên thời gian hoàn thành là <code>baseTime[3] = 1</code>.</li>
	<li>Task 2 có một task con là task 3:
	<ul>
		<li><code>earliest = latest = 1</code></li>
		<li><code>ownDuration = (latest - earliest) + baseTime[2] = 0 + 2 = 2</code></li>
		<li>Thời gian hoàn thành của task 2 là <code>latest + ownDuration = 1 + 2 = 3</code></li>
	</ul>
	</li>
	<li>Task 0 có hai task con với thời gian hoàn thành lần lượt là 8 và 3:
	<ul>
		<li><code>earliest = 3</code>, <code>latest = 8</code></li>
		<li><code>ownDuration = (latest - earliest) + baseTime[0] = (8 - 3) + 5 = 10</code></li>
		<li>Thời gian hoàn thành của task 0 là <code>latest + ownDuration = 8 + 10 = 18</code></li>
	</ul>
	</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length = n - 1</code></li>
	<li><code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>u<sub>i </sub>!= v<sub>i</sub></code></li>
	<li>Đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
	<li><code>baseTime.length == n</code></li>
	<li><code>1 &lt;= baseTime[i] &lt;= 10<sup>5</sup></code>​​​​​​​</li>
	<li>Thời gian hoàn thành của mỗi task được đảm bảo nhỏ hơn <code>2<sup>53</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Cây chính là graph biểu diễn dependency. Thời gian hoàn thành của một node cần thời gian hoàn thành của mọi task con, cùng với $\textit{baseTime}$ của chính nó.
>
> Dùng DFS từ dưới lên: leaf trả về $\textit{baseTime}[i]$; node bên trong lấy $\textit{earliest}$ và $\textit{latest}$ trong các task con, tốn $\textit{latest}-\textit{earliest}+\textit{baseTime}[i]$, rồi trả về $\textit{latest}$ cộng với khoảng thời gian đó.
>
> Cây có $n-1$ cạnh, nên một lần DFS chính là thời gian hoàn thành của root.

<!-- thinking:end -->

Trước tiên, xây dựng cây từ danh sách cạnh $\textit{edges}$ và lưu các task con của mỗi node trong adjacency list $g$.

Sau đó thực hiện DFS bắt đầu từ node gốc $0$. Định nghĩa hàm $\textit{dfs}(i)$ trả về thời gian hoàn thành của task $i$:

- Nếu $i$ là node lá, trả về trực tiếp $\textit{baseTime}[i]$;
- Ngược lại, đệ quy tính thời gian hoàn thành của mọi task con, rồi gọi giá trị nhỏ nhất và lớn nhất trong số đó lần lượt là $\textit{earliest}$ và $\textit{latest}$;
- Khoảng thời gian riêng của task hiện tại là $\textit{ownDuration} = (\textit{latest} - \textit{earliest}) + \textit{baseTime}[i]$;
- Thời gian hoàn thành của task $i$ là $\textit{latest} + \textit{ownDuration}$.

Đáp án là $\textit{dfs}(0)$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def finishTime(self, n: int, edges: List[List[int]], baseTime: List[int]) -> int:
        def dfs(i: int) -> int:
            if not g[i]:
                return baseTime[i]
            earliest, latest = inf, -inf
            for j in g[i]:
                a = dfs(j)
                earliest = min(earliest, a)
                latest = max(latest, a)
            own_duration = (latest - earliest) + baseTime[i]
            return latest + own_duration

        g = [[] for _ in range(n)]
        for u, v in edges:
            g[u].append(v)
        return dfs(0)
```

#### Java

```java
class Solution {
    List<Integer>[] g;
    int[] baseTime;

    long dfs(int i) {
        if (g[i].isEmpty()) {
            return baseTime[i];
        }

        long earliest = Long.MAX_VALUE / 4;
        long latest = Long.MIN_VALUE / 4;

        for (int j : g[i]) {
            long a = dfs(j);
            earliest = Math.min(earliest, a);
            latest = Math.max(latest, a);
        }

        long ownDuration = (latest - earliest) + baseTime[i];
        return latest + ownDuration;
    }

    public long finishTime(int n, int[][] edges, int[] baseTime) {
        this.baseTime = baseTime;
        g = new ArrayList[n];
        Arrays.setAll(g, k -> new ArrayList<>());

        for (int[] e : edges) {
            g[e[0]].add(e[1]);
        }

        return dfs(0);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long finishTime(int n, vector<vector<int>>& edges, vector<int>& baseTime) {
        vector<vector<int>> g(n);

        for (auto& e : edges) {
            g[e[0]].push_back(e[1]);
        }

        auto dfs = [&](this auto&& dfs, int i) -> long long {
            if (g[i].empty()) {
                return baseTime[i];
            }

            long long earliest = LLONG_MAX / 4;
            long long latest = -LLONG_MAX / 4;

            for (int j : g[i]) {
                long long a = dfs(j);
                earliest = min(earliest, a);
                latest = max(latest, a);
            }

            long long own_duration = (latest - earliest) + baseTime[i];
            return latest + own_duration;
        };

        return dfs(0);
    }
};
```

#### Go

```go
func finishTime(n int, edges [][]int, baseTime []int) int64 {
	g := make([][]int, n)

	for _, e := range edges {
		g[e[0]] = append(g[e[0]], e[1])
	}

	var dfs func(int) int64

	dfs = func(i int) int64 {
		if len(g[i]) == 0 {
			return int64(baseTime[i])
		}

		var INF int64 = 1 << 62
		var earliest int64 = INF
		var latest int64 = -INF

		for _, j := range g[i] {
			a := dfs(j)
            earliest = min(earliest, a)
            latest = max(latest, a)
		}

		ownDuration := (latest - earliest) + int64(baseTime[i])
		return latest + ownDuration
	}

	return dfs(0)
}
```

#### TypeScript

```ts
function finishTime(n: number, edges: number[][], baseTime: number[]): number {
    const g: number[][] = Array.from({ length: n }, () => []);

    for (const [u, v] of edges) {
        g[u].push(v);
    }

    const dfs = (i: number): number => {
        if (g[i].length === 0) {
            return baseTime[i];
        }

        let earliest = Number.MAX_SAFE_INTEGER;
        let latest = -Number.MAX_SAFE_INTEGER;

        for (const j of g[i]) {
            const a = dfs(j);
            earliest = Math.min(earliest, a);
            latest = Math.max(latest, a);
        }

        const ownDuration = latest - earliest + baseTime[i];
        return latest + ownDuration;
    };

    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
