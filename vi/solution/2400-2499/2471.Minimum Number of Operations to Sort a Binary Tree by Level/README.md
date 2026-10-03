---
comments: true
difficulty: Medium
rating: 1635
source: Weekly Contest 319 Q3
tags:
    - Tree
    - Breadth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [2471. Minimum Number of Operations to Sort a Binary Tree by Level](https://leetcode.com/problems/minimum-number-of-operations-to-sort-a-binary-tree-by-level)

[Tài liệu tiếng Trung](/solution/2400-2499/2471.Minimum%20Number%20of%20Operations%20to%20Sort%20a%20Binary%20Tree%20by%20Level/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>root</code> của một cây nhị phân có các giá trị <strong>duy nhất</strong>.</p>

<p>Trong một phép toán, bạn có thể chọn bất kỳ hai node <strong>cùng level</strong> và hoán đổi giá trị của chúng.</p>

<p>Hãy trả về <em>số phép toán ít nhất cần thực hiện để các giá trị ở mỗi level được sắp xếp theo <strong>thứ tự tăng dần nghiêm ngặt</strong></em>.</p>

<p><strong>Level</strong> của một node là số cạnh trên đường đi giữa node đó và node gốc<em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2471.Minimum%20Number%20of%20Operations%20to%20Sort%20a%20Binary%20Tree%20by%20Level/images/image-20220918174006-2.png" style="width: 500px; height: 324px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,4,3,7,6,8,5,null,null,null,null,9,null,10]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
- Hoán đổi 4 và 3. Level 2<sup>nd</sup> trở thành [3,4].
- Hoán đổi 7 và 5. Level 3<sup>rd</sup> trở thành [5,6,8,7].
- Hoán đổi 8 và 7. Level 3<sup>rd</sup> trở thành [5,6,7,8].
Ta đã sử dụng 3 phép toán, vì vậy trả về 3.
Có thể chứng minh rằng 3 là số phép toán ít nhất cần thực hiện.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2471.Minimum%20Number%20of%20Operations%20to%20Sort%20a%20Binary%20Tree%20by%20Level/images/image-20220918174026-3.png" style="width: 400px; height: 303px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,3,2,7,6,5,4]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
- Hoán đổi 3 và 2. Level 2<sup>nd</sup> trở thành [2,3].
- Hoán đổi 7 và 4. Level 3<sup>rd</sup> trở thành [4,6,5,7].
- Hoán đổi 6 và 5. Level 3<sup>rd</sup> trở thành [4,5,6,7].
Ta đã sử dụng 3 phép toán, vì vậy trả về 3.
Có thể chứng minh rằng 3 là số phép toán ít nhất cần thực hiện.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2471.Minimum%20Number%20of%20Operations%20to%20Sort%20a%20Binary%20Tree%20by%20Level/images/image-20220918174052-4.png" style="width: 400px; height: 274px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,4,5,6]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mỗi level đã được sắp xếp theo thứ tự tăng dần, vì vậy trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
	<li>Tất cả các giá trị trong cây đều <strong>duy nhất</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS + Discretization + Hoán đổi phần tử

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi level được sắp xếp độc lập bằng cách hoán đổi các giá trị. Số lần hoán đổi ít nhất bằng số bước cần thực hiện để đưa permutation về identity, tức là duyệt qua các cycle sau khi xếp hạng các giá trị.
>
> BFS thu thập một level, ánh xạ các giá trị thành các rank $0..len-1$, sau đó hoán đổi $t[i]$ với $t[t[i]]$ cho đến khi tất cả phần tử đúng vị trí và đếm số lần hoán đổi.

<!-- thinking:end -->

Trước hết, ta duyệt cây nhị phân bằng BFS để tìm các giá trị node ở từng level. Sau đó, ta sắp xếp các giá trị node ở mỗi level. Nếu các giá trị node sau khi sắp xếp khác với các giá trị ban đầu, điều đó có nghĩa là ta cần hoán đổi các phần tử. Số lần hoán đổi chính là số phép toán cần thực hiện ở level đó.

Độ phức tạp thời gian là $O(n \times \log n)$. Trong đó, $n$ là số node trong cây nhị phân.

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
    def minimumOperations(self, root: Optional[TreeNode]) -> int:
        def swap(arr, i, j):
            arr[i], arr[j] = arr[j], arr[i]

        def f(t):
            n = len(t)
            m = {v: i for i, v in enumerate(sorted(t))}
            for i in range(n):
                t[i] = m[t[i]]
            ans = 0
            for i in range(n):
                while t[i] != i:
                    swap(t, i, t[i])
                    ans += 1
            return ans

        q = deque([root])
        ans = 0
        while q:
            t = []
            for _ in range(len(q)):
                node = q.popleft()
                t.append(node.val)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            ans += f(t)
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
    public int minimumOperations(TreeNode root) {
        Deque<TreeNode> q = new ArrayDeque<>();
        q.offer(root);
        int ans = 0;
        while (!q.isEmpty()) {
            List<Integer> t = new ArrayList<>();
            for (int n = q.size(); n > 0; --n) {
                TreeNode node = q.poll();
                t.add(node.val);
                if (node.left != null) {
                    q.offer(node.left);
                }
                if (node.right != null) {
                    q.offer(node.right);
                }
            }
            ans += f(t);
        }
        return ans;
    }

    private int f(List<Integer> t) {
        int n = t.size();
        List<Integer> alls = new ArrayList<>(t);
        alls.sort((a, b) -> a - b);
        Map<Integer, Integer> m = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            m.put(alls.get(i), i);
        }
        int[] arr = new int[n];
        for (int i = 0; i < n; ++i) {
            arr[i] = m.get(t.get(i));
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            while (arr[i] != i) {
                swap(arr, i, arr[i]);
                ++ans;
            }
        }
        return ans;
    }

    private void swap(int[] arr, int i, int j) {
        int t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
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
    int minimumOperations(TreeNode* root) {
        queue<TreeNode*> q{{root}};
        int ans = 0;
        auto f = [](vector<int>& t) {
            int n = t.size();
            vector<int> alls(t.begin(), t.end());
            sort(alls.begin(), alls.end());
            unordered_map<int, int> m;
            int ans = 0;
            for (int i = 0; i < n; ++i) m[alls[i]] = i;
            for (int i = 0; i < n; ++i) t[i] = m[t[i]];
            for (int i = 0; i < n; ++i) {
                while (t[i] != i) {
                    swap(t[i], t[t[i]]);
                    ++ans;
                }
            }
            return ans;
        };
        while (!q.empty()) {
            vector<int> t;
            for (int n = q.size(); n; --n) {
                auto node = q.front();
                q.pop();
                t.emplace_back(node->val);
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            ans += f(t);
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
func minimumOperations(root *TreeNode) (ans int) {
	f := func(t []int) int {
		var alls []int
		for _, v := range t {
			alls = append(alls, v)
		}
		sort.Ints(alls)
		m := make(map[int]int)
		for i, v := range alls {
			m[v] = i
		}
		for i := range t {
			t[i] = m[t[i]]
		}
		res := 0
		for i := range t {
			for t[i] != i {
				t[i], t[t[i]] = t[t[i]], t[i]
				res++
			}
		}
		return res
	}

	q := []*TreeNode{root}
	for len(q) > 0 {
		t := []int{}
		for n := len(q); n > 0; n-- {
			node := q[0]
			q = q[1:]
			t = append(t, node.Val)
			if node.Left != nil {
				q = append(q, node.Left)
			}
			if node.Right != nil {
				q = append(q, node.Right)
			}
		}
		ans += f(t)
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

function minimumOperations(root: TreeNode | null): number {
    const queue = [root];
    let ans = 0;
    while (queue.length !== 0) {
        const n = queue.length;
        const row: number[] = [];
        for (let i = 0; i < n; i++) {
            const { val, left, right } = queue.shift();
            row.push(val);
            left && queue.push(left);
            right && queue.push(right);
        }
        for (let i = 0; i < n - 1; i++) {
            let minIdx = i;
            for (let j = i + 1; j < n; j++) {
                if (row[j] < row[minIdx]) {
                    minIdx = j;
                }
            }
            if (i !== minIdx) {
                [row[i], row[minIdx]] = [row[minIdx], row[i]];
                ans++;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
// Definition for a binary tree node.
// #[derive(Debug, PartialEq, Eq)]
// pub struct TreeNode {
//   pub val: i32,
//   pub left: Option<Rc<RefCell<TreeNode>>>,
//   pub right: Option<Rc<RefCell<TreeNode>>>,
// }
//
// impl TreeNode {
//   #[inline]
//   pub fn new(val: i32) -> Self {
//     TreeNode {
//       val,
//       left: None,
//       right: None
//     }
//   }
// }
use std::cell::RefCell;
use std::collections::VecDeque;
use std::rc::Rc;
impl Solution {
    pub fn minimum_operations(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        let mut queue = VecDeque::new();
        queue.push_back(root);
        let mut ans = 0;
        while !queue.is_empty() {
            let n = queue.len();
            let mut row = Vec::new();
            for _ in 0..n {
                let mut node = queue.pop_front().unwrap();
                let mut node = node.as_mut().unwrap().borrow_mut();
                row.push(node.val);
                if node.left.is_some() {
                    queue.push_back(node.left.take());
                }
                if node.right.is_some() {
                    queue.push_back(node.right.take());
                }
            }
            for i in 0..n - 1 {
                let mut min_idx = i;
                for j in i + 1..n {
                    if row[j] < row[min_idx] {
                        min_idx = j;
                    }
                }
                if i != min_idx {
                    row.swap(i, min_idx);
                    ans += 1;
                }
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
