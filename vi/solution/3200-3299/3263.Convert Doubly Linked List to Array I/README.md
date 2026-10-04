---
comments: true
difficulty: Easy
tags:
    - Array
    - Linked List
    - Doubly-Linked List
---

<!-- problem:start -->

# [3263. Convert Doubly Linked List to Array I 🔒](https://leetcode.com/problems/convert-doubly-linked-list-to-array-i)

[中文文档](/solution/3200-3299/3263.Convert%20Doubly%20Linked%20List%20to%20Array%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho trước <code>head</code> của một <strong>danh sách liên kết kép</strong>, trong đó mỗi node có con trỏ next và con trỏ previous.</p>

<p>Trả về một mảng số nguyên chứa các phần tử của danh sách liên kết <strong>theo đúng thứ tự</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">head = [1,2,3,4,3,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,3,4,3,2,1]</span></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">head = [2,2,2,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,2,2,2,2]</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">head = [3,2,3,2,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,2,3,2,3,2]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong danh sách đã cho nằm trong khoảng <code>[1, 50]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Với head của một danh sách liên kết kép, ta xuất các giá trị từ trái sang phải. Danh sách là hữu hạn, nên chỉ cần duyệt theo `next`; `prev` không được sử dụng.
>
> Thêm $\textit{root.val}$ rồi di chuyển đến node tiếp theo cho đến khi gặp null. Độ phức tạp thời gian là tuyến tính, và ngoài mảng kết quả thì cần thêm không gian hằng số.

<!-- thinking:end -->

Ta có thể duyệt trực tiếp danh sách liên kết, lần lượt thêm giá trị của các node vào mảng kết quả $\textit{ans}$.

Sau khi duyệt xong, trả về mảng kết quả $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của danh sách liên kết. Không tính phần không gian của mảng kết quả, độ phức tạp không gian là $O(1)$.

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
    def toArray(self, root: "Optional[Node]") -> List[int]:
        ans = []
        while root:
            ans.append(root.val)
            root = root.next
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
    public int[] toArray(Node head) {
        List<Integer> ans = new ArrayList<>();
        for (; head != null; head = head.next) {
            ans.add(head.val);
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
    vector<int> toArray(Node* head) {
        vector<int> ans;
        for (; head; head = head->next) {
            ans.push_back(head->val);
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

func toArray(head *Node) (ans []int) {
	for ; head != nil; head = head.Next {
		ans = append(ans, head.Val)
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

function toArray(head: _Node | null): number[] {
    const ans: number[] = [];
    for (; head; head = head.next) {
        ans.push(head.val);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
