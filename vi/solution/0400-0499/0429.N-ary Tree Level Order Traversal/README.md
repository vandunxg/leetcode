---
comments: true
difficulty: Medium
tags:
    - Tree
    - Breadth-First Search
---

<!-- problem:start -->

# [429. N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal)

[中文文档](/solution/0400-0499/0429.N-ary%20Tree%20Level%20Order%20Traversal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho cây n-ary, trả về các giá trị của node theo thứ tự duyệt <em>theo từng level</em>.</p>

<p><em>Dữ liệu đầu vào của cây n-ary được biểu diễn theo thứ tự duyệt từng level; các nhóm node con được phân tách bằng giá trị null (xem ví dụ).</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0429.N-ary%20Tree%20Level%20Order%20Traversal/images/narytreeexample.png" style="width: 100%; max-width: 300px;" /></p>

<pre>
<strong>Đầu vào:</strong> root = [1,null,3,2,4,null,5,6]
<strong>Đầu ra:</strong> [[1],[3,2,4],[5,6]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0429.N-ary%20Tree%20Level%20Order%20Traversal/images/sample_4_964.png" style="width: 296px; height: 241px;" /></p>

<pre>
<strong>Đầu vào:</strong> root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
<strong>Đầu ra:</strong> [[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Chiều cao của cây n-ary không vượt quá <code>1000</code>.</li>
	<li>Tổng số node nằm trong khoảng <code>[0, 10<sup>4</sup>]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt theo level nhóm các giá trị theo độ sâu. Nếu duyệt depth-first mà không lưu chỉ số level, các level sẽ bị trộn lẫn.
>
> Queue lưu các node ở level hiện tại: ghi nhận số lượng node, pop đúng số node đó để tạo một hàng kết quả, đồng thời enqueue các node con của chúng. Nếu root rỗng thì kết quả là danh sách rỗng.
>
> Ghi nhận kích thước queue giúp xử lý riêng từng level và giữ đúng thứ tự từ trái sang phải.

<!-- thinking:end -->

Trước tiên, kiểm tra root có null không. Nếu có, trả về danh sách rỗng ngay.

Nếu không, tạo queue $q$ và đưa root vào queue.

Khi queue chưa rỗng, lặp lại các bước sau:

1. Tạo danh sách rỗng $t$ để lưu giá trị các node ở level hiện tại.
2. Với mỗi node trong queue, thêm giá trị của nó vào $t$ và đưa các node con vào queue.
3. Thêm $t$ vào danh sách kết quả $ans$.

Cuối cùng, trả về danh sách kết quả $ans$.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là số node trong cây n-ary.

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""


class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        ans = []
        if root is None:
            return ans
        q = deque([root])
        while q:
            t = []
            for _ in range(len(q)):
                root = q.popleft()
                t.append(root.val)
                q.extend(root.children)
            ans.append(t)
        return ans
```

#### Java

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> ans = new ArrayList<>();
        if (root == null) {
            return ans;
        }
        Deque<Node> q = new ArrayDeque<>();
        q.offer(root);
        while (!q.isEmpty()) {
            List<Integer> t = new ArrayList<>();
            for (int n = q.size(); n > 0; --n) {
                root = q.poll();
                t.add(root.val);
                q.addAll(root.children);
            }
            ans.add(t);
        }
        return ans;
    }
}
```

#### C++

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> ans;
        if (!root) {
            return ans;
        }
        queue<Node*> q{{root}};
        while (!q.empty()) {
            vector<int> t;
            for (int n = q.size(); n; --n) {
                root = q.front();
                q.pop();
                t.push_back(root->val);
                for (auto& child : root->children) {
                    q.push(child);
                }
            }
            ans.push_back(t);
        }
        return ans;
    }
};
```

#### Go

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func levelOrder(root *Node) (ans [][]int) {
	if root == nil {
		return
	}
	q := []*Node{root}
	for len(q) > 0 {
		var t []int
		for n := len(q); n > 0; n-- {
			root = q[0]
			q = q[1:]
			t = append(t, root.Val)
			for _, child := range root.Children {
				q = append(q, child)
			}
		}
		ans = append(ans, t)
	}
	return
}
```

#### TypeScript

```ts
/**
 * Definition for node.
 * class Node {
 *     val: number
 *     children: Node[]
 *     constructor(val?: number) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.children = []
 *     }
 * }
 */

function levelOrder(root: Node | null): number[][] {
    const ans: number[][] = [];
    if (!root) {
        return ans;
    }
    const q: Node[] = [root];
    while (q.length) {
        const qq: Node[] = [];
        const t: number[] = [];
        for (const { val, children } of q) {
            qq.push(...children);
            t.push(val);
        }
        ans.push(t);
        q.splice(0, q.length, ...qq);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 cần queue. Ta có thể đệ quy với độ sâu $i$ và thêm node vào $\textit{ans}[i]$ (mở rộng danh sách khi cần) để tạo ra các hàng kết quả tương tự.
>
> Call stack lưu chỉ số level; các node con vẫn được thêm theo thứ tự từ trái sang phải.

<!-- thinking:end -->

Ta có thể dùng Depth-First Search để duyệt toàn bộ cây.

Định nghĩa hàm phụ trợ $dfs(root, i)$, trong đó $root$ là node hiện tại và $i$ là level hiện tại.

Trong hàm $dfs$, trước tiên kiểm tra $root$ có null không. Nếu có, kết thúc hàm.

Nếu không, kiểm tra độ dài của $ans$ có nhỏ hơn hoặc bằng $i$ không. Nếu có, nghĩa là level hiện tại chưa được thêm vào $ans$, nên trước tiên ta thêm một danh sách rỗng. Sau đó, thêm giá trị của $root$ vào $ans[i]$.

Tiếp theo, duyệt tất cả node con của $root$. Với mỗi node con, gọi $dfs(child, i + 1)$.

Trong hàm chính, gọi $dfs(root, 0)$ rồi trả về $ans$.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là số node trong cây n-ary.

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""


class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        def dfs(root, i):
            if root is None:
                return
            if len(ans) <= i:
                ans.append([])
            ans[i].append(root.val)
            for child in root.children:
                dfs(child, i + 1)

        ans = []
        dfs(root, 0)
        return ans
```

#### Java

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    private List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> levelOrder(Node root) {
        dfs(root, 0);
        return ans;
    }

    private void dfs(Node root, int i) {
        if (root == null) {
            return;
        }
        if (ans.size() <= i) {
            ans.add(new ArrayList<>());
        }
        ans.get(i++).add(root.val);
        for (Node child : root.children) {
            dfs(child, i);
        }
    }
}
```

#### C++

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> ans;
        function<void(Node*, int i)> dfs = [&](Node* root, int i) {
            if (!root) {
                return;
            }
            if (ans.size() <= i) {
                ans.push_back({});
            }
            ans[i++].push_back(root->val);
            for (auto& child : root->children) {
                dfs(child, i);
            }
        };
        dfs(root, 0);
        return ans;
    }
};
```

#### Go

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func levelOrder(root *Node) (ans [][]int) {
	var dfs func(root *Node, i int)
	dfs = func(root *Node, i int) {
		if root == nil {
			return
		}
		if len(ans) <= i {
			ans = append(ans, []int{})
		}
		ans[i] = append(ans[i], root.Val)
		for _, child := range root.Children {
			dfs(child, i+1)
		}
	}
	dfs(root, 0)
	return
}
```

#### TypeScript

```ts
/**
 * Definition for node.
 * class Node {
 *     val: number
 *     children: Node[]
 *     constructor(val?: number) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.children = []
 *     }
 * }
 */

function levelOrder(root: Node | null): number[][] {
    const ans: number[][] = [];
    const dfs = (root: Node | null, i: number) => {
        if (root === null) {
            return;
        }
        if (ans.length <= i) {
            ans.push([]);
        }
        const { val, children } = root;
        ans[i++].push(val);
        children.forEach(node => dfs(node, i));
    };
    dfs(root, 0);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
