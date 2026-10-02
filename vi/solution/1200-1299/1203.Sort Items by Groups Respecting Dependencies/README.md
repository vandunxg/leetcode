---
comments: true
difficulty: Hard
rating: 2418
source: Weekly Contest 155 Q4
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
    - Directed Acyclic Graph
---

<!-- problem:start -->

# [1203. Sort Items by Groups Respecting Dependencies](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies)

[中文文档](/solution/1200-1299/1203.Sort%20Items%20by%20Groups%20Respecting%20Dependencies/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> item, mỗi item thuộc không hoặc một trong <code>m</code> group. <code>group[i]</code> là group của item thứ <code>i</code>; giá trị này bằng <code>-1</code> nếu item thứ <code>i</code> không thuộc group nào. Item và group được đánh chỉ số từ 0. Một group có thể không chứa item nào.</p>

<p>Hãy trả về danh sách item đã sắp xếp sao cho:</p>

<ul>
	<li>Các item thuộc cùng một group nằm liền kề nhau trong danh sách đã sắp xếp.</li>
	<li>Các item có một số quan hệ thứ tự; <code>beforeItems[i]</code> là danh sách chứa mọi item phải đứng trước item thứ <code>i</code> trong mảng đã sắp xếp (ở bên trái item thứ <code>i</code>).</li>
</ul>

<p>Nếu có nhiều lời giải, hãy trả về một lời giải bất kỳ; nếu không có lời giải, trả về <strong>danh sách rỗng</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1203.Sort%20Items%20by%20Groups%20Respecting%20Dependencies/images/1359_ex1.png" style="width: 191px; height: 181px;" /></strong></p>

<pre>
<strong>Input:</strong> n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = [[],[6],[5],[6],[3,6],[],[],[]]
<strong>Output:</strong> [6,3,4,1,5,2,0,7]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = [[],[6],[5],[6],[3],[],[4],[]]
<strong>Output:</strong> []
<strong>Giải thích:</strong>&nbsp;Ví dụ này giống ví dụ 1, ngoại trừ việc 4 phải đứng trước 6 trong danh sách đã sắp xếp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m &lt;= n &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>group.length == beforeItems.length == n</code></li>
	<li><code>-1 &lt;= group[i] &lt;= m - 1</code></li>
	<li><code>0 &lt;= beforeItems[i].length &lt;= n - 1</code></li>
	<li><code>0 &lt;= beforeItems[i][j] &lt;= n - 1</code></li>
	<li><code>i != beforeItems[i][j]</code></li>
	<li><code>beforeItems[i]&nbsp;</code>không chứa phần tử trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp topo

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ sắp xếp topo các item có thể làm một group bị tách ra, khiến các item cùng group không còn liền kề. Với $n \le 3\times 10^4$, cần xây dựng graph trong thời gian tuyến tính.
>
> Các ràng buộc có hai tầng: thứ tự giữa các group và thứ tự giữa các item trong cùng group. Mỗi item chưa thuộc group nào được đưa vào một group riêng. Dependency giữa các group tạo thành cạnh trong graph group; dependency trong cùng group tạo thành cạnh trong graph item.
>
> Trước tiên, sắp xếp topo các group, sau đó lần lượt sắp xếp topo item bên trong từng group theo thứ tự đó. Nếu một trong hai tầng có cycle thì không có đáp án. Hai graph tách riêng yêu cầu “các item cùng group nằm liền nhau” và “thứ tự item”; cả hai đều được xử lý bằng thuật toán Kahn.

<!-- thinking:end -->

Trước tiên, duyệt mảng $group$. Với mỗi item chưa thuộc group nào, tạo một group mới có ID $m$ cho item đó rồi tăng $m$ lên. Như vậy, mọi item đều thuộc một group. Sau đó, dùng mảng $groupItems$ để lưu các item thuộc từng group. Chỉ số của mảng là ID group, còn giá trị là danh sách item trong group đó.

Tiếp theo, xây dựng graph. Với mỗi item, ta cần hai graph: một cho các item và một cho các group. Duyệt mảng $group$. Với item hiện tại $i$, group của nó là $group[i]$. Duyệt $beforeItems[i]$; với mỗi item $j$ trong danh sách, nếu $group[i] = group[j]$ thì $i$ và $j$ thuộc cùng group, nên thêm cạnh $j \to i$ vào graph item. Nếu không, chúng thuộc các group khác nhau; thêm cạnh $group[j] \to group[i]$ vào graph group và cập nhật mảng bậc vào tương ứng.

Sau đó, sắp xếp topo graph group để thu được danh sách group đã sắp xếp $groupOrder$. Nếu danh sách này không có đủ tất cả group thì không thể hoàn thành việc sắp xếp, nên trả về danh sách rỗng. Nếu có, duyệt $groupOrder$; với mỗi group $gi$, sắp xếp topo danh sách item $groupItems[gi]$ để thu được danh sách đã sắp xếp $itemOrder$. Nếu danh sách này không có đủ item của group thì không thể hoàn thành việc sắp xếp, nên trả về danh sách rỗng. Nếu có, thêm các item trong $itemOrder$ vào mảng kết quả, rồi trả về mảng này.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là tổng số item và group.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortItems(
        self, n: int, m: int, group: List[int], beforeItems: List[List[int]]
    ) -> List[int]:
        def topo_sort(degree, graph, items):
            q = deque(i for _, i in enumerate(items) if degree[i] == 0)
            res = []
            while q:
                i = q.popleft()
                res.append(i)
                for j in graph[i]:
                    degree[j] -= 1
                    if degree[j] == 0:
                        q.append(j)
            return res if len(res) == len(items) else []

        idx = m
        group_items = [[] for _ in range(n + m)]
        for i, g in enumerate(group):
            if g == -1:
                group[i] = idx
                idx += 1
            group_items[group[i]].append(i)

        item_degree = [0] * n
        group_degree = [0] * (n + m)
        item_graph = [[] for _ in range(n)]
        group_graph = [[] for _ in range(n + m)]
        for i, gi in enumerate(group):
            for j in beforeItems[i]:
                gj = group[j]
                if gi == gj:
                    item_degree[i] += 1
                    item_graph[j].append(i)
                else:
                    group_degree[gi] += 1
                    group_graph[gj].append(gi)

        group_order = topo_sort(group_degree, group_graph, range(n + m))
        if not group_order:
            return []
        ans = []
        for gi in group_order:
            items = group_items[gi]
            item_order = topo_sort(item_degree, item_graph, items)
            if len(items) != len(item_order):
                return []
            ans.extend(item_order)
        return ans
```

#### Java

```java
class Solution {
    public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {
        int idx = m;
        List<Integer>[] groupItems = new List[n + m];
        int[] itemDegree = new int[n];
        int[] groupDegree = new int[n + m];
        List<Integer>[] itemGraph = new List[n];
        List<Integer>[] groupGraph = new List[n + m];
        Arrays.setAll(groupItems, k -> new ArrayList<>());
        Arrays.setAll(itemGraph, k -> new ArrayList<>());
        Arrays.setAll(groupGraph, k -> new ArrayList<>());
        for (int i = 0; i < n; ++i) {
            if (group[i] == -1) {
                group[i] = idx++;
            }
            groupItems[group[i]].add(i);
        }
        for (int i = 0; i < n; ++i) {
            for (int j : beforeItems.get(i)) {
                if (group[i] == group[j]) {
                    ++itemDegree[i];
                    itemGraph[j].add(i);
                } else {
                    ++groupDegree[group[i]];
                    groupGraph[group[j]].add(group[i]);
                }
            }
        }
        List<Integer> items = new ArrayList<>();
        for (int i = 0; i < n + m; ++i) {
            items.add(i);
        }
        var groupOrder = topoSort(groupDegree, groupGraph, items);
        if (groupOrder.isEmpty()) {
            return new int[0];
        }
        List<Integer> ans = new ArrayList<>();
        for (int gi : groupOrder) {
            items = groupItems[gi];
            var itemOrder = topoSort(itemDegree, itemGraph, items);
            if (itemOrder.size() != items.size()) {
                return new int[0];
            }
            ans.addAll(itemOrder);
        }
        return ans.stream().mapToInt(Integer::intValue).toArray();
    }

    private List<Integer> topoSort(int[] degree, List<Integer>[] graph, List<Integer> items) {
        Deque<Integer> q = new ArrayDeque<>();
        for (int i : items) {
            if (degree[i] == 0) {
                q.offer(i);
            }
        }
        List<Integer> ans = new ArrayList<>();
        while (!q.isEmpty()) {
            int i = q.poll();
            ans.add(i);
            for (int j : graph[i]) {
                if (--degree[j] == 0) {
                    q.offer(j);
                }
            }
        }
        return ans.size() == items.size() ? ans : List.of();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> sortItems(int n, int m, vector<int>& group, vector<vector<int>>& beforeItems) {
        int idx = m;
        vector<vector<int>> groupItems(n + m);
        vector<int> itemDegree(n);
        vector<int> groupDegree(n + m);
        vector<vector<int>> itemGraph(n);
        vector<vector<int>> groupGraph(n + m);
        for (int i = 0; i < n; ++i) {
            if (group[i] == -1) {
                group[i] = idx++;
            }
            groupItems[group[i]].push_back(i);
        }
        for (int i = 0; i < n; ++i) {
            for (int j : beforeItems[i]) {
                if (group[i] == group[j]) {
                    ++itemDegree[i];
                    itemGraph[j].push_back(i);
                } else {
                    ++groupDegree[group[i]];
                    groupGraph[group[j]].push_back(group[i]);
                }
            }
        }
        vector<int> items(n + m);
        iota(items.begin(), items.end(), 0);
        auto topoSort = [](vector<vector<int>>& graph, vector<int>& degree, vector<int>& items) -> vector<int> {
            queue<int> q;
            for (int& i : items) {
                if (degree[i] == 0) {
                    q.push(i);
                }
            }
            vector<int> ans;
            while (!q.empty()) {
                int i = q.front();
                q.pop();
                ans.push_back(i);
                for (int& j : graph[i]) {
                    if (--degree[j] == 0) {
                        q.push(j);
                    }
                }
            }
            return ans.size() == items.size() ? ans : vector<int>();
        };
        auto groupOrder = topoSort(groupGraph, groupDegree, items);
        if (groupOrder.empty()) {
            return vector<int>();
        }
        vector<int> ans;
        for (int& gi : groupOrder) {
            items = groupItems[gi];
            auto itemOrder = topoSort(itemGraph, itemDegree, items);
            if (items.size() != itemOrder.size()) {
                return vector<int>();
            }
            ans.insert(ans.end(), itemOrder.begin(), itemOrder.end());
        }
        return ans;
    }
};
```

#### Go

```go
func sortItems(n int, m int, group []int, beforeItems [][]int) []int {
	idx := m
	groupItems := make([][]int, n+m)
	itemDegree := make([]int, n)
	groupDegree := make([]int, n+m)
	itemGraph := make([][]int, n)
	groupGraph := make([][]int, n+m)
	for i, g := range group {
		if g == -1 {
			group[i] = idx
			idx++
		}
		groupItems[group[i]] = append(groupItems[group[i]], i)
	}
	for i, gi := range group {
		for _, j := range beforeItems[i] {
			gj := group[j]
			if gi == gj {
				itemDegree[i]++
				itemGraph[j] = append(itemGraph[j], i)
			} else {
				groupDegree[gi]++
				groupGraph[gj] = append(groupGraph[gj], gi)
			}
		}
	}
	items := make([]int, n+m)
	for i := range items {
		items[i] = i
	}
	topoSort := func(degree []int, graph [][]int, items []int) []int {
		q := []int{}
		for _, i := range items {
			if degree[i] == 0 {
				q = append(q, i)
			}
		}
		ans := []int{}
		for len(q) > 0 {
			i := q[0]
			q = q[1:]
			ans = append(ans, i)
			for _, j := range graph[i] {
				degree[j]--
				if degree[j] == 0 {
					q = append(q, j)
				}
			}
		}
		return ans
	}
	groupOrder := topoSort(groupDegree, groupGraph, items)
	if len(groupOrder) != len(items) {
		return nil
	}
	ans := []int{}
	for _, gi := range groupOrder {
		items = groupItems[gi]
		itemOrder := topoSort(itemDegree, itemGraph, items)
		if len(items) != len(itemOrder) {
			return nil
		}
		ans = append(ans, itemOrder...)
	}
	return ans
}
```

#### TypeScript

```ts
function sortItems(n: number, m: number, group: number[], beforeItems: number[][]): number[] {
    let idx = m;
    const groupItems: number[][] = new Array(n + m).fill(0).map(() => []);
    const itemDegree: number[] = new Array(n).fill(0);
    const gorupDegree: number[] = new Array(n + m).fill(0);
    const itemGraph: number[][] = new Array(n).fill(0).map(() => []);
    const groupGraph: number[][] = new Array(n + m).fill(0).map(() => []);
    for (let i = 0; i < n; ++i) {
        if (group[i] === -1) {
            group[i] = idx++;
        }
        groupItems[group[i]].push(i);
    }
    for (let i = 0; i < n; ++i) {
        for (const j of beforeItems[i]) {
            if (group[i] === group[j]) {
                ++itemDegree[i];
                itemGraph[j].push(i);
            } else {
                ++gorupDegree[group[i]];
                groupGraph[group[j]].push(group[i]);
            }
        }
    }
    let items = new Array(n + m).fill(0).map((_, i) => i);
    const topoSort = (graph: number[][], degree: number[], items: number[]): number[] => {
        const q: number[] = [];
        for (const i of items) {
            if (degree[i] === 0) {
                q.push(i);
            }
        }
        const ans: number[] = [];
        while (q.length) {
            const i = q.pop()!;
            ans.push(i);
            for (const j of graph[i]) {
                if (--degree[j] === 0) {
                    q.push(j);
                }
            }
        }
        return ans.length === items.length ? ans : [];
    };
    const groupOrder = topoSort(groupGraph, gorupDegree, items);
    if (groupOrder.length === 0) {
        return [];
    }
    const ans: number[] = [];
    for (const gi of groupOrder) {
        items = groupItems[gi];
        const itemOrder = topoSort(itemGraph, itemDegree, items);
        if (itemOrder.length !== items.length) {
            return [];
        }
        ans.push(...itemOrder);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
