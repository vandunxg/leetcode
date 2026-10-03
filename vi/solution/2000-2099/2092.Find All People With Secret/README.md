---
comments: true
difficulty: Hard
rating: 2003
source: Weekly Contest 269 Q4
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
    - Sorting
---

<!-- problem:start -->

# [2092. Find All People With Secret](https://leetcode.com/problems/find-all-people-with-secret)

[中文文档](/solution/2000-2099/2092.Find%20All%20People%20With%20Secret/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>, cho biết có <code>n</code> người được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn cũng được cho một mảng số nguyên 2 chiều <strong>đánh chỉ số từ 0</strong> <code>meetings</code>, trong đó <code>meetings[i] = [x<sub>i</sub>, y<sub>i</sub>, time<sub>i</sub>]</code> cho biết người <code>x<sub>i</sub></code> và người <code>y<sub>i</sub></code> có một cuộc gặp tại thời điểm <code>time<sub>i</sub></code>. Một người có thể tham gia <strong>nhiều cuộc gặp</strong> cùng một thời điểm. Cuối cùng, bạn được cho một số nguyên <code>firstPerson</code>.</p>

<p>Người <code>0</code> có một <strong>bí mật</strong> và ban đầu chia sẻ bí mật với người <code>firstPerson</code> tại thời điểm <code>0</code>. Sau đó, bí mật được chia sẻ mỗi khi diễn ra một cuộc gặp với một người biết bí mật. Cụ thể hơn, với mỗi cuộc gặp, nếu người <code>x<sub>i</sub></code> biết bí mật tại thời điểm <code>time<sub>i</sub></code>, người đó sẽ chia sẻ bí mật với người <code>y<sub>i</sub></code>, và ngược lại.</p>

<p>Bí mật được chia sẻ <strong>ngay lập tức</strong>. Nghĩa là một người có thể nhận được bí mật rồi chia sẻ bí mật đó với những người trong các cuộc gặp khác cùng thời điểm.</p>

<p>Trả về <em>danh sách tất cả những người biết bí mật sau khi tất cả cuộc gặp đã diễn ra.</em> Bạn có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6, meetings = [[1,2,5],[2,3,8],[1,5,10]], firstPerson = 1
<strong>Đầu ra:</strong> [0,1,2,3,5]
<strong>Giải thích:
</strong>Ở thời điểm 0, người 0 chia sẻ bí mật với người 1.
Ở thời điểm 5, người 1 chia sẻ bí mật với người 2.
Ở thời điểm 8, người 2 chia sẻ bí mật với người 3.
Ở thời điểm 10, người 1 chia sẻ bí mật với người 5.​​​​
Do đó, những người 0, 1, 2, 3 và 5 biết bí mật sau khi tất cả cuộc gặp đã diễn ra.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, meetings = [[3,1,3],[1,2,2],[0,3,3]], firstPerson = 3
<strong>Đầu ra:</strong> [0,1,3]
<strong>Giải thích:</strong>
Ở thời điểm 0, người 0 chia sẻ bí mật với người 3.
Ở thời điểm 2, cả người 1 và người 2 đều chưa biết bí mật.
Ở thời điểm 3, người 3 chia sẻ bí mật với người 0 và người 1.
Do đó, những người 0, 1 và 3 biết bí mật sau khi tất cả cuộc gặp đã diễn ra.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, meetings = [[3,4,2],[1,2,1],[2,3,1]], firstPerson = 1
<strong>Đầu ra:</strong> [0,1,2,3,4]
<strong>Giải thích:</strong>
Ở thời điểm 0, người 0 chia sẻ bí mật với người 1.
Ở thời điểm 1, người 1 chia sẻ bí mật với người 2, đồng thời người 2 chia sẻ bí mật với người 3.
Lưu ý rằng người 2 có thể chia sẻ bí mật ngay khi nhận được bí mật đó.
Ở thời điểm 2, người 3 chia sẻ bí mật với người 4.
Do đó, những người 0, 1, 2, 3 và 4 biết bí mật sau khi tất cả cuộc gặp đã diễn ra.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= meetings.length &lt;= 10<sup>5</sup></code></li>
	<li><code>meetings[i].length == 3</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i </sub>&lt;= n - 1</code></li>
	<li><code>x<sub>i</sub> != y<sub>i</sub></code></li>
	<li><code>1 &lt;= time<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= firstPerson &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng + Duyệt đồ thị

<!-- thinking:start -->

> **Tư duy**
>
> Bí mật lan truyền qua các thành phần liên thông của những cuộc gặp diễn ra cùng thời điểm. Cả $n$ và số cuộc gặp đều bằng $10^5$. Ta sắp xếp theo thời gian; mỗi mốc thời gian tạo thành một đồ thị tạm thời, và chỉ BFS từ những người đã biết bí mật.
>
> Các thời điểm về sau không nhìn lại quá khứ. `vis` giữ nguyên giá trị true sau khi đã được thiết lập.

<!-- thinking:end -->

Ý tưởng cốt lõi của bài toán này là tìm tất cả những người cuối cùng biết bí mật bằng cách mô phỏng quá trình lan truyền qua các cuộc gặp. Ta có thể xem mỗi cuộc gặp là một cạnh trong đồ thị vô hướng, mỗi người tham gia là một node của đồ thị, còn thời điểm gặp là "timestamp" của cạnh nối hai node. Ở mỗi thời điểm, ta duyệt qua tất cả các cuộc gặp và dùng Breadth-First Search (BFS) để mô phỏng quá trình lan truyền bí mật.

Ta tạo một mảng boolean $\textit{vis}$ để ghi nhận mỗi người có biết bí mật hay không. Ban đầu, $\textit{vis}[0] = \text{true}$ và $\textit{vis}[\textit{firstPerson}] = \text{true}$, cho biết người 0 và $\textit{firstPerson}$ đã biết bí mật.

Trước tiên, ta sắp xếp các cuộc gặp theo thời gian để bảo đảm xử lý đúng thứ tự trong mỗi lần duyệt.

Tiếp theo, ta xử lý từng nhóm cuộc gặp diễn ra cùng thời điểm. Với mỗi nhóm cuộc gặp cùng thời điểm, tức là $\textit{meetings}[i][2] = \textit{meetings}[i+1][2]$, ta xem chúng là các sự kiện xảy ra cùng một lúc. Với mỗi cuộc gặp, ta ghi lại mối quan hệ giữa những người tham gia và thêm những người này vào một set.

Sau đó, ta dùng BFS để lan truyền bí mật. Với tất cả người tham gia tại thời điểm hiện tại, nếu bất kỳ ai trong số họ đã biết bí mật, ta sẽ duyệt các node kề với người đó, tức là những người khác trong cuộc gặp, bằng BFS và đánh dấu họ đã biết bí mật. Quá trình này tiếp tục cho đến khi cập nhật xong tất cả những người có thể biết bí mật trong cuộc gặp này.

Khi đã xử lý tất cả cuộc gặp, ta duyệt mảng $\textit{vis}$, thêm ID của tất cả những người biết bí mật vào danh sách kết quả rồi trả về danh sách đó.

Độ phức tạp thời gian là $O(m \times \log m + n)$, độ phức tạp không gian là $O(n)$, trong đó $m$ và $n$ lần lượt là số cuộc gặp và số người.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findAllPeople(
        self, n: int, meetings: List[List[int]], firstPerson: int
    ) -> List[int]:
        vis = [False] * n
        vis[0] = vis[firstPerson] = True
        meetings.sort(key=lambda x: x[2])
        i, m = 0, len(meetings)
        while i < m:
            j = i
            while j + 1 < m and meetings[j + 1][2] == meetings[i][2]:
                j += 1
            s = set()
            g = defaultdict(list)
            for x, y, _ in meetings[i : j + 1]:
                g[x].append(y)
                g[y].append(x)
                s.update([x, y])
            q = deque([u for u in s if vis[u]])
            while q:
                u = q.popleft()
                for v in g[u]:
                    if not vis[v]:
                        vis[v] = True
                        q.append(v)
            i = j + 1
        return [i for i, v in enumerate(vis) if v]
```

#### Java

```java
class Solution {
    public List<Integer> findAllPeople(int n, int[][] meetings, int firstPerson) {
        boolean[] vis = new boolean[n];
        vis[0] = true;
        vis[firstPerson] = true;
        int m = meetings.length;
        Arrays.sort(meetings, Comparator.comparingInt(a -> a[2]));
        for (int i = 0; i < m;) {
            int j = i;
            for (; j + 1 < m && meetings[j + 1][2] == meetings[i][2];) {
                ++j;
            }
            Map<Integer, List<Integer>> g = new HashMap<>();
            Set<Integer> s = new HashSet<>();
            for (int k = i; k <= j; ++k) {
                int x = meetings[k][0], y = meetings[k][1];
                g.computeIfAbsent(x, key -> new ArrayList<>()).add(y);
                g.computeIfAbsent(y, key -> new ArrayList<>()).add(x);
                s.add(x);
                s.add(y);
            }
            Deque<Integer> q = new ArrayDeque<>();
            for (int u : s) {
                if (vis[u]) {
                    q.offer(u);
                }
            }
            while (!q.isEmpty()) {
                int u = q.poll();
                for (int v : g.getOrDefault(u, List.of())) {
                    if (!vis[v]) {
                        vis[v] = true;
                        q.offer(v);
                    }
                }
            }
            i = j + 1;
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            if (vis[i]) {
                ans.add(i);
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
    vector<int> findAllPeople(int n, vector<vector<int>>& meetings, int firstPerson) {
        vector<bool> vis(n);
        vis[0] = vis[firstPerson] = true;
        sort(meetings.begin(), meetings.end(), [&](const auto& x, const auto& y) {
            return x[2] < y[2];
        });
        for (int i = 0, m = meetings.size(); i < m;) {
            int j = i;
            for (; j + 1 < m && meetings[j + 1][2] == meetings[i][2];) {
                ++j;
            }
            unordered_map<int, vector<int>> g;
            unordered_set<int> s;
            for (int k = i; k <= j; ++k) {
                int x = meetings[k][0], y = meetings[k][1];
                g[x].push_back(y);
                g[y].push_back(x);
                s.insert(x);
                s.insert(y);
            }
            queue<int> q;
            for (int u : s) {
                if (vis[u]) {
                    q.push(u);
                }
            }
            while (!q.empty()) {
                int u = q.front();
                q.pop();
                for (int v : g[u]) {
                    if (!vis[v]) {
                        vis[v] = true;
                        q.push(v);
                    }
                }
            }
            i = j + 1;
        }
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            if (vis[i]) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findAllPeople(n int, meetings [][]int, firstPerson int) []int {
	vis := make([]bool, n)
	vis[0], vis[firstPerson] = true, true
	sort.Slice(meetings, func(i, j int) bool {
		return meetings[i][2] < meetings[j][2]
	})
	for i, j, m := 0, 0, len(meetings); i < m; i = j + 1 {
		j = i
		for j+1 < m && meetings[j+1][2] == meetings[i][2] {
			j++
		}
		g := map[int][]int{}
		s := map[int]bool{}
		for _, e := range meetings[i : j+1] {
			x, y := e[0], e[1]
			g[x] = append(g[x], y)
			g[y] = append(g[y], x)
			s[x], s[y] = true, true
		}
		q := []int{}
		for u := range s {
			if vis[u] {
				q = append(q, u)
			}
		}
		for len(q) > 0 {
			u := q[0]
			q = q[1:]
			for _, v := range g[u] {
				if !vis[v] {
					vis[v] = true
					q = append(q, v)
				}
			}
		}
	}
	var ans []int
	for i, v := range vis {
		if v {
			ans = append(ans, i)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findAllPeople(n: number, meetings: number[][], firstPerson: number): number[] {
    const vis: boolean[] = Array(n).fill(false);
    vis[0] = true;
    vis[firstPerson] = true;

    meetings.sort((x, y) => x[2] - y[2]);

    for (let i = 0, m = meetings.length; i < m;) {
        let j = i;
        while (j + 1 < m && meetings[j + 1][2] === meetings[i][2]) {
            ++j;
        }

        const g = new Map<number, number[]>();
        const s = new Set<number>();

        for (let k = i; k <= j; ++k) {
            const x = meetings[k][0];
            const y = meetings[k][1];

            if (!g.has(x)) g.set(x, []);
            if (!g.has(y)) g.set(y, []);

            g.get(x)!.push(y);
            g.get(y)!.push(x);

            s.add(x);
            s.add(y);
        }

        const q: number[] = [];
        for (const u of s) {
            if (vis[u]) {
                q.push(u);
            }
        }

        for (const u of q) {
            for (const v of g.get(u)!) {
                if (!vis[v]) {
                    vis[v] = true;
                    q.push(v);
                }
            }
        }

        i = j + 1;
    }

    const ans: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (vis[i]) {
            ans.push(i);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
