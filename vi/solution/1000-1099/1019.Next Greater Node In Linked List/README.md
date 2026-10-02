---
comments: true
difficulty: Medium
rating: 1570
source: Weekly Contest 130 Q3
tags:
    - Stack
    - Array
    - Linked List
    - Monotonic Stack
---

<!-- problem:start -->

# [1019. Next Greater Node In Linked List](https://leetcode.com/problems/next-greater-node-in-linked-list)

[中文文档](/solution/1000-1099/1019.Next%20Greater%20Node%20In%20Linked%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>head</code> của một linked list có <code>n</code> node.</p>

<p>Với mỗi node trong danh sách, hãy tìm giá trị của <strong>node lớn hơn tiếp theo</strong>, tức node đầu tiên đứng sau node hiện tại có giá trị <strong>lớn hơn nghiêm ngặt</strong>.</p>

<p>Hãy trả về mảng số nguyên <code>answer</code>, trong đó <code>answer[i]</code> là giá trị của node lớn hơn tiếp theo ứng với node thứ <code>i<sup>th</sup></code> (<strong>đánh chỉ số từ 1</strong>). Nếu node thứ <code>i<sup>th</sup></code> không có node lớn hơn tiếp theo, đặt <code>answer[i] = 0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1019.Next%20Greater%20Node%20In%20Linked%20List/images/linkedlistnext1.jpg" style="width: 304px; height: 133px;" />
<pre>
<strong>Input:</strong> head = [2,1,5]
<strong>Output:</strong> [5,5,0]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1019.Next%20Greater%20Node%20In%20Linked%20List/images/linkedlistnext2.jpg" style="width: 500px; height: 113px;" />
<pre>
<strong>Input:</strong> head = [2,7,4,3,5]
<strong>Output:</strong> [7,0,5,5,0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong danh sách là <code>n</code>.</li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt sang phải từ từng node sẽ tốn $O(n^2)$, quá chậm khi $n\le 10^4$. Đây là bài toán tìm phần tử lớn hơn tiếp theo ở bên phải, tương tự bài toán trên mảng.
>
> Duyệt từ phải sang trái bằng stack giảm dần để lưu các ứng viên. Với mỗi giá trị hiện tại, pop mọi phần tử trên đỉnh không lớn hơn nghiêm ngặt; phần tử mới ở đỉnh, nếu có, chính là giá trị lớn hơn tiếp theo.
>
> Ta chuyển linked list thành mảng rồi điền đáp án bằng một lượt duyệt ngược.

<!-- thinking:end -->

Bài toán yêu cầu tìm node lớn hơn tiếp theo cho từng node trong linked list, tức node đầu tiên ở bên phải có giá trị lớn hơn node đó. Trước tiên, ta duyệt linked list và lưu các giá trị vào mảng $nums$. Với mỗi phần tử trong $nums$, ta cần tìm phần tử đầu tiên bên phải có giá trị lớn hơn nó. Có thể giải bài toán này bằng monotonic stack.

Ta duyệt mảng $nums$ từ cuối về đầu, đồng thời duy trì stack $stk$ có các giá trị giảm dần từ đáy lên đỉnh. Nếu phần tử trên đỉnh nhỏ hơn hoặc bằng phần tử hiện tại, ta liên tục pop cho đến khi đỉnh stack lớn hơn phần tử hiện tại hoặc stack rỗng.

Nếu stack rỗng, phần tử hiện tại không có phần tử lớn hơn tiếp theo. Ngược lại, phần tử trên đỉnh stack là phần tử lớn hơn tiếp theo; ta cập nhật mảng kết quả $ans$. Sau đó push phần tử hiện tại vào stack và tiếp tục duyệt.

Sau khi duyệt xong, ta trả về mảng kết quả $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của linked list.

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def nextLargerNodes(self, head: Optional[ListNode]) -> List[int]:
        nums = []
        while head:
            nums.append(head.val)
            head = head.next
        stk = []
        n = len(nums)
        ans = [0] * n
        for i in range(n - 1, -1, -1):
            while stk and stk[-1] <= nums[i]:
                stk.pop()
            if stk:
                ans[i] = stk[-1]
            stk.append(nums[i])
        return ans
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
    public int[] nextLargerNodes(ListNode head) {
        List<Integer> nums = new ArrayList<>();
        for (; head != null; head = head.next) {
            nums.add(head.val);
        }
        Deque<Integer> stk = new ArrayDeque<>();
        int n = nums.size();
        int[] ans = new int[n];
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && stk.peek() <= nums.get(i)) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                ans[i] = stk.peek();
            }
            stk.push(nums.get(i));
        }
        return ans;
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
    vector<int> nextLargerNodes(ListNode* head) {
        vector<int> nums;
        for (; head; head = head->next) {
            nums.push_back(head->val);
        }
        stack<int> stk;
        int n = nums.size();
        vector<int> ans(n);
        for (int i = n - 1; ~i; --i) {
            while (!stk.empty() && stk.top() <= nums[i]) {
                stk.pop();
            }
            if (!stk.empty()) {
                ans[i] = stk.top();
            }
            stk.push(nums[i]);
        }
        return ans;
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
func nextLargerNodes(head *ListNode) []int {
	nums := []int{}
	for ; head != nil; head = head.Next {
		nums = append(nums, head.Val)
	}
	stk := []int{}
	n := len(nums)
	ans := make([]int, n)
	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 && stk[len(stk)-1] <= nums[i] {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			ans[i] = stk[len(stk)-1]
		}
		stk = append(stk, nums[i])
	}
	return ans
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

function nextLargerNodes(head: ListNode | null): number[] {
    const nums: number[] = [];
    while (head) {
        nums.push(head.val);
        head = head.next;
    }
    const stk: number[] = [];
    const n = nums.length;
    const ans: number[] = Array(n).fill(0);
    for (let i = n - 1; ~i; --i) {
        while (stk.length && stk.at(-1)! <= nums[i]) {
            stk.pop();
        }
        ans[i] = stk.length ? stk.at(-1)! : 0;
        stk.push(nums[i]);
    }
    return ans;
}
```

#### Rust

```rust
// Definition for singly-linked list.
// #[derive(PartialEq, Eq, Clone, Debug)]
// pub struct ListNode {
//   pub val: i32,
//   pub next: Option<Box<ListNode>>
// }
//
// impl ListNode {
//   #[inline]
//   fn new(val: i32) -> Self {
//     ListNode {
//       next: None,
//       val
//     }
//   }
// }
use std::collections::VecDeque;
impl Solution {
    pub fn next_larger_nodes(head: Option<Box<ListNode>>) -> Vec<i32> {
        let mut nums = Vec::new();
        let mut current = &head;
        while let Some(node) = current {
            nums.push(node.val);
            current = &node.next;
        }

        let mut stk = VecDeque::new();
        let n = nums.len();
        let mut ans = vec![0; n];
        for i in (0..n).rev() {
            while !stk.is_empty() && stk.back().copied().unwrap() <= nums[i] {
                stk.pop_back();
            }
            if let Some(&top) = stk.back() {
                ans[i] = top;
            }
            stk.push_back(nums[i]);
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {number[]}
 */
var nextLargerNodes = function (head) {
    const nums = [];
    while (head) {
        nums.push(head.val);
        head = head.next;
    }
    const stk = [];
    const n = nums.length;
    const ans = new Array(n).fill(0);
    for (let i = n - 1; i >= 0; --i) {
        while (stk.length && stk[stk.length - 1] <= nums[i]) {
            stk.pop();
        }
        ans[i] = stk.length ? stk[stk.length - 1] : 0;
        stk.push(nums[i]);
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
