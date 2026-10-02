---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Hash Table
---

<!-- problem:start -->

# [582. Kill Process 🔒](https://leetcode.com/problems/kill-process)

[中文文档](/solution/0500-0599/0582.Kill%20Process/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code> process tạo thành cấu trúc cây có gốc. Cho hai mảng số nguyên <code>pid</code> và <code>ppid</code>, trong đó <code>pid[i]</code> là ID của process thứ <code>i</code>, còn <code>ppid[i]</code> là ID của process cha của process thứ <code>i</code>.</p>

<p>Mỗi process chỉ có <strong>một process cha</strong> nhưng có thể có nhiều process con. Chỉ một process có <code>ppid[i] = 0</code>, nghĩa là process này <strong>không có process cha</strong> và là gốc của cây.</p>

<p>Khi một process bị <strong>kill</strong>, tất cả process con của nó cũng sẽ bị kill.</p>

<p>Cho số nguyên <code>kill</code> là ID của process bạn muốn kill, hãy trả về <em>danh sách ID của các process sẽ bị kill. Có thể trả về kết quả theo <strong>bất kỳ thứ tự nào</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0582.Kill%20Process/images/ptree.jpg" style="width: 207px; height: 302px;" />
<pre>
<strong>Đầu vào:</strong> pid = [1,3,10,5], ppid = [3,0,5,3], kill = 5
<strong>Đầu ra:</strong> [5,10]
<strong>Giải thích:</strong>&nbsp;Các process được tô đỏ là những process cần bị kill.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> pid = [1], ppid = [0], kill = 1
<strong>Đầu ra:</strong> [1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == pid.length</code></li>
	<li><code>n == ppid.length</code></li>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= pid[i] &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= ppid[i] &lt;= 5 * 10<sup>4</sup></code></li>
	<li>Chỉ có một process không có process cha.</li>
	<li>Mọi giá trị trong <code>pid</code> đều <strong>khác nhau</strong>.</li>
	<li>Đảm bảo <code>kill</code> có trong <code>pid</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Kill một process sẽ kill toàn bộ cây hậu duệ của nó. Trước tiên cần chuyển thông tin cha-con thành adjacency list.
>
> $g[p]$ lưu các process con của $p$. Chạy DFS hoặc BFS từ `kill` để thu thập mọi ID có thể đi tới. Mỗi process chỉ được thăm một lần.

<!-- thinking:end -->

Đầu tiên, ta xây dựng graph $g$ từ $pid$ và $ppid$, trong đó $g[i]$ chứa tất cả process con của process $i$. Sau đó, bắt đầu từ process $kill$ và thực hiện DFS để tìm tất cả process sẽ bị kill.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số process.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def killProcess(self, pid: List[int], ppid: List[int], kill: int) -> List[int]:
        def dfs(i: int):
            ans.append(i)
            for j in g[i]:
                dfs(j)

        g = defaultdict(list)
        for i, p in zip(pid, ppid):
            g[p].append(i)
        ans = []
        dfs(kill)
        return ans
```

#### Java

```java
class Solution {
    private Map<Integer, List<Integer>> g = new HashMap<>();
    private List<Integer> ans = new ArrayList<>();

    public List<Integer> killProcess(List<Integer> pid, List<Integer> ppid, int kill) {
        int n = pid.size();
        for (int i = 0; i < n; ++i) {
            g.computeIfAbsent(ppid.get(i), k -> new ArrayList<>()).add(pid.get(i));
        }
        dfs(kill);
        return ans;
    }

    private void dfs(int i) {
        ans.add(i);
        for (int j : g.getOrDefault(i, Collections.emptyList())) {
            dfs(j);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> killProcess(vector<int>& pid, vector<int>& ppid, int kill) {
        unordered_map<int, vector<int>> g;
        int n = pid.size();
        for (int i = 0; i < n; ++i) {
            g[ppid[i]].push_back(pid[i]);
        }
        vector<int> ans;
        function<void(int)> dfs = [&](int i) {
            ans.push_back(i);
            for (int j : g[i]) {
                dfs(j);
            }
        };
        dfs(kill);
        return ans;
    }
};
```

#### Go

```go
func killProcess(pid []int, ppid []int, kill int) (ans []int) {
	g := map[int][]int{}
	for i, p := range ppid {
		g[p] = append(g[p], pid[i])
	}
	var dfs func(int)
	dfs = func(i int) {
		ans = append(ans, i)
		for _, j := range g[i] {
			dfs(j)
		}
	}
	dfs(kill)
	return
}
```

#### TypeScript

```ts
function killProcess(pid: number[], ppid: number[], kill: number): number[] {
    const g: Map<number, number[]> = new Map();
    for (let i = 0; i < pid.length; ++i) {
        if (!g.has(ppid[i])) {
            g.set(ppid[i], []);
        }
        g.get(ppid[i])?.push(pid[i]);
    }
    const ans: number[] = [];
    const dfs = (i: number) => {
        ans.push(i);
        for (const j of g.get(i) ?? []) {
            dfs(j);
        }
    };
    dfs(kill);
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn kill_process(pid: Vec<i32>, ppid: Vec<i32>, kill: i32) -> Vec<i32> {
        let mut g: HashMap<i32, Vec<i32>> = HashMap::new();
        let mut ans: Vec<i32> = Vec::new();

        let n = pid.len();
        for i in 0..n {
            g.entry(ppid[i]).or_insert(Vec::new()).push(pid[i]);
        }

        Self::dfs(&mut ans, &g, kill);
        ans
    }

    fn dfs(ans: &mut Vec<i32>, g: &HashMap<i32, Vec<i32>>, i: i32) {
        ans.push(i);
        if let Some(children) = g.get(&i) {
            for &j in children {
                Self::dfs(ans, g, j);
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
