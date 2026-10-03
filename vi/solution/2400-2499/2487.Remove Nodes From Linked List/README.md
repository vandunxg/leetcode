---
comments: true
difficulty: Medium
rating: 1454
source: Weekly Contest 321 Q3
tags:
    - Stack
    - Recursion
    - Linked List
    - Monotonic Stack
---

<!-- problem:start -->

# [2487. Remove Nodes From Linked List](https://leetcode.com/problems/remove-nodes-from-linked-list)

[中文文档](/solution/2400-2499/2487.Remove%20Nodes%20From%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>head</code> của một linked list.</p>

<p>Hãy xóa mọi node có một node khác với giá trị lớn hơn ở bất kỳ vị trí nào bên phải nó.</p>

<p>Trả về <em>con trỏ </em><code>head</code><em> của linked list sau khi đã chỉnh sửa.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2487.Remove%20Nodes%20From%20Linked%20List/images/drawio.png" style="width: 631px; height: 51px;" />
<pre>
<strong>Đầu vào:</strong> head = [5,2,13,3,8]
<strong>Đầu ra:</strong> [13,8]
<strong>Giải thích:</strong> Các node cần xóa là 5, 2 và 3.
- Node 13 nằm bên phải node 5.
- Node 13 nằm bên phải node 2.
- Node 8 nằm bên phải node 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> head = [1,1,1,1]
<strong>Đầu ra:</strong> [1,1,1,1]
<strong>Giải thích:</strong> Mọi node đều có giá trị 1, nên không có node nào bị xóa.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li>Số lượng node trong linked list đã cho nằm trong khoảng <code>[1, 10<sup>5</sup>]</code>.</li>
    <li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack (Mảng)

<!-- thinking:start -->

> **Tư duy**
>
> Xóa một node nếu tồn tại một giá trị lớn hơn nó ở bên phải: giữ lại hậu tố giảm dần khi duyệt từ phải sang trái. Vì $n\le 10^5$, ta sao chép các giá trị, duy trì một stack giảm dần, rồi dựng lại danh sách.

<!-- thinking:end -->

Lưu các giá trị của node vào một mảng $nums$, sau đó duyệt $nums$ bằng một stack đơn điệu giảm dần. Pop khi giá trị hiện tại lớn hơn phần tử trên cùng, rồi push giá trị hiện tại vào stack. Dựng lại danh sách từ đáy stack.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của linked list.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNodes(self, head: Optional[ListNode]) -> Optional[ListNode]:
        nums = []
        while head:
            nums.append(head.val)
            head = head.next
        stk = []
        for v in nums:
            while stk and stk[-1] < v:
                stk.pop()
            stk.append(v)
        dummy = ListNode()
        head = dummy
        for v in stk:
            head.next = ListNode(v)
            head = head.next
        return dummy.next
```

#### Java

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNodes(ListNode head) {
        List<Integer> nums = new ArrayList<>();
        while (head != null) {
            nums.add(head.val);
            head = head.next;
        }
        Deque<Integer> stk = new ArrayDeque<>();
        for (int v : nums) {
            while (!stk.isEmpty() && stk.peekLast() < v) {
                stk.pollLast();
            }
            stk.offerLast(v);
        }
        ListNode dummy = new ListNode();
        head = dummy;
        while (!stk.isEmpty()) {
            head.next = new ListNode(stk.pollFirst());
            head = head.next;
        }
        return dummy.next;
    }
}
```

#### C++

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNodes(ListNode* head) {
        vector<int> nums;
        while (head) {
            nums.emplace_back(head->val);
            head = head->next;
        }
        vector<int> stk;
        for (int v : nums) {
            while (!stk.empty() && stk.back() < v) {
                stk.pop_back();
            }
            stk.push_back(v);
        }
        ListNode* dummy = new ListNode();
        head = dummy;
        for (int v : stk) {
            head->next = new ListNode(v);
            head = head->next;
        }
        return dummy->next;
    }
};
```

#### Go

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func removeNodes(head *ListNode) *ListNode {
	nums := []int{}
	for head != nil {
		nums = append(nums, head.Val)
		head = head.Next
	}
	stk := []int{}
	for _, v := range nums {
		for len(stk) > 0 && stk[len(stk)-1] < v {
			stk = stk[:len(stk)-1]
		}
		stk = append(stk, v)
	}
	dummy := &ListNode{}
	head = dummy
	for _, v := range stk {
		head.Next = &ListNode{Val: v}
		head = head.Next
	}
	return dummy.Next
}
```

#### TypeScript

```ts
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     val: number
 *     next: ListNode | null
 *     constructor(val?: number, next?: ListNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.next = (next===undefined ? null : next)
 *     }
 * }
 */

function removeNodes(head: ListNode | null): ListNode | null {
    const nums = [];
    for (; head; head = head.next) {
        nums.push(head.val);
    }
    const stk: number[] = [];
    for (const v of nums) {
        while (stk.length && stk.at(-1)! < v) {
            stk.pop();
        }
        stk.push(v);
    }
    const dummy = new ListNode();
    head = dummy;
    for (const v of stk) {
        head.next = new ListNode(v);
        head = head.next;
    }
    return dummy.next;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Monotonic Stack (Danh sách In-place)

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 cấp phát một mảng giá trị và các node mới. Ta có thể dùng chính stack để lưu các node của linked list: pop khi phần tử trên cùng nhỏ hơn, rồi nối phần tử trên cùng mới với node hiện tại. Một dummy có giá trị $+\infty$ giúp tránh stack rỗng.

<!-- thinking:end -->

Duyệt trực tiếp linked list bằng một stack đơn điệu giảm dần chứa các node. Khi giá trị hiện tại lớn hơn, thực hiện pop; sau đó nối $next$ của phần tử trên cùng mới (hoặc dummy head) với node hiện tại và push node đó vào stack. Trả về $next$ của dummy head.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của linked list.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNodes(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(inf, head)
        cur = head
        stk = [dummy]
        while cur:
            while stk[-1].val < cur.val:
                stk.pop()
            stk[-1].next = cur
            stk.append(cur)
            cur = cur.next
        return dummy.next
```

#### Java

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNodes(ListNode head) {
        ListNode dummy = new ListNode(1 << 30, head);
        Deque<ListNode> stk = new ArrayDeque<>();
        stk.offerLast(dummy);
        for (ListNode cur = head; cur != null; cur = cur.next) {
            while (stk.peekLast().val < cur.val) {
                stk.pollLast();
            }
            stk.peekLast().next = cur;
            stk.offerLast(cur);
        }
        return dummy.next;
    }
}
```

#### C++

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNodes(ListNode* head) {
        ListNode* dummy = new ListNode(1e9, head);
        ListNode* cur = head;
        vector<ListNode*> stk = {dummy};
        for (ListNode* cur = head; cur; cur = cur->next) {
            while (stk.back()->val < cur->val) {
                stk.pop_back();
            }
            stk.back()->next = cur;
            stk.push_back(cur);
        }
        return dummy->next;
    }
};
```

#### Go

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func removeNodes(head *ListNode) *ListNode {
	dummy := &ListNode{1 << 30, head}
	stk := []*ListNode{dummy}
	for cur := head; cur != nil; cur = cur.Next {
		for stk[len(stk)-1].Val < cur.Val {
			stk = stk[:len(stk)-1]
		}
		stk[len(stk)-1].Next = cur
		stk = append(stk, cur)
	}
	return dummy.Next
}
```

#### TypeScript

```ts
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     val: number
 *     next: ListNode | null
 *     constructor(val?: number, next?: ListNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.next = (next===undefined ? null : next)
 *     }
 * }
 */

function removeNodes(head: ListNode | null): ListNode | null {
    const dummy = new ListNode(Infinity, head);
    const stk: ListNode[] = [dummy];
    for (let cur = head; cur; cur = cur.next) {
        while (stk.at(-1)!.val < cur.val) {
            stk.pop();
        }
        stk.at(-1)!.next = cur;
        stk.push(cur);
    }
    return dummy.next;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
