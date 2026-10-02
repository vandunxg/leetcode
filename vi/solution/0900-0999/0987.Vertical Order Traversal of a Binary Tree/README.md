---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Hash Table
    - Binary Tree
    - Sorting
---

<!-- problem:start -->

# [987. Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree)

[中文文档](/solution/0900-0999/0987.Vertical%20Order%20Traversal%20of%20a%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>root</code> của một binary tree, hãy thực hiện <strong>duyệt theo thứ tự dọc</strong> của cây.</p>

<p>Với mỗi node ở vị trí <code>(row, col)</code>, node con trái và node con phải lần lượt ở vị trí <code>(row + 1, col - 1)</code> và <code>(row + 1, col + 1)</code>. Root của cây nằm tại <code>(0, 0)</code>.</p>

<p><strong>Duyệt theo thứ tự dọc</strong> của binary tree là danh sách các node theo thứ tự từ trên xuống dưới trong từng cột, bắt đầu từ cột ngoài cùng bên trái đến cột ngoài cùng bên phải. Có thể có nhiều node cùng hàng và cùng cột; khi đó, sắp xếp chúng theo giá trị.</p>

<p>Trả về <em>kết quả <strong>duyệt theo thứ tự dọc</strong> của binary tree</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0987.Vertical%20Order%20Traversal%20of%20a%20Binary%20Tree/images/vtree1.jpg" style="width: 431px; height: 304px;" />
<pre>
<strong>Đầu vào:</strong> root = [3,9,20,null,null,15,7]
<strong>Đầu ra:</strong> [[9],[3,15],[20],[7]]
<strong>Giải thích:</strong>
Cột -1: Chỉ có node 9 trong cột này.
Cột 0: Node 3 và 15 nằm trong cột này theo thứ tự từ trên xuống dưới.
Cột 1: Chỉ có node 20 trong cột này.
Cột 2: Chỉ có node 7 trong cột này.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0987.Vertical%20Order%20Traversal%20of%20a%20Binary%20Tree/images/vtree2.jpg" style="width: 512px; height: 304px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,4,5,6,7]
<strong>Đầu ra:</strong> [[4],[2],[1,5,6],[3],[7]]
<strong>Giải thích:</strong>
Cột -2: Chỉ có node 4 trong cột này.
Cột -1: Chỉ có node 2 trong cột này.
Cột 0: Các node 1, 5 và 6 nằm trong cột này.
          Node 1 ở trên cùng nên đứng đầu.
          Node 5 và 6 cùng ở vị trí (2, 0), nên ta sắp xếp theo giá trị, 5 đứng trước 6.
Cột 1: Chỉ có node 3 trong cột này.
Cột 2: Chỉ có node 7 trong cột này.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0987.Vertical%20Order%20Traversal%20of%20a%20Binary%20Tree/images/vtree3.jpg" style="width: 512px; height: 304px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,4,6,5,7]
<strong>Đầu ra:</strong> [[4],[2],[1,5,6],[3],[7]]
<strong>Giải thích:</strong>
Trường hợp này giống hệt ví dụ 2, chỉ hoán đổi vị trí node 5 và 6.
Kết quả vẫn giống nhau vì node 5 và 6 ở cùng một vị trí, nên được sắp xếp theo giá trị.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 1000]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Liệt kê node theo cột từ trái sang phải, rồi theo hàng và giá trị trong từng cột. DFS ghi lại $(col,row,val)$; một lần sắp xếp sẽ đưa chúng về đúng thứ tự.

<!-- thinking:end -->

Ta định nghĩa hàm $dfs(root, i, j)$, trong đó $i$ và $j$ lần lượt là hàng và cột của node hiện tại. Dùng depth-first search để ghi nhận hàng, cột của các node vào mảng hoặc list $nodes$, sau đó sắp xếp $nodes$ theo cột, hàng rồi giá trị.

Tiếp theo, duyệt $nodes$, đưa giá trị của các node cùng cột vào một list, rồi trả về các list này.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số node trong binary tree.

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
    def verticalTraversal(self, root: Optional[TreeNode]) -> List[List[int]]:
        def dfs(root: Optional[TreeNode], i: int, j: int):
            if root is None:
                return
            nodes.append((j, i, root.val))
            dfs(root.left, i + 1, j - 1)
            dfs(root.right, i + 1, j + 1)

        nodes = []
        dfs(root, 0, 0)
        nodes.sort()
        ans = []
        prev = -2000
        for j, _, val in nodes:
            if prev != j:
                ans.append([])
                prev = j
            ans[-1].append(val)
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
    private List<int[]> nodes = new ArrayList<>();

    public List<List<Integer>> verticalTraversal(TreeNode root) {
        dfs(root, 0, 0);
        Collections.sort(nodes, (a, b) -> {
            if (a[0] != b[0]) {
                return Integer.compare(a[0], b[0]);
            }
            if (a[1] != b[1]) {
                return Integer.compare(a[1], b[1]);
            }
            return Integer.compare(a[2], b[2]);
        });
        List<List<Integer>> ans = new ArrayList<>();
        int prev = -2000;
        for (int[] node : nodes) {
            int j = node[0], val = node[2];
            if (prev != j) {
                ans.add(new ArrayList<>());
                prev = j;
            }
            ans.get(ans.size() - 1).add(val);
        }

        return ans;
    }

    private void dfs(TreeNode root, int i, int j) {
        if (root == null) {
            return;
        }
        nodes.add(new int[] {j, i, root.val});
        dfs(root.left, i + 1, j - 1);
        dfs(root.right, i + 1, j + 1);
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
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        vector<tuple<int, int, int>> nodes;
        function<void(TreeNode*, int, int)> dfs = [&](TreeNode* root, int i, int j) {
            if (!root) {
                return;
            }
            nodes.emplace_back(j, i, root->val);
            dfs(root->left, i + 1, j - 1);
            dfs(root->right, i + 1, j + 1);
        };
        dfs(root, 0, 0);
        sort(nodes.begin(), nodes.end());
        vector<vector<int>> ans;
        int prev = -2000;
        for (auto [j, _, val] : nodes) {
            if (j != prev) {
                prev = j;
                ans.emplace_back();
            }
            ans.back().push_back(val);
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
func verticalTraversal(root *TreeNode) (ans [][]int) {
	nodes := [][3]int{}
	var dfs func(*TreeNode, int, int)
	dfs = func(root *TreeNode, i, j int) {
		if root == nil {
			return
		}
		nodes = append(nodes, [3]int{j, i, root.Val})
		dfs(root.Left, i+1, j-1)
		dfs(root.Right, i+1, j+1)
	}
	dfs(root, 0, 0)
	sort.Slice(nodes, func(i, j int) bool {
		a, b := nodes[i], nodes[j]
		return a[0] < b[0] || a[0] == b[0] && (a[1] < b[1] || a[1] == b[1] && a[2] < b[2])
	})
	prev := -2000
	for _, node := range nodes {
		j, val := node[0], node[2]
		if j != prev {
			ans = append(ans, nil)
			prev = j
		}
		ans[len(ans)-1] = append(ans[len(ans)-1], val)
	}
	return
}
```

#### TypeScript

```ts
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     val: number
 *     left: TreeNode | null
 *     right: TreeNode | null
 *     constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.left = (left===undefined ? null : left)
 *         this.right = (right===undefined ? null : right)
 *     }
 * }
 */

function verticalTraversal(root: TreeNode | null): number[][] {
    const nodes: [number, number, number][] = [];
    const dfs = (root: TreeNode | null, i: number, j: number) => {
        if (!root) {
            return;
        }
        nodes.push([j, i, root.val]);
        dfs(root.left, i + 1, j - 1);
        dfs(root.right, i + 1, j + 1);
    };
    dfs(root, 0, 0);
    nodes.sort((a, b) => a[0] - b[0] || a[1] - b[1] || a[2] - b[2]);
    const ans: number[][] = [];
    let prev = -2000;
    for (const [j, _, val] of nodes) {
        if (j !== prev) {
            prev = j;
            ans.push([]);
        }
        ans.at(-1)!.push(val);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2：BFS + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> DFS vẫn cần sắp xếp theo cột, hàng và giá trị sau khi duyệt. BFS vốn đã duyệt theo thứ tự hàng, nên ta nhóm node theo cột và sắp xếp từng nhóm theo $(row,val)$. Dùng deque để thêm cột mới ở hai đầu mà không phải dịch các chỉ số cột.

<!-- thinking:end -->

Ta thực hiện breadth-first search (BFS) trên cây.

Vì kết quả cần được trả về từ cột ngoài cùng bên trái đến cột ngoài cùng bên phải, ta lưu:

- `leftmostCol`: chỉ số cột nhỏ nhất hiện đang được lưu.
- `rightmostCol`: chỉ số cột lớn nhất hiện đang được lưu.

Ta cũng dùng deque `columnsValues`, trong đó mỗi phần tử đại diện cho một cột và lưu các giá trị node thuộc cột đó.

Khi node vừa duyệt thuộc cột nằm ngoài phạm vi hiện tại:

- Nếu chỉ số cột của node $<$ `leftmostCol`, thêm một phần tử cột mới vào đầu deque.
- Nếu chỉ số cột của node $>$ `rightmostCol`, thêm một phần tử cột mới vào cuối deque.

- Với mỗi chỉ số cột `col`, vị trí tương ứng trong `columnsValues` được tính như sau:
- $$
  col - `leftmostCol`
  $$

Nhờ đó, ta có thể tìm cột cần truy cập trong thời gian hằng số.

Sau khi BFS kết thúc, mỗi cột chứa toàn bộ giá trị node thuộc cột đó.

Vì BFS đã duyệt node theo từng tầng, ta chỉ cần sắp xếp các giá trị trong từng cột theo thứ tự tăng dần để đáp ứng yêu cầu của đề bài.

Cuối cùng, xuất các cột từ trái sang phải.

#### Phân tích độ phức tạp

Giả sử binary tree có $n$ node.

- Độ phức tạp thời gian: $O(n \log n)$

    BFS duyệt mỗi node đúng một lần, mất $O(n)$ thời gian. Trong trường hợp xấu nhất, sắp xếp các giá trị mất $O(n \log n)$.
    Vì vậy, độ phức tạp thời gian tổng thể là $O(n \log n)$.

- Độ phức tạp không gian: $O(n)$

    Queue của BFS, deque và cấu trúc kết quả có thể lưu tổng cộng toàn bộ node, nên cần $O(n)$ không gian.

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
    def verticalTraversal(self, root: Optional[TreeNode]) -> list[list[int]]:
        # Format: (tree node, row, column).
        queue: deque[tuple[TreeNode, int, int]] = deque([(root, 0, 0)])

        # Deque append left speeds up indexing.
        # Each tuple format: (row, value).
        columns_values: deque[list[tuple[int, int]]] = deque([[]])

        leftmost_col, rightmost_col = 0, 0

        while queue:
            node, row, column = queue.popleft()

            if column < leftmost_col:
                leftmost_col = column
                columns_values.appendleft([])

            if column > rightmost_col:
                rightmost_col = column
                columns_values.append([])

            columns_values[column - leftmost_col].append((row, node.val))

            if node.left:
                queue.append((node.left, row + 1, column - 1))
            if node.right:
                queue.append((node.right, row + 1, column + 1))

        vertical_traversal: list[list[int]] = []

        for column_values in columns_values:
            vertical_traversal.append([])

            column_values.sort()
            for _, value in column_values:
                vertical_traversal[-1].append(value)

        return vertical_traversal
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
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        // Each tuple format: {tree node, row, column}.
        deque<tuple<TreeNode*, int, int>> queue = {{root, 0, 0}};

        // Each tuple format: {row, value}.
        deque<vector<tuple<int, int>>> columnsValues = {{}};

        int leftmostCol = 0, rightmostCol = 0;

        while (!queue.empty()) {
            auto [node, row, column] = queue.front();
            queue.pop_front();

            if (column < leftmostCol) {
                leftmostCol = column;
                columnsValues.push_front({});
            }

            if (column > rightmostCol) {
                rightmostCol = column;
                columnsValues.push_back({});
            }

            columnsValues[column - leftmostCol].push_back({row, node->val});

            if (node->left != nullptr)
                queue.push_back({node->left, row + 1, column - 1});

            if (node->right != nullptr)
                queue.push_back({node->right, row + 1, column + 1});
        }

        vector<vector<int>> verticalTraversal = {};

        for (auto columnValues : columnsValues) { // Need to sort so no const.
            verticalTraversal.push_back({});

            sort(columnValues.begin(), columnValues.end());
            for (const auto& [row, value] : columnValues)
                verticalTraversal.back().push_back(value);
        }

        return verticalTraversal;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
