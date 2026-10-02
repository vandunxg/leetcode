---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Binary Tree
---

<!-- problem:start -->

# [545. Boundary of Binary Tree 🔒](https://leetcode.com/problems/boundary-of-binary-tree)

[中文文档](/solution/0500-0599/0545.Boundary%20of%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Biên</strong> của cây nhị phân là phép nối <strong>root</strong>, <strong>biên trái</strong>, các <strong>node lá</strong> theo thứ tự từ trái sang phải, rồi đến <strong>biên phải</strong> theo thứ tự ngược.</p>

<p><strong>Biên trái</strong> gồm các node được xác định như sau:</p>

<ul>
	<li>Con trái của root thuộc biên trái. Nếu root không có con trái thì biên trái <strong>rỗng</strong>.</li>
	<li>Nếu một node thuộc biên trái và có con trái, thì con trái cũng thuộc biên trái.</li>
	<li>Nếu một node thuộc biên trái, <strong>không có</strong> con trái nhưng có con phải, thì con phải thuộc biên trái.</li>
	<li>Node lá ngoài cùng bên trái <strong>không</strong> thuộc biên trái.</li>
</ul>

<p><strong>Biên phải</strong> được xác định tương tự biên trái, nhưng nằm ở phía phải của cây con phải của root. Node lá cũng <strong>không</strong> thuộc <strong>biên phải</strong>, và <strong>biên phải</strong> rỗng nếu root không có con phải.</p>

<p><strong>Node lá</strong> là node không có con. Trong bài này, root <strong>không</strong> được xem là node lá.</p>

<p>Cho <code>root</code> của một cây nhị phân, hãy trả về <em>các giá trị trên <strong>biên</strong> của cây</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0545.Boundary%20of%20Binary%20Tree/images/boundary1.jpg" style="width: 299px; height: 290px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,null,2,3,4]
<strong>Đầu ra:</strong> [1,3,4,2]
<b>Giải thích:</b>
- Biên trái rỗng vì root không có con trái.
- Biên phải đi theo đường bắt đầu từ con phải 2 của root: 2 -&gt; 4.
  4 là node lá nên biên phải là [2].
- Các node lá từ trái sang phải là [3,4].
Nối các phần lại, ta được [1] + [] + [3,4] + [2] = [1,3,4,2].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0545.Boundary%20of%20Binary%20Tree/images/boundary2.jpg" style="width: 599px; height: 411px;" />
<pre>
<strong>Đầu vào:</strong> root = [1,2,3,4,5,6,null,null,null,7,8,9,10]
<strong>Đầu ra:</strong> [1,2,4,7,8,9,10,6,3]
<b>Giải thích:</b>
- Biên trái đi theo đường bắt đầu từ con trái 2 của root: 2 -&gt; 4.
  4 là node lá nên biên trái là [2].
- Biên phải đi theo đường bắt đầu từ con phải 3 của root: 3 -&gt; 6 -&gt; 10.
  10 là node lá nên biên phải là [3,6], và theo thứ tự ngược là [6,3].
- Các node lá từ trái sang phải là [4,7,8,9,10].
Nối các phần lại, ta được [1] + [2] + [4,7,8,9,10] + [6,3] = [1,2,4,7,8,9,10,6,3].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong cây nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>-1000 &lt;= Node.val &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Biên gồm phía trái, các node lá và phía phải theo thứ tự ngược chiều kim đồng hồ. Root chỉ xuất hiện một lần và không được lặp node lá. Nếu gộp thành một lần duyệt, sẽ khó xử lý hướng duyệt và tránh trùng lặp.
>
> Thực hiện ba lượt DFS: duyệt biên trái, ưu tiên đi trái (nếu không có thì đi phải) và bỏ qua node lá; lượt thứ hai thu thập các node lá; cuối cùng duyệt biên phải, ưu tiên đi phải rồi đảo ngược kết quả. Thêm root đầu tiên nếu root không phải node lá. Biến cờ $i$ xác định vai trò của mỗi lượt.

<!-- thinking:end -->

Trước tiên, nếu cây chỉ có một node, ta trả về ngay danh sách chứa giá trị của node đó.

Nếu không, ta dùng depth-first search (DFS) để tìm biên trái, các node lá và biên phải của cây nhị phân.

Cụ thể, ta dùng hàm đệ quy $\textit{dfs}$ để tìm ba phần này. Hàm nhận danh sách $\textit{nums}$, node $\textit{root}$ và số nguyên $\textit{i}$. $\textit{nums}$ lưu giá trị các node thuộc phần hiện tại; $\textit{root}$ là node đang xét; còn $\textit{i}$ cho biết loại phần cần tìm (biên trái, node lá hay biên phải).

Hàm được triển khai như sau:

- Nếu $\textit{root}$ là null, trả về ngay.
- Nếu $\textit{i} = 0$, ta tìm biên trái. Nếu $\textit{root}$ không phải node lá, thêm giá trị của nó vào $\textit{nums}$. Nếu $\textit{root}$ có con trái, gọi đệ quy $\textit{dfs}$ với $\textit{nums}$, con trái của $\textit{root}$ và $\textit{i}$. Nếu không, gọi với con phải của $\textit{root}$.
- Nếu $\textit{i} = 1$, ta tìm các node lá. Nếu $\textit{root}$ là node lá, thêm giá trị của nó vào $\textit{nums}$. Nếu không, gọi đệ quy $\textit{dfs}$ cho cả con trái và con phải của $\textit{root}$, truyền $\textit{nums}$ và $\textit{i}$ vào mỗi lần gọi.
- Nếu $\textit{i} = 2$, ta tìm biên phải. Nếu $\textit{root}$ không phải node lá, thêm giá trị của nó vào $\textit{nums}$. Nếu $\textit{root}$ có con phải, gọi đệ quy $\textit{dfs}$ với $\textit{nums}$, con phải của $\textit{root}$ và $\textit{i}$. Nếu không, gọi với con trái của $\textit{root}$.

Ta lần lượt gọi hàm $\textit{dfs}$ để tìm biên trái, các node lá và biên phải, sau đó nối ba phần này lại để tạo đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây nhị phân.

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
    def boundaryOfBinaryTree(self, root: Optional[TreeNode]) -> List[int]:
        def dfs(nums: List[int], root: Optional[TreeNode], i: int):
            if root is None:
                return
            if i == 0:
                if root.left != root.right:
                    nums.append(root.val)
                    if root.left:
                        dfs(nums, root.left, i)
                    else:
                        dfs(nums, root.right, i)
            elif i == 1:
                if root.left == root.right:
                    nums.append(root.val)
                else:
                    dfs(nums, root.left, i)
                    dfs(nums, root.right, i)
            else:
                if root.left != root.right:
                    nums.append(root.val)
                    if root.right:
                        dfs(nums, root.right, i)
                    else:
                        dfs(nums, root.left, i)

        ans = [root.val]
        if root.left == root.right:
            return ans
        left, leaves, right = [], [], []
        dfs(left, root.left, 0)
        dfs(leaves, root, 1)
        dfs(right, root.right, 2)
        ans += left + leaves + right[::-1]
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> boundaryOfBinaryTree(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        ans.add(root.val);
        if (root.left == root.right) {
            return ans;
        }
        List<Integer> left = new ArrayList<>();
        List<Integer> leaves = new ArrayList<>();
        List<Integer> right = new ArrayList<>();
        dfs(left, root.left, 0);
        dfs(leaves, root, 1);
        dfs(right, root.right, 2);

        ans.addAll(left);
        ans.addAll(leaves);
        Collections.reverse(right);
        ans.addAll(right);
        return ans;
    }

    private void dfs(List<Integer> nums, TreeNode root, int i) {
        if (root == null) {
            return;
        }
        if (i == 0) {
            if (root.left != root.right) {
                nums.add(root.val);
                if (root.left != null) {
                    dfs(nums, root.left, i);
                } else {
                    dfs(nums, root.right, i);
                }
            }
        } else if (i == 1) {
            if (root.left == root.right) {
                nums.add(root.val);
            } else {
                dfs(nums, root.left, i);
                dfs(nums, root.right, i);
            }
        } else {
            if (root.left != root.right) {
                nums.add(root.val);
                if (root.right != null) {
                    dfs(nums, root.right, i);
                } else {
                    dfs(nums, root.left, i);
                }
            }
        }
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
    vector<int> boundaryOfBinaryTree(TreeNode* root) {
        auto dfs = [&](this auto&& dfs, vector<int>& nums, TreeNode* root, int i) -> void {
            if (!root) {
                return;
            }
            if (i == 0) {
                if (root->left != root->right) {
                    nums.push_back(root->val);
                    if (root->left) {
                        dfs(nums, root->left, i);
                    } else {
                        dfs(nums, root->right, i);
                    }
                }
            } else if (i == 1) {
                if (root->left == root->right) {
                    nums.push_back(root->val);
                } else {
                    dfs(nums, root->left, i);
                    dfs(nums, root->right, i);
                }
            } else {
                if (root->left != root->right) {
                    nums.push_back(root->val);
                    if (root->right) {
                        dfs(nums, root->right, i);
                    } else {
                        dfs(nums, root->left, i);
                    }
                }
            }
        };
        vector<int> ans = {root->val};
        if (root->left == root->right) {
            return ans;
        }
        vector<int> left, right, leaves;
        dfs(left, root->left, 0);
        dfs(leaves, root, 1);
        dfs(right, root->right, 2);
        ans.insert(ans.end(), left.begin(), left.end());
        ans.insert(ans.end(), leaves.begin(), leaves.end());
        ans.insert(ans.end(), right.rbegin(), right.rend());
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
func boundaryOfBinaryTree(root *TreeNode) []int {
	ans := []int{root.Val}
	if root.Left == root.Right {
		return ans
	}

	left, leaves, right := []int{}, []int{}, []int{}

	var dfs func(nums *[]int, root *TreeNode, i int)
	dfs = func(nums *[]int, root *TreeNode, i int) {
		if root == nil {
			return
		}
		if i == 0 {
			if root.Left != root.Right {
				*nums = append(*nums, root.Val)
				if root.Left != nil {
					dfs(nums, root.Left, i)
				} else {
					dfs(nums, root.Right, i)
				}
			}
		} else if i == 1 {
			if root.Left == root.Right {
				*nums = append(*nums, root.Val)
			} else {
				dfs(nums, root.Left, i)
				dfs(nums, root.Right, i)
			}
		} else {
			if root.Left != root.Right {
				*nums = append(*nums, root.Val)
				if root.Right != nil {
					dfs(nums, root.Right, i)
				} else {
					dfs(nums, root.Left, i)
				}
			}
		}
	}

	dfs(&left, root.Left, 0)
	dfs(&leaves, root, 1)
	dfs(&right, root.Right, 2)

	ans = append(ans, left...)
	ans = append(ans, leaves...)
	for i := len(right) - 1; i >= 0; i-- {
		ans = append(ans, right[i])
	}

	return ans
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

function boundaryOfBinaryTree(root: TreeNode | null): number[] {
    const ans: number[] = [root.val];
    if (root.left === root.right) {
        return ans;
    }

    const left: number[] = [];
    const leaves: number[] = [];
    const right: number[] = [];

    const dfs = function (nums: number[], root: TreeNode | null, i: number) {
        if (!root) {
            return;
        }
        if (i === 0) {
            if (root.left !== root.right) {
                nums.push(root.val);
                if (root.left) {
                    dfs(nums, root.left, i);
                } else {
                    dfs(nums, root.right, i);
                }
            }
        } else if (i === 1) {
            if (root.left === root.right) {
                nums.push(root.val);
            } else {
                dfs(nums, root.left, i);
                dfs(nums, root.right, i);
            }
        } else {
            if (root.left !== root.right) {
                nums.push(root.val);
                if (root.right) {
                    dfs(nums, root.right, i);
                } else {
                    dfs(nums, root.left, i);
                }
            }
        }
    };

    dfs(left, root.left, 0);
    dfs(leaves, root, 1);
    dfs(right, root.right, 2);

    return ans.concat(left).concat(leaves).concat(right.reverse());
}
```

#### JavaScript

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[]}
 */
var boundaryOfBinaryTree = function (root) {
    const ans = [root.val];
    if (root.left === root.right) {
        return ans;
    }

    const left = [];
    const leaves = [];
    const right = [];

    const dfs = function (nums, root, i) {
        if (!root) {
            return;
        }
        if (i === 0) {
            if (root.left !== root.right) {
                nums.push(root.val);
                if (root.left) {
                    dfs(nums, root.left, i);
                } else {
                    dfs(nums, root.right, i);
                }
            }
        } else if (i === 1) {
            if (root.left === root.right) {
                nums.push(root.val);
            } else {
                dfs(nums, root.left, i);
                dfs(nums, root.right, i);
            }
        } else {
            if (root.left !== root.right) {
                nums.push(root.val);
                if (root.right) {
                    dfs(nums, root.right, i);
                } else {
                    dfs(nums, root.left, i);
                }
            }
        }
    };

    dfs(left, root.left, 0);
    dfs(leaves, root, 1);
    dfs(right, root.right, 2);
    return ans.concat(left).concat(leaves).concat(right.reverse());
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
