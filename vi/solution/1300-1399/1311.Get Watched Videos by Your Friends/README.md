---
comments: true
difficulty: Medium
rating: 1652
source: Weekly Contest 170 Q3
tags:
    - Breadth-First Search
    - Graph
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [1311. Get Watched Videos by Your Friends](https://leetcode.com/problems/get-watched-videos-by-your-friends)

[中文文档](/solution/1300-1399/1311.Get%20Watched%20Videos%20by%20Your%20Friends/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> người, mỗi người có một <em>id</em> riêng trong khoảng từ <code>0</code> đến <code>n-1</code>. Cho hai mảng <code>watchedVideos</code> và <code>friends</code>, trong đó <code>watchedVideos[i]</code> và <code>friends[i]</code> lần lượt chứa danh sách video người có <code>id = i</code> đã xem và danh sách bạn bè của người đó.</p>

<p>Level video <strong>1</strong> gồm tất cả video bạn bè của bạn đã xem; level <strong>2</strong> gồm video bạn bè của bạn bè đã xem, v.v. Tổng quát, video ở level <code>k</code> là các video được xem bởi những người có khoảng cách đường đi ngắn nhất đến bạn <strong>đúng bằng</strong> <code>k</code>. Cho <code>id</code> của bạn và <code>level</code> cần xét, hãy trả về danh sách video được sắp xếp theo tần suất tăng dần. Nếu các video có cùng tần suất, hãy sắp xếp theo thứ tự alphabet từ nhỏ đến lớn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1311.Get%20Watched%20Videos%20by%20Your%20Friends/images/leetcode_friends_1.png" style="width: 144px; height: 200px;" /></strong></p>

<pre>
<strong>Input:</strong> watchedVideos = [[&quot;A&quot;,&quot;B&quot;],[&quot;C&quot;],[&quot;B&quot;,&quot;C&quot;],[&quot;D&quot;]], friends = [[1,2],[0,3],[0,3],[1,2]], id = 0, level = 1
<strong>Output:</strong> [&quot;B&quot;,&quot;C&quot;] 
<strong>Giải thích:</strong> 
Bạn có id = 0 (màu xanh lá trong hình), còn bạn bè của bạn được tô màu vàng:
Người có id = 1 -&gt; watchedVideos = [&quot;C&quot;]&nbsp;
Người có id = 2 -&gt; watchedVideos = [&quot;B&quot;,&quot;C&quot;]&nbsp;
Tần suất video mà bạn bè của bạn đã xem là:&nbsp;
B -&gt; 1&nbsp;
C -&gt; 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1311.Get%20Watched%20Videos%20by%20Your%20Friends/images/leetcode_friends_2.png" style="width: 144px; height: 200px;" /></strong></p>

<pre>
<strong>Input:</strong> watchedVideos = [[&quot;A&quot;,&quot;B&quot;],[&quot;C&quot;],[&quot;B&quot;,&quot;C&quot;],[&quot;D&quot;]], friends = [[1,2],[0,3],[0,3],[1,2]], id = 0, level = 2
<strong>Output:</strong> [&quot;D&quot;]
<strong>Giải thích:</strong> 
Bạn có id = 0 (màu xanh lá trong hình), và người bạn duy nhất của bạn bè bạn là người có id = 3 (màu vàng trong hình).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == watchedVideos.length ==&nbsp;friends.length</code></li>
	<li><code>2 &lt;= n&nbsp;&lt;= 100</code></li>
	<li><code>1 &lt;=&nbsp;watchedVideos[i].length &lt;= 100</code></li>
	<li><code>1 &lt;=&nbsp;watchedVideos[i][j].length &lt;= 8</code></li>
	<li><code>0 &lt;= friends[i].length &lt; n</code></li>
	<li><code>0 &lt;= friends[i][j]&nbsp;&lt; n</code></li>
	<li><code>0 &lt;= id &lt; n</code></li>
	<li><code>1 &lt;= level &lt; n</code></li>
	<li>Nếu&nbsp;<code>friends[i]</code> chứa <code>j</code>, thì <code>friends[j]</code> cũng chứa <code>i</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm video được xem bởi những người cách đúng $\textit{level}$ bước, rồi sắp xếp theo tần suất và tên. DFS không giới hạn sẽ không dừng ở đúng một level và có thể thăm lại người khác. Thực hiện BFS $\textit{level}$ lượt từ $\textit{id}$ sẽ để lại đúng level cần tìm trong queue.
>
> Ta đếm số lần xuất hiện của video; sắp xếp theo $(\textit{cnt}[v], v)$ sẽ cho thứ tự tên cần trả về. Set visited đảm bảo mỗi người chỉ được đưa vào queue một lần.

<!-- thinking:end -->

Ta có thể dùng Breadth-First Search (BFS) bắt đầu từ $\textit{id}$ để tìm tất cả bạn bè ở khoảng cách $\textit{level}$, rồi đếm các video họ đã xem.

Cụ thể, ta dùng queue $\textit{q}$ để lưu bạn bè ở level hiện tại. Ban đầu, thêm $\textit{id}$ vào queue $\textit{q}$. Dùng hash table hoặc mảng boolean $\textit{vis}$ để đánh dấu những người đã thăm. Sau đó lặp $\textit{level}$ lần; mỗi lần lấy hết người trong queue ra và thêm bạn bè của họ vào queue, cho đến khi tìm được tất cả người ở khoảng cách $\textit{level}$.

Tiếp theo, ta dùng hash table $\textit{cnt}$ để đếm video được những người này xem và tần suất tương ứng. Cuối cùng, sắp xếp các cặp key-value trong hash table theo tần suất tăng dần; nếu tần suất bằng nhau thì sắp xếp theo tên video tăng dần. Trả về danh sách tên video đã sắp xếp.

Độ phức tạp thời gian là $O(n + m + v \times \log v)$ và độ phức tạp không gian là $O(n + v)$. Trong đó, $n$ và $m$ lần lượt là độ dài các mảng $\textit{watchedVideos}$ và $\textit{friends}$, còn $v$ là tổng số video được tất cả bạn bè xem.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def watchedVideosByFriends(
        self,
        watchedVideos: List[List[str]],
        friends: List[List[int]],
        id: int,
        level: int,
    ) -> List[str]:
        q = deque([id])
        vis = {id}
        for _ in range(level):
            for _ in range(len(q)):
                i = q.popleft()
                for j in friends[i]:
                    if j not in vis:
                        vis.add(j)
                        q.append(j)
        cnt = Counter()
        for i in q:
            for v in watchedVideos[i]:
                cnt[v] += 1
        return sorted(cnt.keys(), key=lambda k: (cnt[k], k))
```

#### Java

```java
class Solution {
    public List<String> watchedVideosByFriends(
        List<List<String>> watchedVideos, int[][] friends, int id, int level) {
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(id);
        int n = friends.length;
        boolean[] vis = new boolean[n];
        vis[id] = true;
        while (level-- > 0) {
            for (int k = q.size(); k > 0; --k) {
                int i = q.poll();
                for (int j : friends[i]) {
                    if (!vis[j]) {
                        vis[j] = true;
                        q.offer(j);
                    }
                }
            }
        }
        Map<String, Integer> cnt = new HashMap<>();
        for (int i : q) {
            for (var v : watchedVideos.get(i)) {
                cnt.merge(v, 1, Integer::sum);
            }
        }
        List<String> ans = new ArrayList<>(cnt.keySet());
        ans.sort((a, b) -> {
            int x = cnt.get(a), y = cnt.get(b);
            return x == y ? a.compareTo(b) : Integer.compare(x, y);
        });
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> watchedVideosByFriends(vector<vector<string>>& watchedVideos, vector<vector<int>>& friends, int id, int level) {
        queue<int> q{{id}};
        int n = friends.size();
        vector<bool> vis(n);
        vis[id] = true;
        while (level--) {
            for (int k = q.size(); k; --k) {
                int i = q.front();
                q.pop();
                for (int j : friends[i]) {
                    if (!vis[j]) {
                        vis[j] = true;
                        q.push(j);
                    }
                }
            }
        }
        unordered_map<string, int> cnt;
        while (!q.empty()) {
            int i = q.front();
            q.pop();
            for (const auto& v : watchedVideos[i]) {
                cnt[v]++;
            }
        }
        vector<string> ans;
        for (const auto& [key, _] : cnt) {
            ans.push_back(key);
        }
        sort(ans.begin(), ans.end(), [&cnt](const string& a, const string& b) {
            return cnt[a] == cnt[b] ? a < b : cnt[a] < cnt[b];
        });
        return ans;
    }
};
```

#### Go

```go
func watchedVideosByFriends(watchedVideos [][]string, friends [][]int, id int, level int) []string {
	q := []int{id}
	n := len(friends)
	vis := make([]bool, n)
	vis[id] = true
	for level > 0 {
		level--
		nextQ := []int{}
		for _, i := range q {
			for _, j := range friends[i] {
				if !vis[j] {
					vis[j] = true
					nextQ = append(nextQ, j)
				}
			}
		}
		q = nextQ
	}
	cnt := make(map[string]int)
	for _, i := range q {
		for _, v := range watchedVideos[i] {
			cnt[v]++
		}
	}
	ans := []string{}
	for key := range cnt {
		ans = append(ans, key)
	}
	sort.Slice(ans, func(i, j int) bool {
		if cnt[ans[i]] == cnt[ans[j]] {
			return ans[i] < ans[j]
		}
		return cnt[ans[i]] < cnt[ans[j]]
	})
	return ans
}
```

#### TypeScript

```ts
function watchedVideosByFriends(
    watchedVideos: string[][],
    friends: number[][],
    id: number,
    level: number,
): string[] {
    let q: number[] = [id];
    const n: number = friends.length;
    const vis: boolean[] = Array(n).fill(false);
    vis[id] = true;
    while (level-- > 0) {
        const nq: number[] = [];
        for (const i of q) {
            for (const j of friends[i]) {
                if (!vis[j]) {
                    vis[j] = true;
                    nq.push(j);
                }
            }
        }
        q = nq;
    }
    const cnt: { [key: string]: number } = {};
    for (const i of q) {
        for (const v of watchedVideos[i]) {
            cnt[v] = (cnt[v] || 0) + 1;
        }
    }
    const ans: string[] = Object.keys(cnt);
    ans.sort((a, b) => {
        if (cnt[a] === cnt[b]) {
            return a.localeCompare(b);
        }
        return cnt[a] - cnt[b];
    });
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
