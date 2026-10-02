---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
---

<!-- problem:start -->

# [841. Keys and Rooms](https://leetcode.com/problems/keys-and-rooms)

[中文文档](/solution/0800-0899/0841.Keys%20and%20Rooms/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> căn phòng được đánh số từ <code>0</code> đến <code>n - 1</code>&nbsp;và tất cả đều bị khóa ngoại trừ phòng <code>0</code>. Mục tiêu của bạn là thăm tất cả các phòng. Tuy nhiên, bạn không thể vào phòng đang khóa nếu không có chìa khóa của phòng đó.</p>

<p>Khi vào một phòng, bạn có thể tìm thấy một tập hợp các chìa khóa <strong>khác nhau</strong>. Trên mỗi chìa khóa có ghi số phòng mà nó mở được; bạn có thể mang theo tất cả chìa khóa để mở các phòng khác.</p>

<p>Cho mảng <code>rooms</code>, trong đó <code>rooms[i]</code> là tập chìa khóa bạn có thể lấy được khi thăm phòng <code>i</code>. Hãy trả về <code>true</code> <em>nếu bạn có thể thăm <strong>tất cả</strong> các phòng, nếu không thì trả về</em> <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> rooms = [[1],[2],[3],[]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 
Ta thăm phòng 0 và lấy chìa khóa 1.
Sau đó, ta thăm phòng 1 và lấy chìa khóa 2.
Tiếp theo, ta thăm phòng 2 và lấy chìa khóa 3.
Cuối cùng, ta thăm phòng 3.
Vì có thể thăm mọi phòng nên ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> rooms = [[1,3],[3,0,1],[2],[0]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Ta không thể vào phòng số 2 vì chìa khóa duy nhất mở được phòng đó nằm ngay trong phòng ấy.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == rooms.length</code></li>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>0 &lt;= rooms[i].length &lt;= 1000</code></li>
	<li><code>1 &lt;= sum(rooms[i].length) &lt;= 3000</code></li>
	<li><code>0 &lt;= rooms[i][j] &lt; n</code></li>
	<li>Tất cả giá trị trong <code>rooms[i]</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm theo chiều sâu (DFS)

<!-- thinking:start -->

> **Tư duy**
>
> Các phòng tạo thành một đồ thị có hướng, trong đó chìa khóa là các cạnh. Ta cần xác định liệu phòng $0$ có thể đi đến mọi phòng hay không. Vì $n\le 1000$, chỉ cần duyệt đồ thị một lần.
>
> Chạy DFS từ $0$ theo các chìa khóa; thành công khi $|\textit{vis}|=n$.

<!-- thinking:end -->

Ta có thể dùng tìm kiếm theo chiều sâu (DFS) để duyệt toàn bộ đồ thị, đếm số node có thể đến được và dùng mảng `vis` đánh dấu node đã thăm để tránh duyệt lặp.

Cuối cùng, ta đếm số node đã thăm. Nếu số này bằng tổng số node thì có thể thăm tất cả; nếu không, vẫn có node không thể đến được.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node và $m$ là số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canVisitAllRooms(self, rooms: List[List[int]]) -> bool:
        def dfs(i: int):
            if i in vis:
                return
            vis.add(i)
            for j in rooms[i]:
                dfs(j)

        vis = set()
        dfs(0)
        return len(vis) == len(rooms)
```

#### Java

```java
class Solution {
    private int cnt;
    private boolean[] vis;
    private List<List<Integer>> g;

    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        g = rooms;
        vis = new boolean[g.size()];
        dfs(0);
        return cnt == g.size();
    }

    private void dfs(int i) {
        if (vis[i]) {
            return;
        }
        vis[i] = true;
        ++cnt;
        for (int j : g.get(i)) {
            dfs(j);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        int n = rooms.size();
        int cnt = 0;
        bool vis[n];
        memset(vis, false, sizeof(vis));
        function<void(int)> dfs = [&](int i) {
            if (vis[i]) {
                return;
            }
            vis[i] = true;
            ++cnt;
            for (int j : rooms[i]) {
                dfs(j);
            }
        };
        dfs(0);
        return cnt == n;
    }
};
```

#### Go

```go
func canVisitAllRooms(rooms [][]int) bool {
	n := len(rooms)
	cnt := 0
	vis := make([]bool, n)
	var dfs func(int)
	dfs = func(i int) {
		if vis[i] {
			return
		}
		vis[i] = true
		cnt++
		for _, j := range rooms[i] {
			dfs(j)
		}
	}
	dfs(0)
	return cnt == n
}
```

#### TypeScript

```ts
function canVisitAllRooms(rooms: number[][]): boolean {
    const n = rooms.length;
    const vis: boolean[] = Array(n).fill(false);
    const dfs = (i: number) => {
        if (vis[i]) {
            return;
        }
        vis[i] = true;
        for (const j of rooms[i]) {
            dfs(j);
        }
    };
    dfs(0);
    return vis.every(v => v);
}
```

#### Rust

```rust
impl Solution {
    pub fn can_visit_all_rooms(rooms: Vec<Vec<i32>>) -> bool {
        let n = rooms.len();
        let mut is_open = vec![false; n];
        let mut keys = vec![0];
        while !keys.is_empty() {
            let i = keys.pop().unwrap();
            if is_open[i] {
                continue;
            }
            is_open[i] = true;
            rooms[i].iter().for_each(|&key| keys.push(key as usize));
        }
        is_open.iter().all(|&v| v)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể xác định khả năng đi đến các phòng tương tự bằng queue: chạy BFS từ $0$ và đưa vào queue những phòng được mở bằng chìa khóa hiện có.
>
> Set lưu các phòng đã thăm vẫn giúp tránh lặp. Khác biệt duy nhất so với DFS là dùng queue tường minh.

<!-- thinking:end -->

Ta cũng có thể dùng tìm kiếm theo chiều rộng (BFS) để duyệt toàn bộ đồ thị. Dùng hash table hoặc mảng `vis` để đánh dấu node đã thăm, tránh duyệt lặp.

Cụ thể, ta định nghĩa queue $q$, ban đầu đưa node $0$ vào queue rồi liên tục duyệt queue. Mỗi lần lấy node đầu $i$ ra, nếu $i$ đã được thăm thì bỏ qua; nếu chưa, đánh dấu nó đã thăm rồi đưa các node mà $i$ có thể đi đến vào queue.

Cuối cùng, ta đếm số node đã thăm. Nếu số này bằng tổng số node thì có thể thăm tất cả; nếu không, vẫn còn node không thể đến được.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node và $m$ là số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canVisitAllRooms(self, rooms: List[List[int]]) -> bool:
        vis = set()
        q = deque([0])
        while q:
            i = q.popleft()
            if i in vis:
                continue
            vis.add(i)
            q.extend(j for j in rooms[i])
        return len(vis) == len(rooms)
```

#### Java

```java
class Solution {
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        int n = rooms.size();
        boolean[] vis = new boolean[n];
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(0);
        int cnt = 0;
        while (!q.isEmpty()) {
            int i = q.poll();
            if (vis[i]) {
                continue;
            }
            vis[i] = true;
            ++cnt;
            for (int j : rooms.get(i)) {
                q.offer(j);
            }
        }
        return cnt == n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        int n = rooms.size();
        vector<bool> vis(n);
        queue<int> q{{0}};
        int cnt = 0;
        while (q.size()) {
            int i = q.front();
            q.pop();
            if (vis[i]) {
                continue;
            }
            vis[i] = true;
            ++cnt;
            for (int j : rooms[i]) {
                q.push(j);
            }
        }
        return cnt == n;
    }
};
```

#### Go

```go
func canVisitAllRooms(rooms [][]int) bool {
	n := len(rooms)
	vis := make([]bool, n)
	cnt := 0
	q := []int{0}
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		if vis[i] {
			continue
		}
		vis[i] = true
		cnt++
		for _, j := range rooms[i] {
			q = append(q, j)
		}
	}
	return cnt == n
}
```

#### TypeScript

```ts
function canVisitAllRooms(rooms: number[][]): boolean {
    const vis = new Set<number>();
    const q: number[] = [0];

    while (q.length) {
        const i = q.pop()!;
        if (vis.has(i)) {
            continue;
        }
        vis.add(i);
        q.push(...rooms[i]);
    }

    return vis.size == rooms.length;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
