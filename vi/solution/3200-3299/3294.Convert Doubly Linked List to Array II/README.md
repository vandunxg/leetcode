---
comments: true
difficulty: Medium
tags:
    - Array
    - Linked List
    - Doubly-Linked List
---

<!-- problem:start -->

# [3294. Convert Doubly Linked List to Array II 🔒](https://leetcode.com/problems/convert-doubly-linked-list-to-array-ii)

[Tài liệu tiếng Trung](/solution/3200-3299/3294.Convert%20Doubly%20Linked%20List%20to%20Array%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một <strong>bất kỳ <code>node</code></strong> trong một <strong>doubly linked list</strong>, trong đó các node có con trỏ next và previous.</p>

<p>Hãy trả về một mảng số nguyên chứa các phần tử của linked list <strong>theo thứ tự</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">head = [1,2,3,4,5], node = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,3,4,5]</span></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">head = [4,5,6,7,8], node = 8</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[4,5,6,7,8]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số lượng node trong danh sách đã cho nằm trong khoảng <code>[1, 500]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 1000</code></li>
	<li>Tất cả node đều có giá trị <code>Node.val</code> không trùng nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt Linked List

<!-- thinking:start -->

> **Tư duy**
>
> Ta được cho một node bất kỳ của doubly linked list và cần xuất toàn bộ list từ trái sang phải. Duyệt theo `prev` đến head, sau đó thu thập các phần tử về bên phải.
>
> Sau khi `prev` là null, duyệt theo `next` như ở Lời giải 1. Có nhiều nhất $500$ node, nên cần hai lượt duyệt tuyến tính.

<!-- thinking:end -->

Ta có thể bắt đầu từ node đã cho và duyệt ngược linked list cho đến khi đến node head. Sau đó, ta duyệt linked list theo chiều xuôi từ node head, thêm giá trị của các node gặp được vào mảng kết quả.

Sau khi duyệt xong, trả về mảng kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số lượng node trong linked list. Không tính phần bộ nhớ dùng cho mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val, prev=None, next=None):
        self.val = val
        self.prev = prev
        self.next = next
"""


class Solution:
    def toArray(self, node: "Optional[Node]") -> List[int]:
        while node.prev:
            node = node.prev
        ans = []
        while node:
            ans.append(node.val)
            node = node.next
        return ans
```

#### Java

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
};
*/

class Solution {
    public int[] toArray(Node node) {
        while (node != null && node.prev != null) {
            node = node.prev;
        }
        var ans = new ArrayList<Integer>();
        for (; node != null; node = node.next) {
            ans.add(node.val);
        }
        return ans.stream().mapToInt(i -> i).toArray();
    }
}
```

#### C++

```cpp
/**
 * Definition for doubly-linked list.
 * class Node {
 *     int val;
 *     Node* prev;
 *     Node* next;
 *     Node() : val(0), next(nullptr), prev(nullptr) {}
 *     Node(int x) : val(x), next(nullptr), prev(nullptr) {}
 *     Node(int x, Node *prev, Node *next) : val(x), next(next), prev(prev) {}
 * };
 */
class Solution {
public:
    vector<int> toArray(Node* node) {
        while (node && node->prev) {
            node = node->prev;
        }
        vector<int> ans;
        for (; node; node = node->next) {
            ans.push_back(node->val);
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
 *     Next *Node
 *     Prev *Node
 * }
 */

func toArray(node *Node) (ans []int) {
	for node != nil && node.Prev != nil {
		node = node.Prev
	}
	for ; node != nil; node = node.Next {
		ans = append(ans, node.Val)
	}
	return
}
```

#### TypeScript

```ts
/**
 * Definition for _Node.
 * class _Node {
 *     val: number
 *     prev: _Node | null
 *     next: _Node | null
 *
 *     constructor(val?: number, prev? : _Node, next? : _Node) {
 *         this.val = (val===undefined ? 0 : val);
 *         this.prev = (prev===undefined ? null : prev);
 *         this.next = (next===undefined ? null : next);
 *     }
 * }
 */

function toArray(node: _Node | null): number[] {
    while (node && node.prev) {
        node = node.prev;
    }
    const ans: number[] = [];
    for (; node; node = node.next) {
        ans.push(node.val);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
