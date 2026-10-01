---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Hash Table
    - Binary Tree
    - Sorting
---

<!-- problem:start -->

# [314. Binary Tree Vertical Order Traversal 🔒](https://leetcode.com/problems/binary-tree-vertical-order-traversal)

[中文文档](/solution/0300-0399/0314.Binary%20Tree%20Vertical%20Order%20Traversal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một cây nhị phân, hãy trả về <em><strong>các giá trị của node theo thứ tự duyệt dọc</strong></em> (tức là từ trên xuống dưới, lần lượt theo từng cột).</p>

<p>Nếu hai node nằm trên cùng một hàng và cột, thứ tự phải là <strong>từ trái sang phải</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0314.Binary%20Tree%20Vertical%20Order%20Traversal/images/image1.png" style="width: 400px; height: 273px;" />
<pre>
<strong>Đầu vào:</strong> root = [3,9,20,null,null,15,7]
<strong>Đầu ra:</strong> [[9],[3,15],[20],[7]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0314.Binary%20Tree%20Vertical%20Order%20Traversal/images/image3.png" style="width: 450px; height: 285px;" />
<pre>
<strong>Đầu vào:</strong> root = [3,9,8,4,0,1,7]
<strong>Đầu ra:</strong> [[4],[9],[3,0,1],[8],[7]]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0314.Binary%20Tree%20Vertical%20Order%20Traversal/images/image2.png" style="width: 350px; height: 342px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,4,10,9,11,null,5,null,null,null,null,null,null,null,6]
<strong>Đầu ra:</strong> [[4],[2,5],[1,10,9,6],[3],[11]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[0, 100]</code>.</li>
	<li><code>-100 &lt;= Node.val &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Thứ tự dọc liệt kê các cột từ trái sang phải, và trong mỗi cột là từ trên xuống dưới. DFS không duyệt theo từng level, nên có thể gặp node ở sâu trước.
>
> Ghi lại độ sâu và độ lệch ngang của mỗi node, nhóm node theo cột rồi sắp xếp từng nhóm theo độ sâu. Sắp xếp các key của cột sẽ cho thứ tự từ trái sang phải.

<!-- thinking:end -->

DFS duyệt cây nhị phân và ghi lại giá trị, độ sâu cùng độ lệch ngang của mỗi node. Sau đó, sắp xếp tất cả node theo độ lệch ngang tăng dần, rồi theo độ sâu tăng dần, cuối cùng nhóm chúng theo độ lệch ngang.

Độ phức tạp thời gian là $O(n\log \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây nhị phân.

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def verticalOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        def dfs(root, depth, offset):
            if root is None:
                return
            d[offset].append((depth, root.val))
            dfs(root.left, depth + 1, offset - 1)
            dfs(root.right, depth + 1, offset + 1)

        d = defaultdict(list)
        dfs(root, 0, 0)
        ans = []
        for _, v in sorted(d.items()):
            v.sort(key=lambda x: x[0])
            ans.append([x[1] for x in v])
        return ans
```

#### Java

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private TreeMap<Integer, List<int[]>> d = new TreeMap<>();

    public List<List<Integer>> verticalOrder(TreeNode root) {
        dfs(root, 0, 0);
        List<List<Integer>> ans = new ArrayList<>();
        for (var v : d.values()) {
            Collections.sort(v, (a, b) -> a[0] - b[0]);
            List<Integer> t = new ArrayList<>();
            for (var e : v) {
                t.add(e[1]);
            }
            ans.add(t);
        }
        return ans;
    }

    private void dfs(TreeNode root, int depth, int offset) {
        if (root == null) {
            return;
        }
        d.computeIfAbsent(offset, k -> new ArrayList<>()).add(new int[] {depth, root.val});
        dfs(root.left, depth + 1, offset - 1);
        dfs(root.right, depth + 1, offset + 1);
    }
}
```

#### C++

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
using pii = pair<int, int>;

class Solution {
public:
    map<int, vector<pii>> d;

    vector<vector<int>> verticalOrder(TreeNode* root) {
        dfs(root, 0, 0);
        vector<vector<int>> ans;
        for (auto& [_, v] : d) {
            sort(v.begin(), v.end(), [&](pii& a, pii& b) {
                return a.first < b.first;
            });
            vector<int> t;
            for (auto& x : v) {
                t.push_back(x.second);
            }
            ans.push_back(t);
        }
        return ans;
    }

    void dfs(TreeNode* root, int depth, int offset) {
        if (!root) return;
        d[offset].push_back({depth, root->val});
        dfs(root->left, depth + 1, offset - 1);
        dfs(root->right, depth + 1, offset + 1);
    }
};
```

#### Go

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func verticalOrder(root *TreeNode) [][]int {
	d := map[int][][]int{}
	var dfs func(*TreeNode, int, int)
	dfs = func(root *TreeNode, depth, offset int) {
		if root == nil {
			return
		}
		d[offset] = append(d[offset], []int{depth, root.Val})
		dfs(root.Left, depth+1, offset-1)
		dfs(root.Right, depth+1, offset+1)
	}
	dfs(root, 0, 0)
	idx := []int{}
	for i := range d {
		idx = append(idx, i)
	}
	sort.Ints(idx)
	ans := [][]int{}
	for _, i := range idx {
		v := d[i]
		sort.SliceStable(v, func(i, j int) bool { return v[i][0] < v[j][0] })
		t := []int{}
		for _, x := range v {
			t = append(t, x[1])
		}
		ans = append(ans, t)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 vẫn phải sắp xếp từng cột theo độ sâu. BFS thăm node ở độ sâu nhỏ trước, nên các node trong mỗi cột đã có thứ tự từ trên xuống dưới; chỉ cần sắp xếp key của các cột. Đưa node trái/phải vào queue với độ lệch lần lượt là $-1$ và $+1$.

<!-- thinking:end -->

Cách phù hợp hơn là dùng BFS để duyệt từ trên xuống dưới theo từng level.

Độ phức tạp thời gian là $O(n\log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây nhị phân.

<!-- tabs:start -->

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def verticalOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if root is None:
            return []
        q = deque([(root, 0)])
        d = defaultdict(list)
        while q:
            for _ in range(len(q)):
                root, offset = q.popleft()
                d[offset].append(root.val)
                if root.left:
                    q.append((root.left, offset - 1))
                if root.right:
                    q.append((root.right, offset + 1))
        return [v for _, v in sorted(d.items())]
```

#### Java

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> verticalOrder(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        if (root == null) {
            return ans;
        }
        Deque<Pair<TreeNode, Integer>> q = new ArrayDeque<>();
        q.offer(new Pair<>(root, 0));
        TreeMap<Integer, List<Integer>> d = new TreeMap<>();
        while (!q.isEmpty()) {
            for (int n = q.size(); n > 0; --n) {
                var p = q.pollFirst();
                root = p.getKey();
                int offset = p.getValue();
                d.computeIfAbsent(offset, k -> new ArrayList()).add(root.val);
                if (root.left != null) {
                    q.offer(new Pair<>(root.left, offset - 1));
                }
                if (root.right != null) {
                    q.offer(new Pair<>(root.right, offset + 1));
                }
            }
        }
        return new ArrayList<>(d.values());
    }
}
```

#### C++

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> verticalOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if (!root) return ans;
        map<int, vector<int>> d;
        queue<pair<TreeNode*, int>> q{{{root, 0}}};
        while (!q.empty()) {
            for (int n = q.size(); n; --n) {
                auto p = q.front();
                q.pop();
                root = p.first;
                int offset = p.second;
                d[offset].push_back(root->val);
                if (root->left) q.push({root->left, offset - 1});
                if (root->right) q.push({root->right, offset + 1});
            }
        }
        for (auto& [_, v] : d) {
            ans.push_back(v);
        }
        return ans;
    }
};
```

#### Go

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func verticalOrder(root *TreeNode) [][]int {
	ans := [][]int{}
	if root == nil {
		return ans
	}
	d := map[int][]int{}
	q := []pair{pair{root, 0}}
	for len(q) > 0 {
		for n := len(q); n > 0; n-- {
			p := q[0]
			q = q[1:]
			root = p.node
			offset := p.offset
			d[offset] = append(d[offset], root.Val)
			if root.Left != nil {
				q = append(q, pair{root.Left, offset - 1})
			}
			if root.Right != nil {
				q = append(q, pair{root.Right, offset + 1})
			}
		}
	}
	idx := []int{}
	for i := range d {
		idx = append(idx, i)
	}
	sort.Ints(idx)
	for _, i := range idx {
		ans = append(ans, d[i])
	}
	return ans
}

type pair struct {
	node   *TreeNode
	offset int
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
